#computer-system 


A computer system consists of **hardware** and **systems software** that work together to run application programs. Specific implementations of systems change over time, but the **underlying concepts do not**. All computer systems have similar hardware and software components that perform similar functions.

This book is written for programmers who want to get better at their craft by understanding **how these components work** and **how they affect the correctness and performance** of their programs.

You are poised for an exciting journey. If you dedicate yourself to learning the concepts in this book, then you will be on your way to becoming a rare **“power programmer,”** enlightened by an understanding of the underlying computer system and its impact on your application programs.


### Key Learning Objectives

- **Numerical Accuracy:** You are going to learn practical skills such as how to avoid strange numerical errors caused by the way that computers represent numbers.

- **Code Optimization:** You will learn how to optimize your C code by using clever tricks that exploit the designs of modern processors and memory systems.

- **Security & Procedure Calls:** You will learn how the compiler implements procedure calls and how to use this knowledge to avoid the security holes from buffer overflow vulnerabilities that plague network and Internet software.

- **Linking:** You will learn how to recognize and avoid the nasty errors during linking that confound the average programmer.

- **Core Systems Construction:** You will learn how to write your own Unix shell, your own dynamic storage allocation package, and even your own Web server.

- **Concurrency:** You will learn the promises and pitfalls of concurrency, a topic of increasing importance as multiple processor cores are integrated onto single chips.


In their classic text on the C programming language [61], Kernighan and Ritchie introduce readers to C using the `hello` program shown in Figure 1.1. Although `hello` is a very simple program, **every major part of the system must work in concert** in order for it to run to completion.

In a sense, **the goal of this book is to help you understand what happens and why when you run `hello` on your system.** We begin our study of systems by tracing the lifetime of the `hello` program, from the time it is created by a programmer, until it runs on a system, prints its simple message, and terminates. As we follow the lifetime of the program, we will briefly introduce the key concepts, terminology, and components that come into play. Later chapters will expand on these ideas.


---


###### *figure 1.1*
#### C Source Code 

```C

#include <stdio.h>

int main() {
	printf("Hello World\n");
	return 0;
}

```


---

###### *figure 1.2*
#### C Code to ASCII Table

| Character | ASCII  | Character | ASCII | Character | ASCII | Character | ASCII |
| --------- | ------ | --------- | ----- | --------- | ----- | --------- | ----- |
| `#`       | 35     | `i`       | 105   | `n`       | 110   | `c`       | 99    |
| `l`       | 108    | `u`       | 117   | `d`       | 100   | `e`       | 101   |
|           | 32     | `<`       | 60    | `s`       | 115   | `t`       | 116   |
| `d`       | 100    | `i`       | 105   | `o`       | 111   | `.`       | 46    |
| `h`       | 104    | `>`       | 62    | `\n`      | 10    | `\n`      | 10    |
| `i`       | 105    | `n`       | 110   | `t`       | 116   |           | 32    |
| `m`       | 109    | `a`       | 97    | `i`       | 105   | `n`       | 110   |
| `(`       | 40     | `)`       | 41    | `\n`      | 10    | `{`       | 123   |
|           | 32     |           | 32    |           | 32    |           | 32    |
| `p`       | 112    | `r`       | 114   | `i`       | 105   | `n`       | 110   |
| `t`       | 116    | `f`       | 102   | `(`       | 40    | `"`       | 34    |
| `h`       | 104    | `e`       | 101   | `l`       | 108   | `l`       | 108   |
| `o`       | 111    | `,`       | 44    |           | 32    | `w`       | 119   |
| `o`       | 111    | `r`       | 114   | `l`       | 108   | `d`       | 100   |
| `\n`      | 92 110 | `"`       | 34    | `)`       | 41    | `;`       | 59    |
|           | 32     |           | 32    |           | 32    |           | 32    |
| `r`       | 114    | `e`       | 101   | `t`       | 116   | `u`       | 117   |
| `r`       | 114    | `n`       | 110   |           | 32    | `0`       | 48    |
| `;`       | 59     | `\n`      | 10    | `}`       | 125   |           |       |


---


#### 1.1 Information Is Bits + Context

**Our hello program begins life as a source program** (==or source file==) that the programmer creates with an editor and saves in a text file called hello.c. 


==The source program is a sequence of bits, each with a value of 0 or 1, organized in 8-bit chunks called bytes.== 

Each byte represents some text character in the program. Most computer systems represent text characters using the ASCII standard that represents each character with a unique byte-size integer value.


**For example, [[A Tour of Computer Systems - Chapter 1 CSAPP#*figure 1.2*| Figure 1.2 ]]** 

* shows the ASCII representation of the hello.c program. 
* The hello.c program is stored in a file as a sequence of bytes
* Each byte has an integer value that corresponds to some character. 

**For example**, the first byte has the integer value 35, which corresponds to the character ‘#’.  The second byte has the integer value 105, which corresponds to the character ‘i’, and so on. 

==Notice that each text line is terminated by the invisible newline character ‘\n’, which is represented by the integer value 10.==

**Files such as hello.c that consist exclusively of ASCII characters are known as text files. All other files are known as binary files**. The representation of hello.c illustrates a fundamental idea: All information in a system including disk files, programs stored in memory, user data stored in memory, and data transferred across a network is represented as a bunch of bits. 

==**The only thing that distinguishes different data objects is the context in which we view them.**== 
**==For example, in different contexts, the same sequence of bytes might represent an integer, floating-point number, character string, or machine instruction.==** 

==As programmers, we need to understand machine representations of numbers because they are not the same as integers and real numbers. They are finite approximations that can behave in unexpected ways. This fundamental idea is explored in detail in Chapter 2.==

`Nex Section is written at` : [[How GCC works under the hood -  CSAPP Chapter 1 | Here]]


![[How GCC works under the hood -  CSAPP Chapter 1#1.2 Programs Are Translated by Other Programs into Different Forms]]



`Nex Section is written at` : [[Why it's important to understand the compilation system| Here]]

![[Why it's important to understand the compilation system#1.3 It Pays to Understand How Compilation Systems Work]]


`Nex Section is written at` : [[Hardware Organization of a Typical System | Here]]

![[Hardware Organization of a Typical System#1.4 Processors Read and Interpret Instructions Stored in Memory]]