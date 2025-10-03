

**==Memory exception==**, or as know by memory fault or memory access violation is a runtime error when a program attempts to access an invalid memory or a memory where it has no permission to access, this is the operating system's reaction to such events.

During this error, a program can cause to be terminate unexpectedly or behave in a unpredictable manner. 

**This is responsible for most of the security vulnerabilities in program flows in the last decade**.

Programming errors often occur, leading to [[Buffer Overflow Overview|Buffer Overflow]] due to inattention when programming with low abstract languages such as C or C++.

These languages are often compiled down almost directly to machine code compared to highly abstracted programming languages such as Java, Python or C# managed runtimes provide runtime safety checks.

==Buffer overflows is an error occur when an input buffer of a process's memory is handling a larger data than what it is expected to causing it to overflow and potentially overwriting other instructions of the program causing a security vulnerability==.

Such a program (Binary Files), is a general executable file stored on a data storage medium. There are multiple different file format for an executable binary files such as the [[Portable Executable Format (PE)]] and [[Executable and Linking Format]](ELF).

==The **PE** format is used by Microsoft platforms and the **ELF** is supported by almost all [[Unix System|Unix]] variants.==

If the [[Loader]] loads such an executable binary file, the program will be executed, the corresponding program code will be loaded into the [[Main Memory]] and then executed by the CPU.

During initialization and execution, the program's data and instructions is stored in memory, these data are displayed in the running software or entered by the user.

Especially for expected user input, a buffer must be created beforehand by saving the input.

==How the instruction works also directly affect how the program runs, for example return addresses refers to other addresses in memory which defines the control flow of the program.== **if an return address is intentionally overwritten by using a buffer overflow, this can or may changed the program's control flow by attempting to direct the next instruction to another function or subroutine, it also may be possibly jump back to a code introduced by the user input.**

To understand how it technically works, we need to be familiar with how:

* How the Virtual Memory is divided and used
* How debuggers displays and names individual instructions.
* How the debugger can be used to detect such vulnerabilities.
* How we can manipulate the virtual memory.

---

![[The Virtual Memory]] 

---
#### The Stack

**Rewrite !!**

==`Stack memory` is a `Last-In-First-Out` data structure== in which the return addresses, parameters, and, depending on the compiler options, frame pointers are stored. `C/C++` local variables are stored here, and you can even copy code to the stack. The `Stack` is a defined area in `RAM`. The linker reserves this area and usually places the stack in RAM's lower area above the global and static variables. The contents are accessed via the `stack pointer`, set to the upper end of the stack during initialization. During execution, the allocated part of the stack grows down to the lower memory addresses.

Modern memory protections (`DEP`/`ASLR`) would prevent the damage caused by buffer overflows. DEP (Data Execution Prevention), marked regions of memory "Read-Only". The read-only memory region is where some user-input is stored (Example: The Stack), so the idea behind DEP was to prevent users from uploading shellcode to memory and then setting the instruction pointer to the shellcode. Hackers started utilizing ROP (Return Oriented Programming) to get around this, as it allowed them to upload the shellcode to an executable space and use existing calls to execute it. With ROP, the attacker needs to know the memory addresses where things are stored, so the defense against it was to implement ASLR (Address Space Layout Randomization) which randomizes where everything is stored making ROP more difficult.

Users can get around ASLR by leaking memory addresses, but this makes exploits less reliable and sometimes impossible. For example the ["Freefloat FTP Server"](https://www.exploit-db.com/exploits/46763) is trivial to exploit on Windows XP (before DEP/ASLR). However, if the application is ran on a modern Windows operating system, the buffer overflow exists but it is currently non-trivial to exploit due to DEP/ASLR (as there's no known way to leak memory addresses.)

