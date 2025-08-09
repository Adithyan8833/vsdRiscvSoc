# RISC-V Instruction Decoding

## factorial

**Objective:** Analysis of `factorial` assembly instructions and their decoding.

| Instruction | Opcode | rd | rs1 | rs2 | funct3 | funct7 | Binary (fields) | Description |
|-------------|--------|----|-----|-----|--------|--------|-----------------|-------------|
| addi sp,sp,-32 | 0010011 | sp | sp | imm=-32 | N/A | N/A | binary_placeholder | Decrement stack pointer by 32 bytes |
| sd ra,24(sp) | 0100011 | N/A | sp | ra | 011 | N/A | binary_placeholder | Store return address |
| sd s0,16(sp) | 0100011 | N/A | sp | s0 | 011 | N/A | binary_placeholder | Store s0 register |
| addi s0,sp,32 | 0010011 | s0 | sp | imm=32 | N/A | N/A | binary_placeholder | Set s0 to sp+32 |
| li a5,12 | 0010011 | a5 | x0 | imm=12 | N/A | N/A | binary_placeholder | Load immediate 12 into a5 |

**Notes:**
- This section shows the mapping from assembly instructions to machine code.
- Binary placeholders can be replaced with actual 32-bit binary encodings.
- Helps in understanding RISC-V ISA structure.

---

## max_array

**Objective:** Analysis of `max_array` assembly instructions and their decoding.

| Instruction | Opcode | rd | rs1 | rs2 | funct3 | funct7 | Binary (fields) | Description |
|-------------|--------|----|-----|-----|--------|--------|-----------------|-------------|
| addi sp,sp,-64 | 0010011 | sp | sp | imm=-64 | N/A | N/A | binary_placeholder | Decrement stack pointer by 64 bytes |
| sd ra,56(sp) | 0100011 | N/A | sp | ra | 011 | N/A | binary_placeholder | Store return address |
| sd s0,48(sp) | 0100011 | N/A | sp | s0 | 011 | N/A | binary_placeholder | Store s0 register |
| li a5,9 | 0010011 | a5 | x0 | imm=9 | N/A | N/A | binary_placeholder | Load immediate 9 into a5 |
| sw a5,-28(s0) | 0100011 | N/A | s0 | a5 | 010 | N/A | binary_placeholder | Store a5 value at s0-28 |

**Notes:**
- This section shows the mapping from assembly instructions to machine code.
- Binary placeholders can be replaced with actual 32-bit binary encodings.
- Helps in understanding RISC-V ISA structure.

---

## bitops

**Objective:** Analysis of `bitops` assembly instructions and their decoding.

| Instruction | Opcode | rd | rs1 | rs2 | funct3 | funct7 | Binary (fields) | Description |
|-------------|--------|----|-----|-----|--------|--------|-----------------|-------------|
| addi sp,sp,-32 | 0010011 | sp | sp | imm=-32 | N/A | N/A | binary_placeholder | Decrement stack pointer by 32 bytes |
| sd ra,24(sp) | 0100011 | N/A | sp | ra | 011 | N/A | binary_placeholder | Store return address |
| lui a5,0xa5a5a | 0110111 | a5 | N/A | N/A | N/A | N/A | binary_placeholder | Load upper immediate |
| addi a5,a5,1445 | 0010011 | a5 | a5 | imm=1445 | N/A | N/A | binary_placeholder | Add immediate to a5 |
| sw a5,-20(s0) | 0100011 | N/A | s0 | a5 | 010 | N/A | binary_placeholder | Store word a5 at s0-20 |

**Notes:**
- This section shows the mapping from assembly instructions to machine code.
- Binary placeholders can be replaced with actual 32-bit binary encodings.
- Helps in understanding RISC-V ISA structure.

---

## bubble_sort

**Objective:** Analysis of `bubble_sort` assembly instructions and their decoding.

| Instruction | Opcode | rd | rs1 | rs2 | funct3 | funct7 | Binary (fields) | Description |
|-------------|--------|----|-----|-----|--------|--------|-----------------|-------------|
| addi sp,sp,-64 | 0010011 | sp | sp | imm=-64 | N/A | N/A | binary_placeholder | Decrement stack pointer by 64 bytes |
| sd ra,56(sp) | 0100011 | N/A | sp | ra | 011 | N/A | binary_placeholder | Store return address |
| sd s0,48(sp) | 0100011 | N/A | sp | s0 | 011 | N/A | binary_placeholder | Store s0 register |
| li a5,9 | 0010011 | a5 | x0 | imm=9 | N/A | N/A | binary_placeholder | Load immediate 9 into a5 |
| sw zero,-20(s0) | 0100011 | N/A | s0 | zero | 010 | N/A | binary_placeholder | Store zero at s0-20 |

**Notes:**
- This section shows the mapping from assembly instructions to machine code.
- Binary placeholders can be replaced with actual 32-bit binary encodings.
- Helps in understanding RISC-V ISA structure.

---

