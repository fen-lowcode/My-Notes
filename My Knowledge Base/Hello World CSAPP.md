#CSAPP 


In their classic text on the C programming language [61], Kernighan and Ritchie introduce readers to C using the `hello` program shown in Figure 1.1. Although `hello` is a very simple program, **every major part of the system must work in concert** in order for it to run to completion.

In a sense, **the goal of this book is to help you understand what happens and why when you run `hello` on your system.** We begin our study of systems by tracing the lifetime of the `hello` program, from the time it is created by a programmer, until it runs on a system, prints its simple message, and terminates. As we follow the lifetime of the program, we will briefly introduce the key concepts, terminology, and components that come into play. Later chapters will expand on these ideas.

---

###### _figure 1.1_

#### C Source Code

```c

#include <stdio.h>

int main() {
	printf("Hello World\n");
	return 0;
}

```

---

###### _figure 1.2_

#### C Code to ASCII Table

|Character|ASCII|Character|ASCII|Character|ASCII|Character|ASCII|
|---|---|---|---|---|---|---|---|
|`#`|35|`i`|105|`n`|110|`c`|99|
|`l`|108|`u`|117|`d`|100|`e`|101|
||32|`<`|60|`s`|115|`t`|116|
|`d`|100|`i`|105|`o`|111|`.`|46|
|`h`|104|`>`|62|`\n`|10|`\n`|10|
|`i`|105|`n`|110|`t`|116||32|
|`m`|109|`a`|97|`i`|105|`n`|110|
|`(`|40|`)`|41|`\n`|10|`{`|123|
||32||32||32||32|
|`p`|112|`r`|114|`i`|105|`n`|110|
|`t`|116|`f`|102|`(`|40|`"`|34|
|`h`|104|`e`|101|`l`|108|`l`|108|
|`o`|111|`,`|44||32|`w`|119|
|`o`|111|`r`|114|`l`|108|`d`|100|
|`\n`|92 110|`"`|34|`)`|41|`;`|59|
||32||32||32||32|
|`r`|114|`e`|101|`t`|116|`u`|117|
|`r`|114|`n`|110||32|`0`|48|
|`;`|59|`\n`|10|`}`|125|||

---

#### 1.1 Information Is Bits + Context

**Our hello program begins life as a source program** (==or source file==) that the programmer creates with an editor and saves in a text file called hello.c.

==The source program is a sequence of bits, each with a value of 0 or 1, organized in 8-bit chunks called bytes.==

Each byte represents some text character in the program. Most computer systems represent text characters using the ASCII standard that represents each character with a unique byte-size integer value.

**For example, [Figure 1.2](app://obsidian.md/A%20Tour%20of%20Computer%20Systems%20-%20Chapter%201%20CSAPP#*figure%201.2*)**

- shows the ASCII representation of the hello.c program.
- The hello.c program is stored in a file as a sequence of bytes
- Each byte has an integer value that corresponds to some character.

**For example**, the first byte has the integer value 35, which corresponds to the character ‘#’. The second byte has the integer value 105, which corresponds to the character ‘i’, and so on.

==Notice that each text line is terminated by the invisible newline character ‘\n’, which is represented by the integer value 10.==

**Files such as hello.c that consist exclusively of ASCII characters are known as text files. All other files are known as binary files**. The representation of hello.c illustrates a fundamental idea: All information in a system including disk files, programs stored in memory, user data stored in memory, and data transferred across a network is represented as a bunch of bits.

==**The only thing that distinguishes different data objects is the context in which we view them.**==  
**==For example, in different contexts, the same sequence of bytes might represent an integer, floating-point number, character string, or machine instruction.==**

==As programmers, we need to understand machine representations of numbers because they are not the same as integers and real numbers. They are finite approximations that can behave in unexpected ways. This fundamental idea is explored in detail in Chapter 2.==


