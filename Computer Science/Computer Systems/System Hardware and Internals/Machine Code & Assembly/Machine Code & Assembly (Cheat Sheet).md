### Basics
- **Machine code** = raw instructions for CPU.
- **Assembly** = human-readable form of machine code.
- **Compiler (e.g., GCC)**:
    - Source → Assembly → Machine code (via assembler & linker).

### High-Level vs Assembly
- ==High-level languages: type safety, portability, efficiency.==
- ==Assembly: machine-specific, manual control.==
- ==Today: l**earn to read compiler assembly, not write it**.==

### Why Learn Assembly
- **Understand compiler optimizations.**
- **Debug performance-critical code.**
- **Analyze concurrency behavior.**
- **Security:** ==*buffer overflows, memory exploits.*==

### Compiler Optimizations
- **Reorder, remove redundant ops, replace with faster ones.**
- **Transform recursion → iteration.**
- **Reverse engineering compiler output is easier (regular patterns).**

### x86-64 Overview
- Evolved: 16-bit → 32-bit (IA32) → 64-bit.
- Widely used (PCs, servers, supercomputers).
- Modern systems use **subset** of instructions.
- Course focus: GCC + Linux subset.

### Learning Path
1. C ↔ Assembly ↔ Machine code.
2. x86-64 basics: data, control, instructions.
3. Control flow: if, loops, switch.
4. Procedures: stack, locals, calls.
5. Data: arrays, structs, unions.
6. Memory errors: ==OOB==, buffer overflow.
7. Debugging: `gdb`.
8. Floating point: representation + ops.

### 32-bit vs 64-bit
- 32-bit: 4 GB addressable memory.
- 64-bit: up to 256 TB (expandable to 16 EB).

### Scope
- Focus: compiled C programs on modern OS.
- Skip: legacy 16-bit or obsolete features.