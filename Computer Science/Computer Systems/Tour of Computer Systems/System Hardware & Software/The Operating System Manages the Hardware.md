#computer-system #programming 

Back to our [[Hello World Program]] example. ==When the shell loaded and ran the hello program, and when the hello program printed its message, neither the program accessed the keyboard, display, or main memory directly.==

![](Figure1.10.png)
![](Figure1.11.png)

Rather, ==they relied on the services provided by the **[[Operating system]]**==. We can think 
We can ==think of the operating system as a layer of software interposed between the application program and the hardware== as **shown in Figure 1.10.**

==All attempts by an application program to manipulate the hardware must go through the operating system==

**The operating system has two primary purposes:** 

* **(1)** ==To protect the hardware from misuse by runaway applications==

* **(2)** ==To provide applications with simple and uniform mechanisms for manipulating  complicated and often wildly different low-level hardware devices==

The ==operating system achieves both goals via the fundamental abstractions== **shown in Figure 1.11**

==**[[Processes]], [[Virtual memory]], and [[Files]]**.== As this figure suggests, **==Files** are abstractions for **[[IO Devices]]**,== ==**Virtual Memory** is an abstraction for both the **[[Main Memory]] and disk [[IO Device]]==,** and **Processes** are abstractions for the **[[Processor]], [[Main Memory]], [[Files]].**

---

### Processes

When a program such a [[Hello World Program]] runs on a modern system, **==The operating system provides the illusion that the program is the only one running on the system.==**  

==The program appears to have exclusive use of both the Processor, Main memory and IO Devices==

==The processor appears to execute the instruction in the program, one after other, without interruptions==. And the ==code and data of the program appears to be the only objects in the system's memory==.

==These illusions are provided by the notion of a process==, one of the most important and successful ideas in computer science.

==A **Process** is the operating system's abstraction for running program==.

==Multiple processes can run concurrently on the same system==, and ==each process== appears to ==have exclusive use of the hardware==.

By ==concurrently we mean that the instruction of one process are interleaved with the instructions of another process==. In most systems, ==there are more processes to run than there are CPU(s) to run them==.

Traditional systems could only execute  one program at a time, ==while newer **multi-core processors** can execute several programs simultaneously.== 

In either case, ==a single CPU can appear to execute multiple processes concurrently by having the processor switch among them.==

The ==operating system performs this interleaving== with a mechanism ==known as **context switching**.==

 To simplify the rest of this discussion, we consider only a uni processor system containing a single CPU. We will return to the discussion of multiprocessor systems in Section 1.9.2.
 
**==The operating system keeps track of all the state information that the process needs in order to run**.== This state, ==which is known as the context, includes information such as the current values of the PC, the register file, and the contents of main memory.== 

At any point in time, ==a uni processor system can only execute the code for a single process.== **When the operating system decides to transfer control from the current process to some new process,** ==it performs a context switch by saving the context of the current process, restoring the context of the new process, and then passing control to the new process. The new process picks up exactly where it left off.==

---

**Figure 1.12** shows the basic idea for our example hello scenario.

![](Figure1.12.png)


**There are two concurrent processes in our example scenario:** 

==The shell process and the hello process. Initially, the shell process is running alone, waiting for input on the command line.== When we ask it to run the hello program, ==the shell carries out our request by invoking a special function known as a system call that passes control to the operating system.== 

==The operating system saves the shell’s context, creates a new hello process and its context, and then passes control to the new hello process.== 

==After hello terminates, the operating system restores the context of the shell process and passes control back to it, where it waits for the next command-line input.==

As Figure 1.12 indicates, the transition from one process to another is managed by the ***operating system kernel.*** 

---

==The **[[Kernel]]** is the portion of the operating system code== that is always resident in memory. ==When an application program requires some action by the operating system, such as to read or write a file, it executes a special system call instruction, transferring control to the kernel.==

The kernel then performs the requested operation and returns back to the application program.

---
> **NOTE** 
```
That the kernel is not a separate process. Instead, it is a collection of code
and data structures that the system uses to manage all the processes.
```
---

**==Implementing the process abstraction requires close cooperation between both the low-level hardware and the operating system software.==** We will explore how this works, and how applications can create and control their own processes,
in Chapter 8.

---
### Threads

**Although we normally think of a process as having a single control flow**, ==in modern systems a process can actually consist of multiple execution units,==
==called ***[[Threads]]***, each running in the context of the process and sharing the same code and global data.==

==Threads are an increasingly important programming model because of the requirement for concurrency in network servers, because it is easier to share data between multiple threads than between multiple processes, and because threads are typically more efficient than processes.== 

==***[[Multi-threading]]*** is also one way to make programs run faster when multiple processors are available.==

![](Figure1.13.png)

> **Note** 

Refer to **[[Concurrency and Parallelism]]** for further explanation.

---

### Virtual Memory 


***[[Virtual memory]]*** is an ==abstraction that provides each process with the illusion that it has exclusive use of the main memory==. 

==Each process has the same uniform view of memory==, which is known as its **virtual address space.** The virtual address space for Linux processes is **shown in Figure 1.13.**(Other Unix systems use a similar layout.)

In Linux, the topmost region of the address space is reserved for code and data in the operating system that is common to all processes

The lower region of the address space holds the code and data defined by the user’s process Note that addresses in the figure increase from the bottom to the top.

The virtual address space seen by each process consists of a number of well-defined areas, each with a specific purpose.

* ==**Program code and data.**== Code begins at the same fixed address for all processes,followed by data locations that correspond to global C variables. The code and data areas are initialized directly from the contents of an executable object file—in our case, the hello executable. You will learn more about this part of the address space when we study linking and loading in Chapter 7.

* ==**Heap.**== The code and data areas are followed immediately by the run-time heap.
  Unlike the code and data areas, which are fixed in size once the process begins running, the heap expands and contracts dynamically at run time as a result
  of calls to C standard library routines such as malloc and free. We will study
  heaps in detail when we learn about managing virtual memory in Chapter 9.

* ==**Shared libraries.**== Near the middle of the address space is an area that holds the
  code and data for shared libraries such as the C standard library and the math
  library. The notion of a shared library is a powerful but somewhat difficult
  concept. You will learn how they work when we study dynamic linking in
  Chapter 7.

* ==**Stack**.== At the top of the user’s virtual address space is the user stack that
  the compiler uses to implement function calls. Like the heap, the user stack
  expands and contracts dynamically during the execution of the program. In
  particular, each time we call a function, the stack grows. Each time we return
  from a function, it contracts. You will learn how the compiler uses the stack
  in Chapter 3.

* ==**Kernel virtual memory**.== The top region of the address space is reserved for the
  kernel. Application programs are not allowed to read or write the contents of
  this area or to directly call functions defined in the kernel code. Instead, they
  must invoke the kernel to perform these operations.

==For virtual memory to work, a sophisticated interaction is required between==
==the hardware and the operating system software,== including a hardware translation
of every address generated by the processor. The basic idea is to store the contents
of a process’s virtual memory on disk and then use the main memory as a cache
for the disk. Chapter 9 explains how this works and why it is so important to the
operation of modern systems

---

### Files

==A file is a sequence of bytes, nothing more and nothing less. Every I/O device,==
==including disks, keyboards, displays, and even networks, is modeled as a file.== 

==All input and output in the system is performed by reading and writing files==, ==using== a
==small set of system calls known as Unix I/O.==

This simple and elegant notion of a file is nonetheless very powerful because it provides applications with a ==uniform view of all the varied I/O devices that might be contained in the system.== 

For example, ==application programmers who manipulate the contents of a disk file are blissfully unaware of the specific disk technology==. Further, ==the same program will run on different systems that use different disk technologies==. You will learn about Unix I/O in Chapter 10.

---

### Systems Communicate with Other Systems Using Networks

Up to this point in our tour of systems, we have treated a system as an isolated
collection of hardware and software. In practice, ==modern systems are often linked==
==to other systems by networks.== 

==From the point of view of an individual system, network can be viewed as just another I/O device,== as **shown in Figure 1.14**. 

When ==the system copies a sequence of bytes from main memory to the network
adapter==, ==the data flow across the network to another machine==, instead of, say, to a local disk drive. 

Similarly, ==the system can read data sent from other machines and copy these data to its main memory.==

![](Figure1.14.png)


With the advent of global networks such as the Internet, ==copying information from one machine to another has become one of the most important uses of computer systems.==

For example, applications such as email, ==instant messaging, the World Wide Web, FTP, and telnet are all based on the ability to copy information over a network.==

Returning to our hello example, ==we could use the familiar telnet application==
==to run hello on a remote machine.==

**Suppose we use a telnet client running on our local machine to connect to a telnet server on a remote machine.**

 > **Over the Telnet Example**

![](Figure1.15.png)

After we log in to the remote machine and run a shell, the remote shell is waiting to receive an input command.  
From this point, running the `hello` program remotely involves the **five basic steps** shown in *Figure 1.15*:

* We type in the `hello` string to the **telnet client** and hit the Enter key.
* The client sends the string to the **telnet server**.
* The telnet server receives the string and passes it to the **remote shell program**.
* The remote shell runs the `hello` program and passes the **output** to the telnet server.
* The telnet server forwards the output string across the network to the telnet client, which prints it on our **local terminal**.

This type of exchange between clients and servers is typical of all **network applications.**

In Chapter 11 you will ==**Learn how to build network applications and apply this knowledge to build a simple Web server.**==


---

### Conclusion

This concludes our initial whirlwind tour of systems. ==An important idea to take away from this discussion is that a system is more than just hardware.== 

==It is a collection of intertwined hardware and systems software that must cooperate in order to achieve the ultimate goal of running application programs.== 

The rest of this notes will fill in some details about the hardware and the software, and it will show how, by knowing these details, you can write programs that are faster, more reliable, and more secure.


---

#### Concurrency and Parallelism

==Throughout the history of digital computers, two demands have been constant forces in driving improvements:== 

* ==we want them to do more==
* ==we want them to run faster.== 

Both of these factors improve when the processor does more things at once. We use the term concurrency to refer to the general concept of a system with multiple, simultaneous activities, 

And the term parallelism to refer to the use of concurrency to make a system run faster. 

Parallelism can be exploited at multiple levels of abstraction in a computer system. We highlight three levels here, working from the highest to the lowest level in the system hierarchy.

**For more information**: [[Concurrency and Parallelism]]

---

#### **The Importance of Abstractions in Computer Systems**

The use of abstractions is one of the most important concepts in computer science. For example, one aspect of good programming practice is to formulate a simple application program interface (API) for a set of functions that allow programmers to use the code without having to delve into its inner workings.

![](virtual-memory-abstraction.png)

==Different Programming languages provide different forms and levels of support for abstraction, such as Java class declarations and C function prototypes.== 

We have already been introduced to several of the abstractions seen in computer systems, as indicated in `Figure 1.18.` ==On the processor side, the instruction set architecture provides an abstraction of the actual processor hardware. With this abstraction, a machine-code program behaves as if it were executed on a processor that performs just one instruction at a time.== 

==The underlying hardware is far more elaborate, executing multiple instructions in parallel, but always in a way that is consistent with the simple, sequential model.== 

**By keeping the same execution model, different processor implementations can execute the same machine code while offering a range of cost and performance.** 

==On the operating system side, we have introduced three abstractions: files as an abstraction of I/O devices, virtual memory as an abstraction of program memory, and processes as an abstraction of a running program.== 

To these abstractions we add a new one: **the virtual machine, providing an abstraction of the entire computer, including the operating system, the processor, and the programs.** 

The idea of a virtual machine was introduced by IBM in the 1960s, but it has become more prominent recently as a way to manage computers that must be able to run programs designed for multiple operating systems (such as Microsoft Windows, Mac OS X, and Linux) or different versions of the same operating system.

---


> [!Summary of the important of abstraction]

- **Abstraction** hides complexity. You use a feature without worrying about how it works inside.
- **Programming example:** An API lets you call functions without reading all the code behind them.
- **Languages help:**

    - Java → classes        
    - C → function prototypes

> **Abstractions in Computer Systems**

**Processor side:**

- Instruction Set Architecture (ISA) makes hardware look simple.
- A program acts like it runs one instruction at a time.
- Reality: hardware executes many instructions in parallel but still keeps results consistent.
- Benefit: the same program runs on different processors with different performance levels.

**Operating system side:**

- **Files** = abstraction of input/output devices.
- **Virtual memory** = abstraction of program memory.
- **Processes** = abstraction of a running program.
- **Virtual machine** = abstraction of the whole computer (OS, CPU, programs).

---

### Summary

**==A computer system consists of hardware and systems software that cooperate to run application programs.==** 

==Information inside the computer is represented as groups of bits that are interpreted in different ways, depending on the context.== 

==Programs are **translated by other programs into different forms, beginning as ASCII text and then translated by compilers and linkers into binary executable files.==**

**==Processors read and interpret binary instructions that are stored in main memory**. Since computers spend most of their time copying data between memory, I/O devices, and the CPU registers, **the storage devices in a system are arranged in a hierarchy, with the CPU registers at the top, followed by multiple levels of hardware cache memories, DRAM main memory, and disk storage.==**  

**==Storage devices that are higher in the hierarchy are faster and more costly per bit than those lower in the hierarchy.==** ==Storage devices that are higher in the hierarchy serve as caches for devices that are lower in the hierarchy. 

***Programmers can optimize the performance of their C programs by understanding and exploiting the memory hierarchy.==*** 

*`Refer to:` [[Storage Devices Hierarchy]] [[Cache Memory]]*

**The operating system kernel serves as an intermediary between the application and the hardware**. I==t provides three fundamental abstractions: (1) Files are abstractions for I/O devices. (2) Virtual memory is an abstraction for both main memory and disks. (3) Processes are abstractions for the processor, main memory, and I/O devices.== 

**Finally, networks provide ways for computer systems to communicate with one another.** ==From the viewpoint of a particular system, the network is just another I/O device.==