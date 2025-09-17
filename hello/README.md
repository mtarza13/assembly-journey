# README.md

# Understanding Assembly: Registers and Data Movement

This guide covers the fundamental concepts of CPU registers and the mov instruction in x86-64 assembly.

## What Are Registers? 🛠️

Registers are small, super-fast storage spots located directly inside the CPU. The CPU uses them to hold the numbers and memory addresses it's currently working on. It is much faster for the CPU to use a register than to get data from main memory (RAM).

The names for registers follow a pattern. For instance, the RAX register can be broken down into smaller parts:

* **RAX**: The complete 64-bit register.
* **EAX**: The lower 32-bit section of RAX.
* **AX**: The lower 16-bit section of EAX.
* **AL**: The lowest 8 bits of AX.
* **AH**: The high 8 bits of AX.

When you modify a 32-bit register like EAX, the upper part of the full RAX register is usually cleared to zero. As a beginner, you will mostly work with the 64-bit (R prefix) and 32-bit (E prefix) register names.

## Key Registers and Their Roles

For projects like libft and general programming on Linux, it's important to know these key registers:

**Function Arguments:**

* RDI: Holds the 1st argument.
* RSI: Holds the 2nd argument.
* RDX: Holds the 3rd argument.
* RCX: Holds the 4th argument.

**Return Value:**

* RAX: Holds the result of a function when it finishes.

**Pointers:** These registers have special and critical purposes.

* RSP: The Stack Pointer, which always points to the top of the stack.
* RBP: The Base Pointer, which points to the base of the current function's workspace on the stack.
* RIP: The Instruction Pointer, which holds the address of the next command for the CPU to execute. This is a crucial register in security (pwn).

### Register Breakdown Table

| 64-bit (Quadword) | 32-bit (Doubleword) | 16-bit (Word) | 8-bit (Byte)        |
| ----------------- | ------------------- | ------------- | ------------------- |
| RAX               | EAX                 | AX            | AL (low), AH (high) |
| RBX               | EBX                 | BX            | BL (low), BH (high) |
| RCX               | ECX                 | CX            | CL (low), CH (high) |
| RDX               | EDX                 | DX            | DL (low), DH (high) |
| RSI               | ESI                 | SI            | SIL                 |
| RDI               | EDI                 | DI            | DIL                 |
| RBP               | EBP                 | BP            | BPL                 |
| RSP               | ESP                 | SP            | SPL                 |
| R8                | R8D                 | R8W           | R8B                 |
| R9                | R9D                 | R9W           | R9B                 |
| R10               | R10D                | R10W          | R10B                |
| R11               | R11D                | R11W          | R11B                |
| R12               | R12D                | R12W          | R12B                |
| R13               | R13D                | R13W          | R13B                |
| R14               | R14D                | R14W          | R14B                |
| R15               | R15D                | R15W          | R15B                |

## The mov Instruction: The CPU's Hands 👐

The mov instruction is fundamental because it's how you move data around. In short, mov copies data from a source to a destination. This is necessary because the CPU can only perform mathematical or logical operations on data located in a register.

### Moving Between Registers

You can copy data from one register to another. This is useful for preparing arguments for function calls.

**Instruction:** `mov rdi, rax`

```
BEFORE                            AFTER

RAX: [   100    ]                  RAX: [   100    ]  <-- Unchanged!
RDI: [ ???????? ]                  RDI: [   100    ]
```

### Moving from RAM to a Register

This is the most critical use of mov. The CPU must move data from RAM to a register before it can perform calculations. The square brackets (\[]) are used to indicate you are accessing the value at a memory address.

**Instruction:** `mov rax, [some_address_in_ram]`

```
BEFORE
RAM (at Address 0x1000): [ 42 ]   Register RAX: [ ???????? ]

AFTER
RAM (at Address 0x1000): [ 42 ]   Register RAX: [    42    ]
```

## Simple Exercise: Adding Two Numbers 🧠

Here is a small, complete program that uses mov and add to calculate 10 + 20.

### 1. The Code

Save this code in a file named `add.s`:

```nasm
section .text
global _start

_start:
; Use 'mov' to put the first number (10) into the RAX register.
mov     rax, 10

; Use 'mov' to put the second number (20) into the RBX register.
mov     rbx, 20

; Use 'add' to add the value in RBX to the value in RAX.
; The result (30) will be stored in RAX.
add     rax, rbx

; Exit the program gracefully.
mov     rax, 60
xor     rdi, rdi
syscall
```

### 2. How to Compile and Run

Open your terminal and run these commands:

```bash
# Assemble the code into an object file
nasm -f elf64 add.s -o add.o

# Link the object file into an executable program
ld add.o -o add_program
```

### 3. How to Check the Result

The program will run and exit, but the result (30) is stored in RAX just before exiting. We can use the debugger gdb to see it.

```bash
# Load the program into GDB
gdb ./add_program

# Inside GDB, run these commands:
(gdb) break _start      # Set a breakpoint at the beginning
(gdb) run               # Run the program
(gdb) si 3              # Execute the first 3 instructions (mov, mov, add)
(gdb) info registers rax # Check the value in RAX
```

GDB will display the following, confirming your result:

```
rax            0x1e     30
```

## What's Next?

Now that you have practiced with mov and add, the next step is to explore other fundamental instructions like xor, inc, push, and pop to manage data and the stack.

