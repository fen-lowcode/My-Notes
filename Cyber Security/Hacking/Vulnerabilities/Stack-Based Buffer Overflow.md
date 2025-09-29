

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


