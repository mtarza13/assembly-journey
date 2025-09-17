# README.md

# Understanding Assembly: Loops, and Conditions 🖥️

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

## 3️⃣ The `mov` Instruction 👐

Moves data between registers or memory.

```nasm
mov rdi, rax          ; copy RAX to RDI
mov rax, [some_address] ; load value from RAM into RAX
```

---

## 4️⃣ Conditional Statements (if/else)

* Use `cmp` to compare values
* Use `jxx` to jump based on the result

### Jump Instructions (All Conditions)

| Instruction   | Meaning                               |
| ------------- | ------------------------------------- |
| `je` / `jz`   | Jump if equal / zero flag set         |
| `jne` / `jnz` | Jump if not equal / zero flag not set |
| `jg` / `jnle` | Jump if greater (signed)              |
| `jge` / `jnl` | Jump if greater or equal (signed)     |
| `jl` / `jnge` | Jump if less (signed)                 |
| `jle` / `jng` | Jump if less or equal (signed)        |
| `ja` / `jnbe` | Jump if above (unsigned)              |
| `jae` / `jnb` | Jump if above or equal (unsigned)     |
| `jb` / `jnae` | Jump if below (unsigned)              |
| `jbe` / `jna` | Jump if below or equal (unsigned)     |
| `js`          | Jump if sign flag set                 |
| `jns`         | Jump if sign flag not set             |
| `jo`          | Jump if overflow                      |
| `jno`         | Jump if not overflow                  |

### Example

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

Loops let you repeat instructions. They can be **counting loops** or **condition-based loops**.

### Counting Loop (for loop)

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

### Repeat-Until Loop Concept

```nasm
mov rax, 1
repeat_loop:
add rax, 1
cmp rax, 5
jle repeat_loop ; repeat while rax <= 5
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

### Using `loop` Instruction (Optional)

NASM also has a `loop` instruction:

```nasm
mov rcx, 5
loop_label:
; do something
loop loop_label ; automatically dec RCX and jump if not zero
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
| `loop`            | Loop instruction using RCX counter         |
| Label             | Marks a location in code                   |
| Loop              | Repeats instructions while condition holds |
| Condition in Loop | Allows decision making inside repetition   |

---

This README now includes **all common conditions**, **loop types**, and examples ready to compile and test. Practice these to master assembly control flow!

