
**`An effective reverse engineer or cracker is one who fully understands the system that they are analyzing. `**

Software is designed to run in a particular environment so understanding how that environment works is very crucial and required.

This note explores the necessaries to get started in reverse engineering applications. 

==Decompilation is crucial to transforming an application from machine code to something that can be read and understood by humans. To actually analyze the resulting code, it is also necessary to understand the architecture of the computers that it is designed to run on.==

---
### **Decompilation**

==Decompilation is the easiest form of reverse engineering since it turns machine code back into source code, but the down side is that it is not always accurate so one must not fully rely on decompilation alone== 

For many programming languages, full decompilation is impossible. These languages build code directly to machine code, and some information, such as variable names, is lost in the process. While some advanced decompilers can build pseudocode for these languages, the process isn’t perfect.

##### **Just-in-time Compilation (JIT)**

==However, some programming languages use what’s called just-­in-­time (JIT) compilation==. When programs written in JIT languages are “built,” they are converted from the source code into an intermediate language (IL), not machine code.


**==JIT compilers store a copy of the code in this IL until the program is run, at which point the code is converted to machine code. Examples of JIT languages include Java, Dalvik (Android), and .NET.==**

For example, Java is well-­known for being largely platform-­agnostic, and the reason for this is its use of an IL (Java bytecode) and the Java Virtual Machine (JVM). By distributing the program code as bytecode and compiling it only at runtime, Java’s JVM 

translates from the Java IL to machine code specific to the machine it’s running on. While this approach can negatively impact file size and performance, it pays off in portability. JIT compilation also makes reverse engineering these applications much
easier. 

**==These intermediate languages are similar enough to the original source code that they can be decompiled or converted back into usable source code**. Source code is designed to be human-­readable, making it far easier to understand the application’s logic and identify software protections or other embedded secrets.==

**`To reverse engineer such, you must know how to use JetBrains Dotpeek and modify .NET application.`**

Decompiling is a powerful and easy approach to understanding and modifying a program. However, it doesn’t work on every program. While programs written in languages such as C/C++ can be decompiled using tools such as IDA’s Hex-­Rays Decompiler or Ghidra, the result is often low-­quality and difficult to use.

When developing applications that contain sensitive information or that you don’t want modified, it’s better to use a language that isn’t easily decompiled. For example, C/C++ is a better choice for sensitive functionality than a .NET language such as C#.

---

### **Computer Architecture**

It's generally known that an average programmer doesn't need an in-depth understanding of how computers work, When writing a program in a high level-language, the operating system handles all of the low-level operations. 

Processes appear to have their own contiguous memory spaces, and files are just a sequence of bytes to read and write.
However, none of this is actually true, and your operating system has been abstracting the truth from you (to make it easier to program). 

**`A solid understanding of how computer architecture actually works is essential for a reverse engineer.** 

Click here to read more about Operating System abstractions **[[The Operating System Manages the Hardware| here ]]**
Click here to know about **[[Von-Neumann architecture]]**

---



