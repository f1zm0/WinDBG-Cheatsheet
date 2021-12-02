# WinDBG

<!-- vim-markdown-toc GFM -->

* [Command Reference](#command-reference)
    * [Registers](#registers)
    * [Memory](#memory)
    * [Breakpoints](#breakpoints)
    * [Tracing](#tracing)
    * [Disassembly](#disassembly)
    * [Modules](#modules)
* [NTAPI Structures](#ntapi-structures)

<!-- vim-markdown-toc -->

## Command Reference

### Registers

| Command           | Function              | Example     |
| ----------------- | --------------------- | ----------- |
| `r`               | show all registers    | -           |
| `r <reg>,[<reg>]` | show registry content | `r rax,rsp` |
| `r @<reg>=<val>`  | set registry value    | `r @rax=0`  |


### Memory

| Command              | Function                                                         | Example               |
| -------------------- | ---------------------------------------------------------------- | --------------------- |
| `dc <addr> [format]` | dump memory content at specified address                         | `dc 7ffe040d0110 L4`  |
| `e <addr> <val>`     | edit memory at specified address. (1+ values separated by space) | `e $ip a3 b6 c9`      |
| `!vprot <addr>`      | show protection attributes for memory page at specified address  | `!vprot 7ffe040d0110` |



### Breakpoints

| Command | Function                                                           | Example                    |
| ------- | ------------------------------------------------------------------ | -------------------------- |
| `bp`    | set a breakpoint                                                   | `bp kernel32!VirtualAlloc` |
| `bu`    | set unresolved breakpoint (becomes `bp` when the module is loaded) | `bu test!TestFunc`         |
| `bm`    | set breakpoint on module function[s] using pattern                 | `bm wow64!*`               |
| `bc`    | clear all breakpoints                                              | `bc *`                     |


### Tracing

| Command    | Function                                    |
| ---------- | ------------------------------------------- |
| `g` (F5)   | go (or resume execution)                    |
| `p` (F10)  | single step                                 |
| `p <addr>` | step to address                             |
| `pr`       | toggle display of registers after each step |


### Disassembly

| Command         | Function                           | Example                        |
| --------------  | --------------------------------   | ------------------------------ |
| `u <name/addr>` | unassemble                         | `u kernel32!VirtualAlloc+0x4f` |
| `u poi(<addr>)` | unassemble from address at pointer | `u poi(777a9228)`              |
| `uf /o [addr]`  | unassemble function with offsets   | `uf /o amsi!AmsiOpenSession`   |


### Modules

| Command              | Function                                          | Example              |
| -----------------    | ---------------------------------                 | -------------        |
| `lm`                 | list loaded (or deferred) modules                 | -                    |
| `lm m <module>`      | check if a module is loaded                       | `lm m amsi`          |
| `sxe ld <module>`    | break when a module is loaded                     | `sxe ld amsi`        |
| `x <module>!<regex>` | show functions exported by the module (reads EAT) | `x ntdll!*Allocate*` |


## NTAPI Structures


| Command                                               | Function                                                            | Example                                                       |
| -----------------                                     | ---------------------------------                                   | -------------                                                 |
| `r $teb`                                              | display TEB base address                                            | -                                                             |
| `r $peb`                                              | display PEB base address                                            | -                                                             |
| `dt ntdll!_PEB @$peb`                                 | display type `ntdll!_PEB` starting from address stored in `$peb`    | -                                                             |
| `dt ntdll!_PEB @$peb <struct>-><pointed struct>->...` | display sub structures starting from PEB                            | `dt ntdll!_PEB @$peb Ldr->InMemoryOrderModuleList`            |
| `!list -x  "dt <type> <attribute[s]>" <base_addr>`    | use link extension to traverse linked list starting at base address | `!list -x "dt _LDR_DATA_TABLE_ENTRY BaseDllName" 0x0001ed...` |

