# Machine-Level Programming I: Assembly Basics
## 1. Overview
### 1.1 Levels of Abstraction
```
C code → Assembly Programmer (CPU, registers, PC, memory) → Computer Designer
```
### 1.2 Core Definitions
#### 1.2.1 ISA (Instruction Set Architecture)
- Part of processor design
- Machine code: byte-level representation
- Assembly code: text representation of machine code
- PC: program counter
#### 1.2.2 Micro-architecture
- Performance aspects
- Cache sizes and core frequency
## 2. Assembly Characteristics
### 2.1 Data Types
- Integer
- Addresses
- Floating point
- SIMD vector type
- **Excluded**: No aggregate types (arrays, structs)
### 2.2 x86-64 Integer Registers
#### 2.2.1 General Purpose Registers (8 bytes each)
- `%rax` - Accumulator
- `%rbx` - Base
- `%rcx` - Counter
- `%rdx` - Data
- `%rsi` - Source index
- `%rdi` - Destination index
#### 2.2.2 Special Purpose Registers
- `%rsp` - Stack pointer
- `%rbp` - Base pointer
## 3. Operations
### 3.1 Moving Data
#### 3.1.1 Basic Syntax
- `movq source, destination`
- `q` = 8 bytes, `l` = 4 bytes (lower)
- No distinction between signed/unsigned
#### 3.1.2 Addressing Modes
- **Immediate**: `$0x400` (constant value)
- **Register**: `%rax` (register content)
- **Memory**: `(%rax)` (dereference)
- **Displacement**: `D(R)` → R + D
- **Base + Index**: `(Rb,Ri)` → Mem[Reg[Rb] + Reg[Ri]]
- **Full**: `D(Rb,Ri)` → Mem[Reg[Rb] + Reg[Ri] + D]
- **Scaled Index**: `(Rb,Ri,S)` → Mem[Reg[Rb] + S * Reg[Ri]]
### 3.2 Function Parameters Convention
- First argument → `%rdi`
- Second argument → `%rsi`
### 3.3 Example: Swap Function
#### 3.3.1 C Code
```c
void swap(long *xp, long *yp) {
    long t0 = *xp;
    long t1 = *yp;
    *xp = t1;
    *yp = t0;
}
```
#### 3.3.2 Assembly Code
```assembly
swap:
    movq    (%rdi), %rax    # t0 = *xp
    movq    (%rsi), %rdx    # t1 = *yp
    movq    %rdx, (%rdi)    # *xp = t1
    movq    %rax, (%rsi)    # *yp = t0
    ret
```

### 3.4 Address Computation
#### 3.4.1 LEA Instruction
- `leaq src, dst` - Load Effective Address
- Computes address without memory reference
#### 3.4.2 Example: Multiply by 12
```c
long m12(long x) { return x*12; }
```
```assembly
leaq (%rdi,%rdi,2), %rax    # t = x+2*x = 3*x
salq $2, %rax               # return t<<2 = 12*x
```
### 3.5 Arithmetic Operations
#### 3.5.1 Two Operand Instructions
- `addq src, dst` → dst += src
- `subq src, dst` → dst -= src
- `imulq src, dst` → dst *= src
- `salq src, dst` → dst <<= src (shift arithmetic left)
#### 3.5.2 One Operand Instructions
- `incq dst` → dst++

## 4. Compilation Pipeline
### 4.1 Process Flow
1. `.c` (C source) → **compile** → `.s` (Assembly)
2. `.s` (Assembly) → **assembler** → `.o` (Object file)
3. `.o` (Object file) + `.a` (Static libraries) → **linker** → **Executable program**
### 4.2 Compiler Options
- `-Og`: Optimize for debugging (recommended for learning)
