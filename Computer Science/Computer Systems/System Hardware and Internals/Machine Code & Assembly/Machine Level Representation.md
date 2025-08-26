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

* Next, the assembler converts the assembly code into binary object-code files p1.o and p2.o. 
	Object code is one form of machine code—it contains binary representations of all of the instructions, but the addresses of global values are not yet filled in. 

* Finally, the linker merges these two object-code files along with code implementing library functions (e.g., printf) and generates the final executable code file p (as specified by the command-line directive -o p). Executable code is the second form of machine code we will consider—it is the exact form of code that is executed by the processor.

---

#### **Machine Level Code**


**As described in [[The Operating System Manages the Hardware#The Importance of Abstractions in Computer Systems|Importance of Abstraction]], computer systems employ several different forms of abstraction, hiding details of an implementation through the use of a simpler abstract model.**

> ==Two of these are especially important for machine-level programming.== 

* First, the format and behavior of a machine-level program is defined by the instruction set architecture, or ISA, defining the processor state, the format of the instructions, and the effect each of these instructions will have on the state.

==Most ISA, including x86-64, describe== the behavior of ==a program as if each instruction is executed in sequence, with one instruction completing before the next one begins.==

==The processor hardware is far more elaborate,== **executing many instructions concurrently, but it employs safeguards to ensure that the overall behavior matches the sequential operation dictated by the ISA.** 

* Second, the memory addresses used by a machine-level program are virtual addresses, providing a memory model that appears to be a very large byte array. 

==The actual implementation of the memory system involves a combination of multiple hardware memories and operating system software.==

---

**The compiler does most of the work in the overall compilation sequence**, ==transforming programs expressed in the relatively abstract execution model provided by C into the very elementary instructions that the processor executes.==


>[!Important]
> #### The assembly-code representation is very close to machine code.
>
Its main feature is that it is in a more readable textual format, as compared to the binary format of
machine code. Being able to understand assembly code and how it relates to the original C code is a key step in understanding how computers execute programs.


==The machine code for x86-64 differs greatly from the original C code. Parts of the processor state are visible that normally are hidden from the C programmer:==


* The **[[Program counter]]** (commonly referred to as the PC, and called %rip in x86-64) **indicates the address in memory of the next instruction to be executed.**

* The integer **[[Register File]]** contains 16 named locations storing 64-bit values. **These registers can hold addresses (corresponding to C pointers) or integer data. Some registers are used to keep track of critical parts of the program state, while others are used to hold temporary data, such as the arguments and local variables of a procedure, as well as the value to be returned by a function.**

* The **[[condition code registers]]** **hold status information about the most recently executed arithmetic or logical instruction.** These are used to implement conditional changes in the control or data flow, such as is required to implement if and while statements.

* A set of **[[vector registers]]** can each **hold one or more integer or floating-point values.**


==Whereas C provides a model in which objects of different data types can be declared and allocated in memory, machine code views the memory as simply a large byte-addressable array.==

==Aggregate data types in C such as arrays and structures are represented in machine code as contiguous collections of bytes.==

==Even for scalar data types, assembly code makes no distinctions between signed or unsigned integers, between different types of pointers, or even between pointers and integers.==

---

==The program memory contains the executable machine code for the program, some information required by the operating system, a run-time stack for managing procedure calls and returns, and blocks of memory allocated by the user (e.g., by using the malloc library function).== 

==As mentioned earlier, the program memory is addressed using virtual addresses. At any given time, only limited subranges of virtual addresses are considered valid.== 

==For example, x86-64 virtual addresses are represented by 64-bit words. In current implementations of these machines, the upper 16 bits must be set to zero, and so an address can potentially specify a byte over a range of 248, or 64 terabytes. More typical programs will only have access to a few megabytes, or perhaps several gigabytes.==

**The operating system manages this virtual address space, translating virtual addresses into the physical addresses of values in the actual processor memory.**

A single machine instruction performs only a very elementary operation:

For example, it might add two numbers stored in registers, transfer data between memory and a register, or conditionally branch to a new instruction address. 

**The compiler must generate sequences of such instructions to implement program constructs such as arithmetic expression evaluation, loops, or procedure calls and returns.**

---


