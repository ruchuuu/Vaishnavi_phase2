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
***

# 2. Vault-door-3
> Find out the correct password to the vault with the help of the given source code.
## Solution:
- Through the hint it is understood that we have to figure out the right password by iterating through the loop.
- So manually doing the logic with charAt() function, the first 8 characters are the same as buffer (password returned).
- Next 8 are equal to (23-i) value of buffer.
- After that, every even index is equal to (46-i) value of buffer.
- Odd indices stay the same.
- This gives the password jU5t_a_s1mpl3_an4gr4m_4_u_79958f. Which is the flag.
![vault](https://github.com/user-attachments/assets/615f53a6-36d6-4cd6-9577-23035ba29f51)
## Flag:
```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
```
## Concepts Learnt:
- charAt(): returns the character at the specified index in a string in Java.
## Resources:
- https://www.w3schools.com/java/ref_string_charat.asp
***
# 3. ARMssembly 1
> For what argument does this program print `win` with variables 68, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})
## Solution:
- Store w0 as stack at offset 16. 68 was moved here.
- Similarly, 2 moved to w0=stack+20 and 3 moved to w0=stack+24
- Load w0=2,w1=68
- Bitwise shift operation on w1,w0 -> 68<<2=272 == w0
- Store w0 as stack+28=272
- Load w1=272, w0=3
- Signed division of w1,w0 -> 272//3 (only int value) == w0
- Store w0 as stack+28 = 90
- Load w1=90, and w0= stack+12=x(user input)
- Subtract without carry w0=w1-w0=90-x
- In main section, compare w0 with 0. If non zero value, bne (branch not equal) branch to .L4, which branches to .L1 which prints 'lose'.
- So to make w0=0, 90-x=0 => x=90
- 90 in hex, lowercase, no 0x and 32 bits is 0000005a.
- Hence the flag is picoCTF{0000005a}.
![arm](https://github.com/user-attachments/assets/e1c362e2-1b5b-45c4-b194-fd69aa9af44a)

## Flag:
```
picoCTF{0000005a}
```
## Concepts Learnt:
- ARMv8-A is the ARM architecture profile and instruction-set architecture (ISA) that enables 64-bit processing by adding a new instruction set (A64) and allowing registers to be 64 bits wide. 
- ldr: loads a register with a value from memory.
- str: store a register value into memory.
- lsl: bitwise shift operator
- mov: copies the value of operand into register.
- sub: subtract without carry
- sdiv: signed divide (only integer)
- cmp: compare two operands
- .L4/.L1: labels for specific codes.

## Resources:
- https://medium.com/@mohamad.aerabi/arm-binary-analysis-part4-9f1388767d28
- https://developer.arm.com/documentation/dui0379/e/arm-and-thumb-instructions/sub
- https://developer.arm.com/documentation/dui0473/m/arm-and-thumb-instructions/sdiv
- https://developer.arm.com/documentation/dui0489/i/arm-and-thumb-instructions/mov
- https://developer.arm.com/documentation/dui0552/a/the-cortex-m3-instruction-set/memory-access-instructions/ldr-and-str--register-offset
