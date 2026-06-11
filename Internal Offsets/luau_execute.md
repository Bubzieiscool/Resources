# Finding `luau_execute`

> By ziadweam8

## Steps

1. Search for the string `"get length of"`
2. Get the xref. For the current version, the top xref is:

```
Up    o    sub_4277DD0:loc_42827B8    lea     rax, aGetLengthOf; "get length of"
```

3. Copy the function name (`sub_4277DD0`) and go to it.
4. Open the xref list — you'll see many references:

| Direction | Type | Address              | Text                 |
|-----------|------|----------------------|----------------------|
| Up        | p    | sub_1BDF430+AA1     | call sub_4277DD0    |
| Up        | p    | sub_1C7CE40+6FE     | call sub_4277DD0    |
| Up        | p    | sub_33FC280+6BF     | call sub_4277DD0    |
| Up        | p    | sub_33FF3E0+81D     | call sub_4277DD0    |
| Up        | p    | sub_425EE90+87      | call sub_4277DD0    |
| Up        | p    | sub_4260A80+95E     | call sub_4277DD0    |
| Up        | p    | sub_42666D0+75F     | call sub_4277DD0    |
| **Up**    | **j** | **sub_4277D20+4**  | **jnz sub_4277DD0** |
| Down      | p    | sub_4297EE0+69B     | call sub_4277DD0    |
| Down      | p    | sub_4297EE0+DAF     | call sub_4277DD0    |
| Down      | p    | sub_42996D0+65E     | call sub_4277DD0    |
| Down      | p    | sub_42AFEE0+6CF     | call sub_4277DD0    |
| Down      | p    | sub_42BA0A0+98E     | call sub_4277DD0    |
| Down      | p    | sub_42BAC00+990     | call sub_4277DD0    |
| Down      | p    | sub_42D0A80+CB3     | call sub_4277DD0    |

5. The one you want is the `jnz` reference:

```
Up    j    sub_4277D20+4    jnz     sub_4277DD0
```

6. Go to that reference and you'll find:

```asm
.text:0000000004277D20 ; =============== S U B R O U T I N E =======================================
.text:0000000004277D20
.text:0000000004277D20 ; int64 fastcall sub_4277D20(__int64)
.text:0000000004277D20 sub_4277D20     proc near               ; CODE XREF: sub_4277DD0+881E↓p
.text:0000000004277D20                                         ; sub_4277DD0+92CF↓p ...
.text:0000000004277D20                 cmp     byte ptr [rcx+5], 0
.text:0000000004277D24                 jnz     sub_4277DD0
.text:0000000004277D2A                 jmp     sub_4282D20
.text:0000000004277D2A sub_4277D20     endp
```

**`luau_execute` is `0x4277D20`**

> ⚠️ This offset is outdated/old and may not work on the current version.

