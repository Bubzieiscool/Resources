# Roblox Hyperion (Byfron) DLL - Full Security Mechanism Analysis

credits to: diwnxss (this is old too)

## Binary Overview

| Property | Value |
| --- | --- |
| Identity | Roblox Hyperion (Byfron) anti-cheat DLL |
| Architecture | x64 PE DLL |
| Load base | 0x7FFA3D960000 |
| Code section | .byfron (RX, 13.4 MB, 0x7FFA3DB40000-0x7FFA3E8B0000) |
| IDA-recognized functions | 502 (in .byfron) |
| Hidden code | ~2 MB unanalyzed gap at 0x7FFA3DB64C1E-0x7FFA3DD66270 |

## Memory Layout

| Section | Range | Size | Perms | Purpose |
| --- | --- | --- | --- | --- |
| _data | 3D960000-3D9A0000 | 256 KB | RW | Runtime state, shellcode pointer tables |
| _rdata (1) | 3D9A0000-3DB19B90 | 1.5 MB | R | Constants: ChaCha20 sigma, SHA-256 H4-H7, key material, RDTSC shellcode blobs |
| _idata | 3DB19B90-3DB19E50 | 704 B | R | Minimal import table |
| _rdata (2) | 3DB19E50-3DB40000 | 152 KB | R | Dynamic API name strings |
| _byfron | 3DB40000-3E8B0000 | 13.4 MB | RX | All visible functions + 2MB hidden ChaCha20/check code |

## 1. ChaCha20 - Location, Parameters, and Usage

### Sigma Constant

Address: 0x7FFA3D9BDBF0 (.rdata)
```
65 78 70 61 6e 64 20 33 32 2d 62 79 74 65 20 6b
```
= "expand 32-byte k"   (ChaCha20-256 standard sigma)

### Key Material Candidates (.rdata context around sigma)

```
0x7FFA3D9BDBB0:  7F 52 0E 51  8C 68 05 9B  AB D9 83 1F  19 CD E0 5B
                 = 0x510E527F  0x9B05688C   0x1F83D9AB   0x5BE0CD19
                 (SHA-256 H4-H7 initial values used as nonce/key seed)

0x7FFA3D9BDC00:  D2 2C 41 A6  FF FF FF FF   (repeated QWORD - likely obfuscated pointer)
0x7FFA3D9BDC10:  03 00 00 00 00 00 00 00   (block counter = 3)
0x7FFA3D9BDC20:  E0 A1 24 AD  00 00 00 00   (possible nonce word 0)
0x7FFA3D9BDC28:  3B 11 4D 3A  02 00 00 00   (possible nonce word 1/counter)
```

### ChaCha20 Implementation Location

Physical location: ~0x7FFA3DC22E00 (inside the 2 MB unanalyzed code gap - IDA could not define function boundaries here due to obfuscated entry dispatch)

Confirmation - all four ChaCha20 rotation constants present in tight cluster:

| Rotation | Pattern | Example Address |
| --- | --- | --- |
| ROL 16 (d ^= a; d <<<= 16) | 41 C1 C3 10 | 0x7FFA3DC22F7C |
| ROL 12 (b ^= c; b <<<= 12) | C1 C0 0C | 0x7FFA3DC22F05 |
| ROL 8 (d ^= a; d <<<= 8) | C1 C7 08 | 0x7FFA3DC22FA8 |
| ROL 7 (b ^= c; b <<<= 7) | C1 C6 07 | 0x7FFA3DC22FE1 |

Raw quarter-round assembly (decoded from 0x7FFA3DC22EE0):

```asm
; ChaCha20 quarter round (scalar + SSE2 interleaved for multi-block)
XOR  EDI, R13D          ; d ^= a
ROL  R11D, 16           ; d <<<= 16  [step 1]
ROL  EDI, 8             ; d <<<= 8   [step 3 of previous column]
ROL  R9D, 16
...
MOV  EAX, [rsp+0x1780]  ; load state word
ADD  ECX, EAX           ; c += d
XOR  EAX, ECX
ROL  EAX, 12            ; b <<<= 12  [step 2]
...
ROL  ESI, 7             ; b <<<= 7   [step 4]
...
; SSE2 parallel path (4 states simultaneously):
MOVD   XMM0, EBX        ; 66 0F 6E C3
PXOR   XMM0, XMM1       ; 66 0F EF C1  (d ^= a)
PADDD  XMM1, XMM5       ; 66 0F FE CD  (a += b)
MOVD   ECX, XMM1        ; 66 41 0F 7E C9
```

### How ChaCha20 Is Used

- **Code-blob decryption at startup**: The 2 MB hidden code region and data section blobs (stored encrypted at rest) are ChaCha20-decrypted into executable memory. The pointer table at 0x7FFA3D979A00 is populated at runtime with function pointers to the decrypted check routines.
- **Runtime integrity tokens**: ChaCha20 generates a keystream block using the current loaded-code checksum as the key, then compares the XOR output against expected values embedded in .rdata. If the code was patched, the decrypted output won't match.
- **Key derivation**: SHA-256 H4-H7 (0x510E527F, 0x9B05688C, 0x1F83D9AB, 0x5BE0CD19) adjacent to the sigma seed the 256-bit key via a KDF (likely the custom hash seen in sub_7FFA3DD73820).

## 2. Anti-Tamper Protection

### Custom 64-bit Hash (Integrity Fingerprinting)

Function: sub_7FFA3DD73820 (offset 0x7FFA3DD73820, size 5347 bytes)

This function implements a non-standard 64-bit hash using the constant 0x76006FB6BDD871D9 with rotating XOR chains:
```
v19 = 0x76006FB6BDD871D9ULL * v18;
hash = ROL8(v19, 24) ^ ROL8(v19, 11);
hash2 = ROL8(hash, 22) ^ ROL8(hash, 48);
hash3 = ROL8(hash2, 44) ^ ROL8(hash2, 32) ^ hash2;
```
It hashes the DLL's own .byfron code section page-by-page and compares against ChaCha20-decrypted reference digests stored in .rdata.

Calls: sub_7FFA3E37C970 - the optimized AVX2/SSE2 memcpy used to compare sections.

## 3. Integrity Checks

### SHA-256 Checksumming

SHA-256 initial hash values H4-H7 (0x7FFA3D9BDBB0) are used directly, implying SHA-256 computes digests of:
- Roblox game module .text segments
- Hyperion's own encrypted blobs (before decryption, to prevent swap attacks)

### Stack Cookie (GS) Verification

Standard MSVC GS cookie chain present:

| Function | Address | Role |
| --- | --- | --- |
| __GSHandlerCheck | 0x7FFA3E1EFD14 | Per-frame cookie verification in SEH |
| __GSHandlerCheckCommon | 0x7FFA3E397D7C | Common cookie check dispatcher |
| __GSHandlerCheck_SEH | 0x7FFA3E602748 | SEH-specific cookie guard |
| __report_gsfailure | 0x7FFA3E15351C | Triggered on mismatch, kills process |
| __raise_securityfailure | 0x7FFA3E054214 | Hard abort path |
| __report_securityfailure | 0x7FFA3E888CA0 | Logged security failure |

## 4. Control Flow Guard (CFG) and Obfuscation

### Internal JMP-Stub Redirectors (8 detected)

Byfron installs 8 internal JMP rel32 hooks within .byfron itself to confuse xref analysis:

| Stub | Address | Redirects To | Real Target |
| --- | --- | --- | --- |
| sub_7FFA3DFEF138 | 0x7FFA3DFEF138 | 0x7FFA3E05731C | sub_7FFA3E05731C (117B) |
| sub_7FFA3E0701A0 | 0x7FFA3E0701A0 | 0x7FFA3E7EBD60 | sub_7FFA3E7EBD60 |
| sub_7FFA3E0944C8 | 0x7FFA3E0944C8 | 0x7FFA3E06FAB8 | sub_7FFA3E06FAB8 (169B) |
| sub_7FFA3E1E9D74 | 0x7FFA3E1E9D74 | 0x7FFA3E07D298 | sub_7FFA3E07D298 (115B) |
| sub_7FFA3E5D45C4 | 0x7FFA3E5D45C4 | 0x7FFA3DB649E8 | C++ exception dispatch |
| sub_7FFA3E670520 | 0x7FFA3E670520 | 0x7FFA3E3B7A48 | sub_7FFA3E3B7A48 (94B) |
| sub_7FFA3E7EBD60 | 0x7FFA3E7EBD60 | 0x7FFA3E6802B8 | sub_7FFA3E6802B8 (17B) |
| sub_7FFA3E879CE8 | 0x7FFA3E879CE8 | 0x7FFA3E06DCBC | sub_7FFA3E06DCBC (577B) |

Pattern: each stub is 5 bytes (E9 xx xx xx xx JMP) followed by garbage bytes designed to look like valid code (sub rsp, push, etc.) to deceive disassemblers.

### VM Dispatcher

Function: sub_7FFA3DD74D60 (75,002 bytes) - the main VM interpreter loop. It:
- Uses a 75KB jump-table dispatch at byte_7FFA3E81CED5[ROL4(-1985211324,5) * opcode]
- References unk_7FFA3D968200 (runtime state) at offset +330592/+330608 for VTable lookups
- Has 1,079 XOR instructions and 327 ROL instructions for opcode operand obfuscation
- Contains a single real call to sub_7FFA3E37C970 (the memcpy), all other dispatch is indirect

## 5. Anti-Debug / Anti-Analysis

### Dynamic API Resolution

All sensitive APIs are resolved at runtime from strings in .rdata (0x7FFA3DB19E50 area):

| API | Address of String | Purpose |
| --- | --- | --- |
| IsDebuggerPresent | 0x7FFA3DB2EE2E | Basic debugger flag check |
| QueryPerformanceCounter | 0x7FFA3DB2EEC0 | Timing delta check |
| QueryPerformanceFrequency | 0x7FFA3DB2EEDA | Calibrate timing baseline |
| GetTickCount64 | 0x7FFA3DB2ED94 | Low-res timing sanity |
| NtTerminateProcess | 0x7FFA3DB2EA52 | Self-termination on detection |
| IsProcessorFeaturePresent | 0x7FFA3DB2EE42 | CPU feature / hypervisor check |
| SetUnhandledExceptionFilter | 0x7FFA3DB2EF56 | Hook exception handler |

### Timing-Based Anti-Debug (RDTSC Blobs)

Location: 0x7FFA3DA6FF60-0x7FFA3DA70780 in .rdata - a 14-entry structured array, each entry ~0x88 bytes, each containing an embedded 0F 31 (RDTSC) opcode byte within a data descriptor structure. These are shellcode templates that Byfron's decryptor maps to executable memory at runtime.

The check:
```
rdtsc_before = RDTSC();
// ... execute small code snippet ...
rdtsc_after = RDTSC();
delta = rdtsc_after - rdtsc_before;
if (delta > threshold)  // debugger slows execution
    terminate();
```
Fourteen separate RDTSC timing probes covering different code paths (likely each major check routine is timed independently).

### CPUID / VM Detection

Location: 0x7FFA3D979A64 in .data - a pointer table entry. The value 0x0E5DA20F at runtime becomes a pointer to code starting with 0F A2 (CPUID). CPUID leaf checks used:
- Leaf 0x1: ECX bit 31 (HYPERVISOR flag) - detects generic VMs
- Leaf 0x40000000: Hypervisor vendor string ("KVMKVMKVM", "Microsoft Hv", "VMwareVMware", etc.)

## 6. Key/Nonce Derivation and Storage

### ChaCha20 Key Setup (0x7FFA3D9BDBF0 region)

Structure in .rdata at 0x7FFA3D9BDB80:
```
+0x00  [16B] 0x02 * 16                  (size-class table entry 2)
+0x10  [16B] 0x04 * 16                  (size-class table entry 4)
+0x20  [16B] 0x3F4F1C41 x 2 (+0xFF padding)  (obfuscated constant)
+0x30  [16B] 7F 52 0E 51  8C 68 05 9B  AB D9 83 1F  19 CD E0 5B
              = SHA256_H4-H7 (seed for key or nonce)
+0x50  [16B] "expand 32-byte k"         (ChaCha20 sigma)
+0x60  [8B]  0xFFFFFFFFA6412CD2         (possible encrypted key word)
+0x70  [8B]  0x03 (block counter)
+0x80  [8B]  0xAD24A1E0                 (nonce word 0)
+0x90  [8B]  0x000000023A4D113B         (nonce word 1)
```

Key derivation flow (inferred):
1. At DLL load, sha256(module_base || load_timestamp || machine_fingerprint) -> 32 bytes
2. First 16 bytes -> ChaCha20 key[0..3]
3. SHA-256 H4-H7 constants XOR'd with machine-specific seed -> key[4..7]
4. Block counter starts at 3 (first 3 blocks used for internal state setup)
5. Nonce (0xAD24A1E0 || 0x3A4D113B || 0x00000002) is partially embedded in .rdata, partially derived at runtime

The key is never stored in plaintext - the surrounding obfuscated words (0xA6412CD2, 0xFFFFFFFF markers) are deobfuscated by the VM at runtime before being passed to the ChaCha20 block function.

## 7. Relevant Function Summary

| Category | Function | Offset | Size | Description |
| --- | --- | --- | --- | --- |
| ChaCha20 core | (unnamed) | 0x7FFA3DC22E00 | ~8 KB | Scalar + SSE2 ChaCha20 block function (in unanalyzed gap) |
| VM dispatcher | sub_7FFA3DD74D60 | +0x1B4D60 | 75002 B | Main VM interpreter - orchestrates all checks |
| Integrity hash | sub_7FFA3DD73820 | +0x1B3820 | 5347 B | Custom ROL8/XOR64 hash of code sections |
| Hash/SM4-like | sub_7FFA3DD8BFE0 | +0x1CBFE0 | 7558 B | Second integrity hash (64-bit rotation pattern) |
| Optimized memcpy | sub_7FFA3E37C970 | +0x3B9970 | 1645 B | AVX2/SSE memcpy (used in comparisons) |
| Exception dispatch | sub_7FFA3DB649E8 | +0x49E8 | 566 B | C++ SEH/CXX exception handler |
| GS cookie | __GSHandlerCheck | 0x7FFA3E1EFD14 | 29 B | Stack cookie per-frame guard |
| GS failure | __report_gsfailure | 0x7FFA3E15351C | 211 B | Abort on cookie mismatch |
| Kill path | sub_7FFA3E298ED4 | 0x7FFA3E298ED4 | 86 B | Called on detection, terminates process |

## 8. Bypass / Evasion Considerations

| Mechanism | Bypass Difficulty | Notes |
| --- | --- | --- |
| IsDebuggerPresent | Easy | Patch PEB.BeingDebugged = 0, or hook API return 0 |
| RDTSC timing | Medium | Patch the threshold comparison or hook RDTSC via TSC_DEADLINE manipulation |
| ChaCha20 integrity | Hard | Key is derived at runtime; re-encrypting patched sections requires duplicating the key derivation |
| VM interpreter checks | Very Hard | 75KB obfuscated interpreter; each opcode must be traced individually |
| GS cookie | Hard | Any stack overflow attempt will trigger; bypass requires patching the check itself |
| SHA-256 digest | Hard | Would need to find all reference digest locations in .rdata and update them |
| CPUID VM detection | Easy | Patch CPUID hypervisor bit check or spoof CPUID output via cpuid interception |
| JMP stubs | Easy (once found) | 8 identified; follow real target in analysis |
