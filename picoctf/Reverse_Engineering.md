# 1. GDB baby step 1
> Figure out what is in the eax register at the end of the main function and put the answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. 

## Solution:
- Download the file and use gdb command to disassemble it.
- Hint provided was that main is also an accepted command with gdb. Using that, we can debug the main function.
- Found break point of main using break main.
- Then disassembled it to get the dump of assembler code.
- Converted the code with %eax into decimal to get the number.

```
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ gdb debugger0_a
GNU gdb (Ubuntu 15.0.50.20240403-0ubuntu1) 15.0.50.20240403-git
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) break main
Breakpoint 1 at 0x1131
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
(gdb)
```
## Flag:
```
picoCTF{549698}
```
## Concepts Learnt:
- gdb is a very common debugger that can be used to disassemble many files in Linux terminal.

## Notes:
- gdb command had to be installed in terminal and was not available already. So downloaded it first and then used it to disassemble the file.

## Resources:
- https://www.geeksforgeeks.org/c/gdb-step-by-step-introduction/