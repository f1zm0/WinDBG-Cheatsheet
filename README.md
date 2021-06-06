# WinDBG Notes

<!-- vim-markdown-toc GFM -->

* [Command Reference](#command-reference)
    * [Registers](#registers)
    * [Memory](#memory)
    * [Breakpoints](#breakpoints)
    * [Tracing](#tracing)
    * [Disassembly](#disassembly)
    * [Modules](#modules)
* [Other resources](#other-resources)

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

| Command    | Function                                    | Example |
| ---------- | ------------------------------------------- | ------- |
| `g` (F5)   | go (or resume execution)                    | -       |
| `p` (F10)  | single step                                 | -       |
| `p <addr>` | step to address                             | -       |
| `pr`       | toggle display of registers after each step | -       |


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



## Other resources

- [defuse.ca / Online Assembler/Disassembler](https://defuse.ca/online-x86-assembler.htm)
