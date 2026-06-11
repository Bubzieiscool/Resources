# Hyperion Hooks

```
[hook] #1 KiRaiseUserExceptionDispatcher [.text] -> 0x7FFD756B0A95
       RVA: 0x00165B10 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [48][81][EC][C8][00] 00  00  89  84  24  C0  00  00  00  65  8B
       MEM : [E9][80][AF][E8][FF] 00  00  89  84  24  C0  00  00  00  65  8B

[hook] #2 KiUserApcDispatcher [.text] -> 0x7FFD756B0F93
       RVA: 0x00165930 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [48][8B][4C][24][18] 48  8B  C1  4C  8B  CC  48  C1  F9  02  48
       MEM : [E9][5E][B6][E8][FF] 48  8B  C1  4C  8B  CC  48  C1  F9  02  48

[hook] #3 KiUserCallbackDispatcher [.text] -> 0x7FFD756B0F13
       RVA: 0x00165A50 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [48][8B][4C][24][20] 8B  54  24  28  44  8B  44  24  2C  65  48
       MEM : [E9][BE][B4][E8][FF] 8B  54  24  28  44  8B  44  24  2C  65  48

[hook] #4 KiUserExceptionDispatcher [.text] -> 0x7FFD756B0FD6
       RVA: 0x00165AA0 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [FC][48][8B][05][40] 18  08  00  48  85  C0  74  0F  48  8B  CC
       MEM : [E9][31][B5][E8][FF] 18  08  00  48  85  C0  74  0F  48  8B  CC

[hook] #5 LdrInitializeThunk [.text] -> 0x7FFD756B0F54
       RVA: 0x000EB0A0 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [40][53][48][83][EC] 20  48  8B  D9  E8  1A  00  00  00  B2  01
       MEM : [E9][AF][5E][F0][FF] 20  48  8B  D9  E8  1A  00  00  00  B2  01

[hook] #6 NtAllocateVirtualMemory [.text] -> 0x7FFD756B0D96
       RVA: 0x00161D40 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [4C][8B][D1][B8][18] 00  00  00  F6  04  25  08  03  FE  7F  01
       MEM : [E9][51][F0][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #7 NtAllocateVirtualMemoryEx [.text] -> 0x7FFD756B0D56
       RVA: 0x00162930 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [4C][8B][D1][B8][78] 00  00  00  F6  04  25  08  03  FE  7F  01
       MEM : [E9][21][E4][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #8 NtContinue [.text] -> 0x7FFD756B0B96
       RVA: 0x001622A0 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [4C][8B][D1][B8][43] 00  00  00  F6  04  25  08  03  FE  7F  01
       MEM : [E9][F1][E8][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #9 NtContinueEx [.text] -> 0x7FFD756B0B56
       RVA: 0x00162ED0 | Type: Inline JMP | Changed: 5 at +0x0
       DISK: [4C][8B][D1][B8][A5] 00  00  00  F6  04  25  08  03  FE  7F  01
       MEM : [E9][81][DC][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #10 NtCreateSection [.text] -> 0x7FFD756B0E56
        RVA: 0x00162380 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][4A] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][EA][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #11 NtCreateSectionEx [.text] -> 0x7FFD756B0E16
        RVA: 0x001632F0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][C6] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][21][DB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #12 NtCreateThread [.text] -> 0x7FFD756B0ED6
        RVA: 0x00162400 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][4E] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][EA][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #13 NtCreateThreadEx [.text] -> 0x7FFD756B0E96
        RVA: 0x00163350 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][C9] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][41][DB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #14 NtFreeVirtualMemory [.text] -> 0x7FFD756B0D16
        RVA: 0x00161E00 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][1E] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][11][EF][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #15 NtMapViewOfSection [.text] -> 0x7FFD756B0C96
        RVA: 0x00161F40 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][28] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][ED][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #16 NtMapViewOfSectionEx [.text] -> 0x7FFD756B0C56
        RVA: 0x00163DF0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][1E] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][61][CE][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #17 NtProtectVirtualMemory [.text] -> 0x7FFD756B0CD6
        RVA: 0x00162440 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][50] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][91][E8][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #18 NtQuerySystemInformation [.text] -> 0x7FFD756B0A56
        RVA: 0x00162100 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][36] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][E9][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #19 NtQueryVirtualMemory [.text] -> 0x7FFD756B0A16
        RVA: 0x00161EA0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][23] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][71][EB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #20 NtRaiseException [.text] -> 0x7FFD756B0956
        RVA: 0x001648B0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][74] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][A1][C0][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #21 NtRaiseHardError [.text] -> 0x7FFD756B0DD6
        RVA: 0x001648D0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][75] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][01][C5][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #22 NtSetContextThread [.text] -> 0x7FFD756B0B16
        RVA: 0x00164D70 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][9A] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][A1][BD][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #23 NtSuspendThread [.text] -> 0x7FFD756B0AD6
        RVA: 0x00165410 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][CF] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][C1][B6][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #24 NtTerminateProcess [.text] -> 0x7FFD756B0996
        RVA: 0x00161FC0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][2C] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][E9][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #25 NtTerminateThread [.text] -> 0x7FFD756B09D6
        RVA: 0x001624A0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][53] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][31][E5][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #26 NtUnmapViewOfSection [.text] -> 0x7FFD756B0C16
        RVA: 0x00161F80 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][2A] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][91][EC][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #27 NtUnmapViewOfSectionEx [.text] -> 0x7FFD756B0BD6
        RVA: 0x00165610 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][DF] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][C1][B5][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #28 RtlExitUserProcess [.text] -> 0x7FFD756B0914
        RVA: 0x0008C4B0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [40][53][48][83][EC] 20  8B  D9  E8  2B  0A  00  00  65  48  8B
        MEM : [E9][5F][44][F6][FF] 20  8B  D9  E8  2B  0A  00  00  65  48  8B

[hook] #29 RtlGetNativeSystemInformation [.text] -> 0x7FFD756B0A56
        RVA: 0x00162100 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][36] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][E9][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #30 ZwAllocateVirtualMemory [.text] -> 0x7FFD756B0D96
        RVA: 0x00161D40 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][18] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][F0][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #31 ZwAllocateVirtualMemoryEx [.text] -> 0x7FFD756B0D56
        RVA: 0x00162930 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][78] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][21][E4][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #32 ZwContinue [.text] -> 0x7FFD756B0B96
        RVA: 0x001622A0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][43] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][F1][E8][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #33 ZwContinueEx [.text] -> 0x7FFD756B0B56
        RVA: 0x00162ED0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][A5] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][81][DC][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #34 ZwCreateSection [.text] -> 0x7FFD756B0E56
        RVA: 0x00162380 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][4A] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][EA][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #35 ZwCreateSectionEx [.text] -> 0x7FFD756B0E16
        RVA: 0x001632F0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][C6] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][21][DB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #36 ZwCreateThread [.text] -> 0x7FFD756B0ED6
        RVA: 0x00162400 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][4E] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][EA][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #37 ZwCreateThreadEx [.text] -> 0x7FFD756B0E96
        RVA: 0x00163350 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][C9] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][41][DB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #38 ZwFreeVirtualMemory [.text] -> 0x7FFD756B0D16
        RVA: 0x00161E00 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][1E] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][11][EF][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #39 ZwMapViewOfSection [.text] -> 0x7FFD756B0C96
        RVA: 0x00161F40 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][28] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][ED][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #40 ZwMapViewOfSectionEx [.text] -> 0x7FFD756B0C56
        RVA: 0x00163DF0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][1E] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][61][CE][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #41 ZwProtectVirtualMemory [.text] -> 0x7FFD756B0CD6
        RVA: 0x00162440 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][50] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][91][E8][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #42 ZwQuerySystemInformation [.text] -> 0x7FFD756B0A56
        RVA: 0x00162100 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][36] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][51][E9][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #43 ZwQueryVirtualMemory [.text] -> 0x7FFD756B0A16
        RVA: 0x00161EA0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][23] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][71][EB][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #44 ZwRaiseException [.text] -> 0x7FFD756B0956
        RVA: 0x001648B0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][74] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][A1][C0][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #45 ZwRaiseHardError [.text] -> 0x7FFD756B0DD6
        RVA: 0x001648D0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][75] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][01][C5][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #46 ZwSetContextThread [.text] -> 0x7FFD756B0B16
        RVA: 0x00164D70 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][9A] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][A1][BD][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #47 ZwSuspendThread [.text] -> 0x7FFD756B0AD6
        RVA: 0x00165410 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][CF] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][C1][B6][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01

[hook] #48 ZwTerminateProcess [.text] -> 0x7FFD756B0996
        RVA: 0x00161FC0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][2C] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][D1][E9][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #49 ZwTerminateThread [.text] -> 0x7FFD756B09D6
        RVA: 0x001624A0 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][53] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][31][E5][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #50 ZwUnmapViewOfSection [.text] -> 0x7FFD756B0C16
        RVA: 0x00161F80 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][2A] 00  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][91][EC][E8][FF] 00  00  00  F6  04  25  08  03  FE  7F  01

[hook] #51 ZwUnmapViewOfSectionEx [.text] -> 0x7FFD756B0BD6
        RVA: 0x00165610 | Type: Inline JMP | Changed: 5 at +0x0
        DISK: [4C][8B][D1][B8][DF] 01  00  00  F6  04  25  08  03  FE  7F  01
        MEM : [E9][C1][B5][E8][FF] 01  00  00  F6  04  25  08  03  FE  7F  01
```

this is old and idk who its from.
