# Integrity Scanner

The integrity scanner validates the hyperion module (RobloxPlayerBeta.dll) page by page. It increments the scanning pointer by exactly 0x1000 (4096 bytes) for each region.

The scanner relies on a dynamically allocated 1MB hash table in the heap. Inside this table, there is exactly one 139-byte (0x8B) structure for every 4KB page.

## The Loop

1. It reads the memory page in 96-byte (0x60) chunks via the r14 pointer (e.g., mov rax, [r14-48], xor rdx,[r14-40]).
2. It performs a series of XOR, IMUL, ROR, and ROL operations with various constants to compute a 64-bit hash in RDX.
3. At the end of the region, it compares the computed hash against the decoded expected hash (cmp [rbp+000006C0],rdx at 0x7FFAAC572124).
4. If they mismatch, it jumps to 0x7FFAAC6072B9, which contains bound and db F1 (icebp) instructions intentionally designed to corrupt the stack and crash the game.

## The Decoding Algorithm

1. NOT the byte.
2. ROL 4 (rotate left by 4 bits) on the byte.

i don't know who this is from and this is old too.
