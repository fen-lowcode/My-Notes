
> [!Note] 
> The **GCC compiler**, also known as the **GNU Compiler Collection**, is primarily known as a compiler for the **C programming language**, but it doesn’t stop there.

---
GCC also supports a wide range of **programming languages**, **hardware architectures**, and **operating systems**, making it a highly versatile and widely-used tool in the software development world.

It is distributed as **free software** by the **Free Software Foundation (FSF)** under the terms of the **GNU General Public License (GNU GPL)**.


> [!Aside]
> **C was developed from 1969 to 1973** by Dennis Ritchie of Bell Laboratories.  
> The American National Standards Institute (ANSI) ratified the ANSI C standard in 1989, and this standardization later became the responsibility of the International Standards Organization (ISO).  
> The standards define the C language and a set of library functions known as the C standard library.  
> Kernighan and Ritchie describe ANSI C in their classic book, affectionately known as “K&R.”  
> In Ritchie’s words, C is “quirky, flawed, and an enormous success.”
>
> ---
>
> **So why the success?**
>
> - C was closely tied with the **Unix operating system**.  
>   C was developed from the beginning as the system programming language for Unix. Most of the Unix kernel, and all of its supporting tools and libraries, were written in C.  
>   As Unix became popular in universities in the late 1970s and early 1980s, many people were exposed to C and found that they liked it.  
>   Since Unix was written almost entirely in C, it could be easily ported to new machines—creating an even wider audience for both C and Unix.
>
> - **C is a small, simple language.**  
>   The design was controlled by a single person, rather than a committee, resulting in a clean and consistent language.  
>   The *K&R* book describes the complete language and standard library—with examples and exercises—in just 261 pages.  
>   This simplicity made C relatively easy to learn and port to different systems.
>
> - **C was designed for a practical purpose.**  
>   Originally created to implement Unix, it turned out to be broadly useful for many other programs.  
>   Developers found they could write the code they needed without the language getting in their way.
>
> ---
>
> **Limitations of C:**  
> C is still widely used for system-level programming and has a huge installed base of application-level software.  
> However, it is not perfect for every situation.  
> Common issues include:
> - Confusing pointer syntax and usage.
> - Lack of abstraction features like classes, objects, and exceptions
`

---

> [!Note]
> So how does GCC compiler work?

The GCC compiler driver reads a source file  and translate it into an executable object file `which a file ends with .o` . The translation is performed in the sequence of four phases. The program performed the four phases **`Preprocessor, Compiler, Assembler and Linker`** are known collectively as the **==compilation system==**.

![](Figure1.3.png)

`
*  ==Preprocessing phase== -  The preprocessor (cpp) modifies the original C program source code according to directive that begoins with the '#' character. For example, the `#include <stdio.h>` tells the preprocessor to read the content of the system header file stdio.h and insert it directly into the program text. the result is another C source code program, typically that ends with the `.i` suffix.

* ==Compilation phase== - The compiler (cc1) translate the text file `that ends with .i suffix` into a text file `that ends with .s suffix` which contains assembly-language program. Assembly language is useful because it provides a common output language for different compilers for different high-level languages. For example, C compilers and Fortran compilers both generate output files in the same assembly language.

*  ==Assembly phase== - Next the assembler (as) translate the file assembly file `that ends with .s`
   into a machine-language instruction, packages them in a form know as a `relocatable object program` and stores the result in a object file `usually that ends with .o`. This file is a binary file to encode some instructions for the program.

*  ==Linking phase== - Is the final step in the compilation process. For example, when a program calls external functions like `printf`, which is part of the standard C library, those functions aren't defined in the user's source code. Instead, they reside in precompiled object files, such as `printf.o`. During linking, a tool called the **linker** (typically `ld`) takes the object file generated from your source code (e.g., `hello.o`) and combines it with other necessary object files and libraries. The result is a complete **executable file** (e.g., `hello`) that contains all the code and data needed to run the program. This file is then ready to be loaded into memory and executed by the operating system.  

   >[!Sub-Topic]
   >If confused refer to [[How linker works]]
