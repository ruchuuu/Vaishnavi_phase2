# 1. IQ Test
> Let your input x = 30478191278. Wrap your answer with nite{ } for the flag. As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}. Use the logic circuit given.
## Solution:
- Converted x from decimal to 36 digit binary.
- 30478191278 -> 011100011000101001000100101010101110 in binary
- Calculated the outputs for each logic gate to get the final 12-digit y value.
<img width="505" height="898" alt="Screenshot 2025-10-31 011743" src="https://github.com/user-attachments/assets/22c6582b-82f5-4cd1-9d79-91698c360081" />

- Final values {y0,...,y11}={100010011000}
- The flag becomes nite{100010011000}
## Flag:
```
nite{100010011000}
```
## Concepts Learnt:
- AND: A.B
- OR: A+B
- NOT: A' (complement)
- NAND: (A.B)'
- NOR: (A+B)'
- XOR: (A'.B)+(A.B')
***
# 2. Bare Metal Alchemist
> my friend recommended me this anime but i think i've heard a wrong name. Analyse the firmware to get to the flag.
## Solution:
- The file provided is firmware.elf.
```
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ file firmware.elf
firmware.elf: ELF 32-bit LSB executable, Atmel AVR 8-bit, version 1 (SYSV), statically linked, with debug_info, not stripped
```
- This tells that the file is in an executable and linkable format. The machine code inside is for an Atmel AVR microcontroller like classic Arduino chips. Also, it has it's main function accessible which makes it easier to analyze the file.
- Upon searching, I found out that such files can be analyzed in Ghidra tool.
- Opening the file in Ghidra, found the main source code by going to symbol tree -> functions -> main.
<img width="1920" height="1080" alt="Screenshot (154)" src="https://github.com/user-attachments/assets/37312410-5f19-4ae9-a515-f029a8edc1c2" />

- The part that can help is the while logic part of the code
```
      R15R14 = 0x68;
      R16 = 0;
      while( true ) {
        Z = (byte *)R15R14;
        R25R24._0_1_ = *(byte *)(uint3)R15R14;
        if ((byte)R25R24 == 0) break;
        Z._1_1_ = (undefined1)(R15R14 >> 8);
        Z._0_1_ = (byte)R25R24 ^ R11;
        if ((byte)R25R24 == 0xa5) break;
        R25R24._0_1_ = DAT_mem_0029;
        R25R24._0_1_ = (byte)R25R24 ^ (byte)R25R24 * '\x02';
        if (((byte)R25R24 & 4) == 0) {
          auStack_7 = (undefined1  [3])0x131;
          z1();
          break;
        }
```
- This basically tells us that we have to start from R15R14 that is 0x68 address and stop at whatever address the byte becomes 0.
- With this loop, perform XOR operation with key R11 which is 0xa5 (mentioned at the start of the c code).
- Using G key, open go to prompt where we can add addresses of our choice. Add 0068, we see hex bytes next to address till 00 encountered.
- They also stand out from other bytes of addresses that seem different from the hex values from 68 to 93.
<img width="1920" height="1080" alt="Screenshot (156)" src="https://github.com/user-attachments/assets/f7081452-2101-4b10-8109-469f028e8fac" />

- Taking these hex values to online XOR decrypter with key A5, we get the flag.
## Flag:
```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```
## Concepts Learnt:
- Firmware is a set of programs that are stored on a hardware device to perform several tasks which a manufacturer wants that device to perform.
- How to reverse engineer a file/program using Ghidra
## Notes:
- I tried opening firmware in vscode but got unreadable characters.
- Then I tried to read it online which also gave nothing valuable.
- I also searched anime with bare metal as the name and description suggested but got nothing useful. Then I researched on firmware and how to analyze them for such challenges.
## Resources:
- https://www.varonis.com/blog/how-to-use-ghidra#reverse-engineering-using-ghidra
- https://prabhankar.medium.com/firmware-analysis-part-1-cd43a1ad3f38
- https://www.jonaslieb.de/blog/arduino-ghidra-intro/
- https://www.dcode.fr/xor-cipher
***
