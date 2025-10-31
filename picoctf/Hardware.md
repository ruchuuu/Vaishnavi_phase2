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
