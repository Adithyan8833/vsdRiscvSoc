# Instruction Decoding

## Factorial  
**Objective:**  
Compute the factorial of a given integer and print the result.

| Instruction       | Opcode   | rd           | rs1         | rs2 | funct3 | funct7   | Binary Representation                                      | Description                                      |
|-------------------|----------|--------------|-------------|-----|--------|----------|-------------------------------------------------------------|--------------------------------------------------|
| addi sp,sp,-32    | 0010011  | sp (x2)      | sp (x2)     | —   | 000    | —        | 111111111000 00010 000 00010 0010011                        | Allocate 32 bytes on stack                       |
| sd ra,24(sp)      | 0100011  | —            | sp (x2)     | ra  | 011    | —        | 0000000 00001 00010 011 11000 0100011                        | Store return address on stack                    |
| li a5,12          | 0010011  | a5 (x15)     | x0          | —   | 000    | —        | 000000001100 00000 000 01111 0010011                         | Load immediate value 12 into a5                  |
| lw a5,-20(s0)     | 0000011  | a5 (x15)     | s0 (x8)     | —   | 010    | —        | 111111111011 01000 010 01111 0000011                         | Load word from stack into a5                     |
| mv a0,a5          | 0110011  | a0 (x10)     | a5 (x15)    | x0  | 000    | 0000000  | 0000000 00000 01111 000 01010 0110011                        | Copy a5 into a0                                  |

**Notes:**  
This snippet shows the function prologue, immediate loading, memory access, and register movement before calling the factorial function.

---

## Max Array  
**Objective:**  
Find the maximum value in a given integer array and print the result.

| Instruction       | Opcode   | rd           | rs1         | rs2 | funct3 | funct7   | Binary Representation                                      | Description                                      |
|-------------------|----------|--------------|-------------|-----|--------|----------|-------------------------------------------------------------|--------------------------------------------------|
| addi sp,sp,-64    | 0010011  | sp (x2)      | sp (x2)     | —   | 000    | —        | 111111100000 00010 000 00010 0010011                        | Allocate 64 bytes on stack                       |
| sd ra,56(sp)      | 0100011  | —            | sp (x2)     | ra  | 011    | —        | 0000000 00001 00010 011 111000 0100011                        | Store return address to stack                    |
| lw a5,-64(s0)     | 0000011  | a5 (x15)     | s0 (x8)     | —   | 010    | —        | 111111000000 01000 010 01111 0000011                         | Load first array element into a5                 |
| bge a5,a4,103a2   | 1100011  | —            | a5 (x15)    | a4  | 101    | —        | 0000000 01110 01111 101 00010 1100011                        | Branch if a5 ≥ a4                                |
| addiw a5,a5,1     | 0011011  | a5 (x15)     | a5 (x15)    | —   | 000    | —        | 000000000001 01111 000 01111 0011011                         | Increment loop index by 1                        |

**Notes:**  
This snippet shows stack setup, array element loading, conditional branch for max comparison, and loop counter update.

---

## BitOps  
**Objective:**  
Perform bitwise operations (AND, OR, XOR, shifts) between two integers and display the results.

| Instruction       | Opcode   | rd           | rs1         | rs2 | funct3 | funct7   | Binary Representation                                      | Description                                      |
|-------------------|----------|--------------|-------------|-----|--------|----------|-------------------------------------------------------------|--------------------------------------------------|
| addi sp,sp,-32    | 0010011  | sp (x2)      | sp (x2)     | —   | 000    | —        | 111111100000 00010 000 00010 0010011                        | Allocate 32 bytes on stack                       |
| lui a5,0xa5a5a    | 0110111  | a5 (x15)     | —           | —   | —      | —        | 10100101101001011010 01111 0110111                          | Load upper 20 bits of immediate into a5          |
| and a5,a5,a4      | 0110011  | a5 (x15)     | a5 (x15)    | a4  | 111    | 0000000  | 0000000 01110 01111 111 01111 0110011                        | Bitwise AND of a5 and a4                         |
| slliw a5,a5,0x3   | 0011011  | a5 (x15)     | a5 (x15)    | —   | 001    | 0000000  | 000000000011 01111 001 01111 0011011                         | Shift left logical immediate by 3                |
| srliw a5,a5,0x2   | 0011011  | a5 (x15)     | a5 (x15)    | —   | 101    | 0000000  | 000000000010 01111 101 01111 0011011                         | Shift right logical immediate by 2               |

**Notes:**  
This sequence covers stack allocation, constant loading, AND operation, left shift, and right shift.

---

## Bubble Sort  
**Objective:**  
Sort an array of integers using the Bubble Sort algorithm and display the sorted output.

| Instruction       | Opcode   | rd           | rs1         | rs2 | funct3 | funct7   | Binary Representation                                      | Description                                      |
|-------------------|----------|--------------|-------------|-----|--------|----------|-------------------------------------------------------------|--------------------------------------------------|
| addi sp,sp,-64    | 0010011  | sp (x2)      | sp (x2)     | —   | 000    | —        | 111111100000 00010 000 00010 0010011                        | Allocate 64 bytes on the stack                   |
| lui a5,0x1d       | 0110111  | a5 (x15)     | —           | —   | —      | —        | 0000000000011101 01111 0110111                              | Load upper immediate into a5                     |
| mv a1,a4          | 0110011  | a1 (x11)     | a4 (x14)    | —   | 000    | 0000000  | 0000000 01110 01011 000 01011 0110011                        | Move value from a4 to a1                         |
| add a5,a5,a4      | 0110011  | a5 (x15)     | a5 (x15)    | a4  | 000    | 0000000  | 0000000 01110 01111 000 01111 0110011                        | Add contents of a5 and a4                        |
| blt a4,a5,10458   | 1100011  | —            | a4 (x14)    | a5  | 100    | —        | 0000000 01111 01110 100 XXXXX 1100011                        | Branch if a4 < a5                                |

**Notes:**  
This snippet shows stack allocation, immediate loading, data movement, addition, and conditional branching in bubble sort.
