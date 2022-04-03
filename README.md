# WinDBG Cheatsheet


<!-- vim-markdown-toc GFM -->

* [Command Reference](#command-reference)
    * [Registers](#registers)
    * [Memory](#memory)
    * [Strings](#strings)
    * [Breakpoints](#breakpoints)
    * [Tracing](#tracing)
    * [Disassembly](#disassembly)
    * [Modules](#modules)
* [NTAPI Structures](#ntapi-structures)

<!-- vim-markdown-toc -->

## Command Reference

### Registers

| Function              | Command           | Examples    |
| --------------------- | ----------------- | ----------- |
| show all registers    | `r`               | -           |
| show registry content | `r <reg>,[<reg>]` | `r rax,rsp` |
| set registry value    | `r @<reg>=<val>`  | `r @rax=0`  |


### Memory

| Function                                    | Command                       | Type / Size                                                                | Examples             |
| ------------------------------------------- | --------------------          | ---------------------                                                      | --------------       |
| display memory at address                   | `d* <addr> [format]`          | bytes: `db`<br>words:`dw`<br>dwords: `dd`<br>qwords: `dq`<br>pointer: `dp` | `db @rax L4` |
| edit memory at address                      | `e* <addr> <val> [<val> ...]` | bytes: `eb`<br>word: `ew`<br>dword: `ed`<br>qword: `eq`<br>pointer `ep`    | `eb @ip a3 b6 c9`    |
| show protection attributes for memory page  | `!vprot <addr>`               |                                                                            |                      |
| dereference memory at address               | `d* poi(<addr>)`              |                                                                            | `dq poi(@rax)`       |

### Strings

| Function                                    | Command                       | Type / Size                 | Examples                 |
| ------------------------------------------- | --------------------          | ---------------------       | --------                 |
| display string at address                   | `d* <addr>`                   | ascii: `da`<br>unicode:`du` | `da 7ffe040d0110`        |
| edit string at address                      | `e* <addr> <val> [<val> ...]` | ascii: `ea`<br>unicode:`eu` | `ea 7ffe040d0110 "AAAA"` |

### Breakpoints

| Function                                                           | Command | Examples                    |
| ------------------------------------------------------------------ | ------- | -------------------------- |
| set a breakpoint                                                   | `bp`    | `bp kernel32!VirtualAlloc` |
| set unresolved breakpoint (becomes `bp` when the module is loaded) | `bu`    | `bu test!TestFunc`         |
| set breakpoint on module function[s] using pattern                 | `bm`    | `bm wow64!*`               |
| clear all breakpoints                                              | `bc`    | `bc *`                     |


### Tracing

| Function                                    | Command    |
| ------------------------------------------- | ---------- |
| go (or resume execution)                    | `g` (F5)   |
| single step                                 | `p` (F10)  |
| step to address                             | `p <addr>` |
| toggle display of registers after each step | `pr`       |


### Disassembly

| Function                           | Command         | Examples                        |
| --------------------------------   | --------------  | ------------------------------ |
| unassemble                         | `u <name/addr>` | `u kernel32!VirtualAlloc+0x4f` |
| unassemble from address at pointer | `u poi(<addr>)` | `u poi(777a9228)`              |
| unassemble function with offsets   | `uf /o [addr]`  | `uf /o amsi!AmsiOpenSession`   |


### Modules

| Function                                          | Command              | Examples              |
| ---------------------------------                 | -----------------    | -------------        |
| list loaded (or deferred) modules                 | `lm`                 | -                    |
| check if a module is loaded                       | `lm m <module>`      | `lm m amsi`          |
| break when a module is loaded                     | `sxe ld <module>`    | `sxe ld amsi`        |
| show functions exported by the module (reads EAT) | `x <module>!<regex>` | `x ntdll!*Allocate*` |


## NTAPI Structures

| Command                                               | Function                                                            | Examples                                                       |
| -----------------                                     | ---------------------------------                                   | -------------                                                 |
| `r $teb`                                              | display TEB base address                                            | -                                                             |
| `r $peb`                                              | display PEB base address                                            | -                                                             |
| `dt ntdll!_PEB @$peb`                                 | display type `ntdll!_PEB` starting from address stored in `$peb`    | -                                                             |
| `dt ntdll!_PEB @$peb <struct>-><pointed struct>->...` | display sub structures starting from PEB                            | `dt ntdll!_PEB @$peb Ldr->InMemoryOrderModuleList`            |
| `!list -x  "dt <type> <attribute[s]>" <base_addr>`    | use link extension to traverse linked list starting at base address | `!list -x "dt _LDR_DATA_TABLE_ENTRY BaseDllName" 0x0001ed...` |

