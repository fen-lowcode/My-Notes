#computer-system  #programming 


This section focus on the deeper layers of programs and machine code and their relation in regards to [[Machine Code & Assembly (Cheat Sheet)]]

---
#### **Program Encoding**

Suppose we write a C program as two files p1.c and p2.c. We can then compile this code using a Unix command line:

```Terminal
linux > gcc -Og -o p p1.c p2.c
```

The command gcc indicates the gcc C compiler. Since this is the default compiler on Linux, we could also invoke it as simply cc. 

==The command-line option -Og instructs the compiler to apply a level of optimization that yields machine code that follows the overall structure of the original C code.== 

Invoking higher levels of optimization can generate code that is so heavily transformed that the relationship between the generated machine code and the original source code is difficult to understand. 

We will therefore use -Og optimization as a learning tool and then see what happens as we increase the level of optimization. 

In practice, **higher levels of optimization (e.g., specified with the option -O1 or -O2) are considered a better choice in terms of the resulting program performance.**

---
##### The gcc command invokes an entire sequence of programs to turn the source code into executable code. `Reference`:  [[What is GCC and G++]],  [[How GCC works under the hood]] 

* First, the C preprocessor expands the source code to include any files specified with `#include` commands and to expand any macros, specified with `#define` declarations. 

* Second, the compiler generates assembly code versions of the two source files having names p1.s and p2.s. 

* Next, the assembler converts the assembly code into binary object-code files p1.o and p2.o. Object code is one form of machine code—it contains binary representations of all of the instructions, but the addresses of global values are not yet filled in. 

* Finally, the linker merges these two object-code files along with code implementing library functions (e.g., printf) and generates the final executable code file p (as specified by the command-line directive -o p). Executable code is the second form of machine code we will consider—it is the exact form of code that is executed by the processor.

---

#### **Machine Level Code**


**As described in [[The Operating System Manages the Hardware#The Importance of Abstractions in Computer Systems|Importance of Abstraction]], computer systems employ several different forms of abstraction, hiding details of an implementation through the use of a simpler abstract model.**

> ==Two of these are especially important for machine-level programming.== 

* First, **the format and behavior of a machine-level program is defined by the instruction set architecture**, or [[ISA]], defining the processor state, the format of the instructions, and the effect each of these instructions will have on the state.

==Most ISA, including [[x86-64]], describe== the behavior of ==a program as if each instruction is executed in sequence, with one instruction completing before the next one begins.==

==The processor hardware is far more elaborate,== **executing many instructions concurrently, but it employs safeguards to ensure that the overall behavior matches the sequential operation dictated by the ISA.** 

* Second, the memory addresses used by a machine-level program are virtual addresses, providing a memory model that appears to be a very large byte array. 

==The **actual implementation of the memory system involves a combination of multiple hardware memories and operating system software.==**

---

**The compiler does most of the work in the overall compilation sequence**, ==transforming programs expressed in the relatively abstract execution model provided by C into the very elementary instructions that the processor executes.==


>[!Important]
> #### **The assembly-code representation is very close to machine code.**
>
Its main feature is that it is in a more readable textual format, as compared to the binary format of
machine code. Being able to understand assembly code and how it relates to the original C code is a key step in understanding how computers execute programs.


==The machine code for [[x86-64]] differs greatly from the original C code. Parts of the processor state are visible that normally are hidden from the C programmer:==


* The **[[Program counter]]** (commonly referred to as the PC, and called %rip in [[x86-64]]) **indicates the address in memory of the next instruction to be executed.**

* The integer **[[Register File]]** contains 16 named locations storing 64-bit values. **These registers can hold addresses (corresponding to C pointers) or integer data. Some registers are used to keep track of critical parts of the program state, while others are used to hold temporary data, such as the arguments and local variables of a procedure, as well as the value to be returned by a function.**

* The **[[condition code registers]]** **hold status information about the most recently executed arithmetic or logical instruction.** These are used to implement conditional changes in the control or data flow, such as is required to implement if and while statements.

* A set of **[[vector registers]]** can each **hold one or more integer or floating-point values.**


==Whereas C provides a model in which objects of different data types can be declared and allocated in memory, machine code views the memory as simply a large byte-addressable array.==

==Aggregate data types in C such as arrays and structures are represented in machine code as contiguous collections of bytes.==

==Even for scalar data types, assembly code makes no distinctions between signed or unsigned integers, between different types of pointers, or even between pointers and integers.==

==The program memory contains the executable machine code for the program, some information required by the operating system, a run-time stack for managing procedure calls and returns, and blocks of memory allocated by the user (e.g., by using the malloc library function).== 

==As mentioned earlier, the program memory is addressed using virtual addresses. At any given time, only limited subranges of virtual addresses are considered valid.== 

==For example, [[x86-64]] virtual addresses are represented by 64-bit words. In current implementations of these machines, the upper 16 bits must be set to zero, and so an address can potentially specify a byte over a range of 248, or 64 terabytes. More typical programs will only have access to a few megabytes, or perhaps several gigabytes.==

**The operating system manages this virtual address space, translating virtual addresses into the physical addresses of values in the actual processor memory.**

A single machine instruction performs only a very elementary operation:

For example, it might add two numbers stored in registers, transfer data between memory and a register, or conditionally branch to a new instruction address. 

**The compiler must generate sequences of such instructions to implement program constructs such as arithmetic expression evaluation, loops, or procedure calls and returns.**


>[! Code Examples]


Suppose we write a C code file mstore.c containing the following function definition:

```C 

// mstore.c

long mult2(long, long);

void multstore(long x, long y, long *dest) {
	long t = mult2(x, y);
	*dest = t;
}
```


==To see the assembly code generated by the C compiler, we can use the -S option on the command line:==

``` linux
linux> gcc -Og -S mstore.c
```

==This will cause gcc to run the compiler, generating an assembly file `mstore.s`, and go no further.== (Normally it would then invoke the assembler to generate an object-code file.)

==The assembly-code file contains various declarations, including the following set of lines:==

``` Assembly

multstore:
	pushq %rbx
	movq  %rdx, %rbx
	call  mult2
	movq  %rax, (%rbx)
	popq  %rbx
	ret
```

==Each indented line in the code corresponds to a single machine instruction.== 

For example, the `pushq` instruction indicates that the contents of register `%rbx` should be pushed onto the program stack. All information about local variable names or data types has been stripped away. 

==If we use the -c command-line option, gcc will both compile and assemble the code==

``` linux
linux> gcc -Og -c mstore.c
```

This will generate an object-code file `mstore.o` that is in binary format and hence cannot be viewed directly. 

==Embedded within the 1,368 bytes of the file `mstore.o` is a 14-byte sequence with the hexadecimal representation==

``` c
53 48 89 d3 e8 00 00 00 00 48 89 03 5b c3
```

==This is the object code corresponding to the assembly instructions listed previously.== 


==**A key lesson to learn from this is that the program executed by the machine is simply a sequence of bytes encoding a series of instructions.**== 

The machine has very little information about the source code from which these instructions were generated.

**To inspect the contents of machine-code files**, ==a class of programs known as **[[Disassemblers]]** can be invaluable==. These programs generate a format similar to assembly code from the machine code. 

==With Linux systems, the program objdump (for “object dump”) can serve this role given the -d command-line flag:==

```
linux> objdump -d mstore.o
```

The result (where we have added line numbers on the left and annotations in italicized text) is as follows:


```
Disassembly of function sum in binary file mstore.
0000000000000000 <multstore>:
	Offset    Bytes                Instruction
	0x0       53                   push   %rbx
	0x1       48 89 d3             mov    %rdx, %rbx
	0x4       e8 00 00 00 00       callq  0x9 <multstore+0x9>
	0x9       48 89 03             mov    %rax, (%rbx)
	0xc       5b                   pop    %rbx
	0xd       c3                   retq

```

On the left we see the 14 hexadecimal byte values, listed in the byte sequence shown earlier, partitioned into groups of 1 to 5 bytes each.  Each of these groups is a single instruction, with the assembly-language equivalent shown on the right. 

>[!Important]
>Several features about machine code and its disassembled representation are worth noting:


* *x86-64 instructions can range in length from 1 to 15 bytes. The instruction encoding is designed so that commonly used instructions and those with fewer operands require a smaller number of bytes than do less common ones or ones with more operands.*

* *The instruction format is designed in such a way that from a given starting position, there is a unique decoding of the bytes into machine instructions.* 
	* *For example, only the instruction `pushq %rbx` can start with byte value 53.*

* *The disassembler determines the assembly code based purely on the byte sequences in the machine-code file. It does not require access to the source or assembly-code versions of the program.* 

* *The disassembler uses a slightly different naming convention for the instructions than does the assembly code generated by gcc. In our example, it has omitted the suffix ‘q’ from many of the instructions. These suffixes are size designators and can be omitted in most cases. Conversely, the disassembler adds the suffix ‘q’ to the call and `ret` instructions. Again, these suffixes can safely be omitted.*


generating the actual executable code requires running a linker on the set of object-code files, one of which must contain a function main.


``` c
#include <stdio.h>

void multstore(long, long, long *);

int main() {

	long d;
	
	multstore(2, 3, &d);
	printf("2 * 3 --> %ld\n", d);
	return 0;
}

long mult2(long a, long b) {

	long s = a * b;
	return s;
}
```


==We generate executable files by using a compiler like as follows:==

```
linux > linux> gcc -Og -o prog main.c mstore.c
```


The file prog has grown to 8,655 bytes, since it contains not just the machine code for the procedures we provided but also code used to start and terminate the program as well as to interact with the operating system.

==We can disassemble the executable file with objdump with `-d` flag.== and the output should look like below.

The disassembler will extract various code sequences, including the following:

```
0000000000400540 <multstore>:
  400540:  53                    push   %rbx
  400541:  48 89 d3              mov    %rdx,%rbx
  400544:  e8 42 00 00 00        callq  40058b <mult2>
  400549:  48 89 03              mov    %rax,(%rbx)
  40054c:  5b                    pop    %rbx
  40054d:  c3                    retq
  40054e:  90                    nop
  40054f:  90                    nop

```


This code is almost identical to that generated by the disassembly of mstore.c. 

==One important difference is that the addresses listed along the left are different the linker has shifted the location of this code to a different range of addresses.== 

A second difference is that the linker has filled in the address that the `callq` instruction should use in calling the function `mult2` (line 4 of the disassembly). ==**One task for the linker is to match function calls with the locations of the executable code for those functions.**== 

A final difference is that we see two additional lines of code (lines 8–9). 

These instructions will have no effect on the program, since they occur after the return instruction (line 7). They have been inserted to grow the code for the function to 16 bytes, enabling a better placement of the next block of code in terms of memory system performance.


>[!Just as you know]
>**For some applications, the programmer must drop down to assembly code to access low-level features of the machine. One approach is to write entire functions in  assembly code and combine them with C functions during the linking stage.**

---

### Data Formats


Intel’s x86 architecture originated as a **16-bit architecture**. From this origin, the terminology reflects the historical word sizes: a **word** refers to a 16-bit quantity, a **double word** to 32 bits, and a **quad word** to 64 bits. These terms are important when reading assembly instructions and understanding how data is stored in memory.

In **x86-64 systems**, C primitive types map to these Intel data representations. The standard `int` type occupies a **double word (4 bytes)**. Pointers, for instance `char *`, occupy **quad words (8 bytes)**, consistent with the 64-bit address space. The `long` type is also implemented as a **64-bit quad word**, allowing a much wider range of integer values than `int`.

The mapping of C types to Intel types, assembly suffixes, and sizes is as follows: `char` is a **byte** (1 byte) with assembly suffix `b`, `short` is a **word** (2 bytes) with suffix `w`, `int` is a **double word** (4 bytes) with suffix `l`, `long` and pointers (`char *`) are **quad words** (8 bytes) with suffix `q`. Floating-point types follow a similar pattern: `float` is **single-precision (4 bytes)** with suffix `s`, while `double` is **double-precision (8 bytes)** with suffix `l`.

The x86-64 instruction set provides instructions that correspond to each data size. Data movement operations, for example, exist in four variants: `movb` for bytes, `movw` for words, `movl` for double words, and `movq` for quad words. The suffix `l` is used both for 4-byte integers and 8-byte double-precision floating-point numbers, but this does not cause ambiguity because floating-point operations use a **completely separate set of registers and instructions**.

Historically, x86 processors used a special **80-bit (10-byte) floating-point format** for all floating-point operations. In C, this corresponds to the `long double` type. This type is not recommended for general use because it is **non-portable across platforms** and usually does not have the same hardware support for high-speed calculations as single- and double-precision floating-point operations.

Understanding these mappings is critical when reading assembly code generated by compilers such as GCC, particularly when inspecting registers and operand sizes. It also explains why memory alignment, pointer sizes, and register usage vary between types, and why suffixes in assembly instructions indicate operand size rather than the underlying type directly.

| C Declaration | Intel Data Type  | Assembly-Code Suffix | Size (bytes) |
| ------------- | ---------------- | -------------------- | ------------ |
| char          | Byte             | b                    | 1            |
| short         | Word             | w                    | 2            |
| int           | Double word      | l                    | 4            |
| long          | Quad word        | q                    | 8            |
| char *        | Quad word        | q                    | 8            |
| float         | Single precision | s                    | 4            |
| double        | Double precision | l                    | 8            |


---

### Accessing Information


==x86-64 central processing unit (CPU) contains a set of 16 general-purpose registers storing 64-bit values. These registers are used to store integer data as well as pointers.==

>[!Important]
>**[[Register| Click here to know what a register is]]**

The diagram below shows the 16 registers. Their names all begin with %r, but otherwise follow multiple different naming conventions, owing to the historical evolution of the instruction set.


==The original 8086 had eight 16-bit registers, shown in diagram as registers `%ax` through `%bp`.==

Each had a specific purpose, and hence they were given names that reflected how they were to be used. ==With the extension to IA32, these registers were expanded to 32-bit registers, labeled `%eax` through `%ebp`.==

==In the extension to x86-64, the original eight registers were expanded to 64 bits, labeled `%rax` through `%rbp`.==

In addition, eight new registers were added, and these were given labels according to a new naming convention: `%r8` through `%r15`. 

As the nested boxes in the diagram indicate, instructions can operate on data of different sizes stored in the low-order bytes of the 16 registers. 

==Byte-level operations can access the least significant byte, 16-bit operations can access the least significant 2 bytes, 32-bit operations can access the least significant 4 bytes, and 64-bit operations can access entire registers.==

| Register (64-bit) | 32-bit | 16-bit | 8-bit | Purpose        |
|-------------------|--------|--------|-------|----------------|
| %rax              | %eax   | %ax    | %al   | Return value   |
| %rbx              | %ebx   | %bx    | %bl   | Callee saved   |
| %rcx              | %ecx   | %cx    | %cl   | 4th argument   |
| %rdx              | %edx   | %dx    | %dl   | 3rd argument   |
| %rsi              | %esi   | %si    | %sil  | 2nd argument   |
| %rdi              | %edi   | %di    | %dil  | 1st argument   |
| %rbp              | %ebp   | %bp    | %bpl  | Callee saved   |
| %rsp              | %esp   | %sp    | %spl  | Stack pointer  |
| %r8               | %r8d   | %r8w   | %r8b  | 5th argument   |
| %r9               | %r9d   | %r9w   | %r9b  | 6th argument   |
| %r10              | %r10d  | %r10w  | %r10b | Caller saved   |
| %r11              | %r11d  | %r11w  | %r11b | Caller saved   |
| %r12              | %r12d  | %r12w  | %r12b | Callee saved   |
| %r13              | %r13d  | %r13w  | %r13b | Callee saved   |
| %r14              | %r14d  | %r14w  | %r14b | Callee saved   |
| %r15              | %r15d  | %r15w  | %r15b | Callee saved   |



>[!important]
Different registers serve different roles in typical programs. 

***Most unique among them is the stack pointer, `%rsp`, used to indicate the end position in the run-time stack.***

Some instructions specifically read and write this register. 

**The other 15 registers have more flexibility in their uses. A small number of instructions make specific use of certain registers. More importantly, a set of standard programming conventions governs how the registers are to be used for managing the stack, passing function arguments, returning values from functions, and storing local and temporary data.** 


> [! Operand Specifiers]


==Most instructions have one or more operands specifying the source values to use in performing an operation and the destination location into which to place the result.==

| Type                | Form                       | Operand Value Representation |
| ------------------- | -------------------------- | ---------------------------- |
| Immediate           | $Imm                       | Imm                          |
| Register            | ra                         | R[ra]                        |
| Memory              | (ra)                       | M[R[ra]]                     |
| Memory              | Imm(rb)                    | M[Imm + R[rb]]               |
| Memory              | (rb, ri)                   | M[R[rb] + R[ri]]             |
| Memory              | Imm(rb, ri)                | M[Imm + R[rb] + R[ri]]       |
| Memory              | (,ri,s)                    | M[R[ri] · s]                 |
| Memory              | Imm(,ri,s)                 | M[Imm + R[ri] · s]           |
| Memory              | (rb,ri,s)                  | M[R[rb] + R[ri] · s]         |
| Memory              | Imm(rb,ri,s)               | M[Imm + R[rb] + R[ri] · s]   |
| Absolute            | M[Imm]                     | Absolute                     |
| Indirect            | M[R[ra]]                   | Indirect                     |
| Base + displacement | M[Imm + R[rb]]             | Base + displacement          |
| Indexed             | M[R[rb] + R[ri]]           | Indexed                      |
| Indexed             | M[Imm + R[rb] + R[ri]]     | Indexed                      |
| Scaled indexed      | M[R[ri] · s]               | Scaled indexed               |
| Scaled indexed      | M[Imm + R[ri] · s]         | Scaled indexed               |
| Scaled indexed      | M[R[rb] + R[ri] · s]       | Scaled indexed               |
| Scaled indexed      | M[Imm + R[rb] + R[ri] · s] | Scaled indexed               |

`Figure 3.3` Operand forms. Operands can denote immediate (constant) values, register values, or values from memory. The scaling factor s must be either 1, 2, 4, or 8.


