# README.md

# Understanding Assembly: Registers, Data Movement, Loops, and Conditions 🖥️

Welcome! This guide will help you understand **CPU registers**, the **mov instruction**, **loops**, and **conditional statements** in x86-64 assembly. We'll also explain important concepts like `.text`, `.data`, and `_start` so beginners can follow along easily.

---

## 1️⃣ Assembly Sections

In NASM, programs are divided into **sections**:

### `.text` Section

* Holds **executable code** (instructions the CPU will run).
* Example:

```nasm
section .text
global _start
_start:
    mov rax, 60
    xor rdi, rdi
    syscall  ; exit program
```

### `.data` Section

* Holds **initialized data** (variables with known values).
* Example:

```nasm
section .data
message db "Hello, World!",0
```

### `.bss` Section

* Holds **uninitialized data**.
* Example:

```nasm
section .bss
buffer resb 64  ; reserve 64 bytes
```

### `_start`

* Entry point of the program for Linux.
* The OS jumps here when the program begins.
* Exit with `syscall` using RAX=60.

---

## 2️⃣ Registers Recap

* **RAX, RBX, RCX, RDX**: General-purpose
* **RDI, RSI, RDX, RCX**: Function arguments
* **RAX**: Function return value
* **RSP**: Stack pointer
* **RBP**: Base pointer
* **RIP**: Instruction pointer (where CPU executes next)

---


```nasm
mov rdi, rax          ; copy RAX to RDI
mov rax, [some_address] ; load value from RAM into RAX
```

---

## 4️⃣ Conditional Statements (if/else)

* Use `cmp` to compare values
* Use `jxx` to jump based on the result

```nasm
mov rax, 5
cmp rax, 10
jl less_label   ; jump if RAX < 10
mov rbx, 1      ; else part
jmp done
less_label:
mov rbx, 0      ; if RAX < 10
done:
```

---

## 5️⃣ Loops 🔁

Loops let you repeat instructions.

### Counting Loop (1 to 5)

```nasm
mov rcx, 1
loop_start:
cmp rcx, 6
jge done
mov rax, rcx ; example action
inc rcx
jmp loop_start
done:
```

### While-style Loop

```nasm
mov rax, 1
while_loop:
cmp rax, 5
jg done
add rax, 1
jmp while_loop
done:
```

### Loop + Condition Example

Check if numbers 1 to 10 are even or odd:

```nasm
mov rcx, 1
loop_start:
cmp rcx, 11
jge done
mov rax, rcx
and rax, 1       ; test even/odd
cmp rax, 0
je even_number
odd_number:
mov rbx, 0       ; odd
jmp next
even_number:
mov rbx, 1       ; even
next:
inc rcx
jmp loop_start
done:
```

---

## 6️⃣ Summary Table

| Concept           | Description                                |
| ----------------- | ------------------------------------------ |
| `.text`           | Holds executable code                      |
| `.data`           | Holds initialized data                     |
| `.bss`            | Holds uninitialized data                   |
| `_start`          | Program entry point                        |
| `cmp`             | Compare two values and set flags           |
| `jxx`             | Conditional jump based on flags            |
| `jmp`             | Unconditional jump                         |
| Label             | Marks a location in code                   |
| Loop              | Repeats instructions while condition holds |
| Condition in Loop | Allows decision making inside repetition   |

---

This README gives you a friendly guide to **moving data**, **using loops**, and **making decisions** in assembly. Practice these concepts to become comfortable with x86-64 programming!

