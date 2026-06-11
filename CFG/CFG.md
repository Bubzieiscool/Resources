# Control Flow Guard (CFG)

## Method 1 — Current

Search for the constant `3F800000h`, then go through its xrefs. You'll find `whitelistedpages` and `setinsert` — the logic of insertion has changed, so it doesn't work anymore; you'll need to recreate it.

- `0x11b1ea0` is the whitelist
- The function it's used in is `setinsert`
- The bitmap constant is `0x7FFFFFFFFFFF` (or close to it — check the xrefs)

**Offsets:**

| Symbol              | Offset     |
| ------------------- | ---------- |
| ControlFlowGuard    | `0xF9BCE0` |
| BitMap              | `0x3C548`  |

> Credits: unknown

---

## Method 2 — Older

1. Open `robloxplayerbeta.dll` in Binary Ninja
2. Rebase to `0x0`
3. `Edit → Find → Find type → Constant Disassembly`
4. Search for `0x7fffffffffff`
5. Press search and it will take you to the function

### Disassembly

```asm
; ControlFlowGuard
0x4a6c40    int64_t sub_4a6c40(void* arg1 @ rax)

0x4a6c40  49bbffffffffff7f…  mov     r11, 0x7fffffffffff
0x4a6c4a  4c39d8             cmp     rax, r11
0x4a6c4d  7726               ja      0x4a6c75

0x4a6c4f  4c8b1d2aa82301     mov     r11, qword [rel sub_16e1480]  ; 0x16e1480 = BitMap
0x4a6c56  4989c2             mov     r10, rax
0x4a6c59  49c1ea0f           shr     r10, 0xf                      ; ByteShift = 15
0x4a6c5d  4f0fb61c13         movzx   r11, byte [r11+r10]
0x4a6c62  4989c2             mov     r10, rax
0x4a6c65  49c1ea0c           shr     r10, 0xc                      ; PageShift = 12
0x4a6c69  4983e207           and     r10, 0x7                      ; BitMask = 7
0x4a6c6d  4d0fa3d3           bt      r11, r10
0x4a6c71  7302               jae     0x4a6c75
0x4a6c73  ffe0               jmp     rax
0x4a6c75  e946ffffff         jmp     sub_4a6bc0
```

> Notes:
> - `shr r10, 0xc` = 12 → shifting right by 12 bits means `address >> 12`, which is the same as `address / 2^12 = 4096 = 0x1000` → **PageSize**
> - `PageMask = PageSize - 1 = 0x1000 - 1 = 4095 = 0xfff`

### C++ Offsets

```cpp
namespace Reversal
{
    static const uintptr_t ControlFlowGuard = 0x4a6c40;
    static const uintptr_t BitMap = 0x16e1480;

    enum Offsets {
        ByteShift = 15,   // 0xf
        PageShift = 12,   // 0xc
        BitMask   = 7,    // 0x7
        PageSize  = 0x1000, // 4096
        PageMask  = 0xfff   // 4095
    };
}
```

> Credits: tioiscool_4
