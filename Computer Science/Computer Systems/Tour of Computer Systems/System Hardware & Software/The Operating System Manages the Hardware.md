#computer-system 

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

### Over the Telnet Example

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

### Concurrency and Parallelism

Throughout the history of digital computers, two demands have been constant forces in driving improvements: 

* we want them to do more
* we want them to run faster. 

Both of these factors improve when the processor does more things at once. We use the term concurrency to refer to the general concept of a system with multiple, simultaneous activities, 

And the term parallelism to refer to the use of concurrency to make a system run faster. 

Parallelism can be exploited at multiple levels of abstraction in a computer system. We highlight three levels here, working from the highest to the lowest level in the system hierarchy.

---



> **Thread-Level Concurrency**

Building on the process abstraction, we are able to devise systems where multiple programs execute at the same time, leading to concurrency. With threads, we can even have multiple control flows executing within a single process.

Support for concurrent execution has been found in computer systems since the advent of time-sharing in the early 1960(s). Traditionally, this concurrent execution was only simulated, by having a single computer rapidly switch among its executing processes, much as a juggler keeps multiple balls flying through the air.

This form of concurrency allows multiple users to interact with a system at the same time, such as when many people want to get pages from a single Web server.

It also allows a single user to engage in multiple tasks concurrently, such as having a Web browser in one window, a word processor in another, and streaming music playing at the same time. Until recently, most actual computing was done by a single processor, even if that processor had to switch among multiple tasks.  This configuration is known as a **uni-processor system.**

When we construct a system consisting of multiple processors all under the control of a single operating system kernel, we have a **multiprocessor system.** 

Such systems have been available for large-scale computing since the 1980(s), but they have more recently become commonplace with the advent of **multi-core processors** and **hyper-threading**. 

**Figure 1.16 shows a taxonomy of these different processor types**

![](Figure1.16.png)

![](Figure1.17.png)
==Typical multi-core processor, where the chip has four CPU cores, each with its own L1 and L2 caches, and with each L1 cache split into two parts—one to hold recently fetched instructions and one to hold data. The cores share higher levels of cache as well as the interface to main memory.== 

Industry experts predict that they will be able to have dozens, and ultimately hundreds, of cores on a single chip. 

**Hyper threading, sometimes called simultaneous multi-threading,** is a technique that allows a single CPU to execute multiple flows of control. 

It involves having multiple copies of some of the CPU hardware, such as program counters and register files, while having only single copies of other parts of the hardware, such as the units that perform floating-point arithmetic. 

Whereas a conventional processor requires around 20,000 clock cycles to shift between different threads,
a hyper-threaded processor decides which of its threads to execute on a cycle-by-cycle basis. It enables the CPU to take better advantage of its processing resources.

For example, if one thread must wait for some data to be loaded into a cache, the CPU can proceed with the execution of a different thread.\

As an example, the Intel Core i7 processor can have each core executing two threads, and so a four-core
system can actually execute eight threads in parallel.

---
**The use of multiprocessing can improve system performance in two ways:**

* ==First, it reduces the need to simulate concurrency when performing multiple tasks. As mentioned, even a personal computer being used by a single person is expected to perform many activities concurrently.==

* ==Second, it can run a single application program faster, but only if that program is expressed in terms of multiple threads that can effectively execute in parallel.==

Thus, although the principles of concurrency have been formulated and studied for over 50 years, the advent of multi-core and hyper threaded systems has greatly increased the desire to find ways to write
application programs that can exploit the thread-level parallelism available with the hardware.

```
Chapter 12 will look much more deeply into concurrency and its use to provide a sharing of processing resources and to enable more parallelism in program execution
```

---

> **Instruction-Level Parallelism**


At a much lower level of abstraction, modern processors can execute multiple instructions at one time, a property known as instruction-level parallelism.

For example, early microprocessors, such as the 1978-vintage Intel 8086, required multiple (typically 3–10) clock cycles to execute a single instruction. 

More recent processors can sustain execution rates of 2–4 instructions per clock cycle. Any given
instruction requires much longer from start to finish,  perhaps 20 cycles or more, but the processor uses a number of clever tricks to process as many as 100 instructions at a time.

In Chapter 4, we will explore the use of pipe lining, where the actions required to execute an instruction are partitioned into different steps and the processor hardware is organized as a series of stages, each performing one of these steps. 

The stages can operate in parallel, working on different parts of different instructions. We will see that a fairly simple hardware design can sustain an execution rate close to 1 instruction per clock cycle.

Processors that can sustain execution rates faster than 1 instruction per cycle are known as superscalar processors. Most modern processors support superscalar operation. 

In Chapter 5, we will describe a high-level model of such processors. We will see that application programmers can use this model to understand the performance of their programs. They can then write programs such that the generated code achieves higher degrees of instruction-level parallelism and therefore runs faster.

---

> **Single-Instruction, Multiple-Data (SIMD) Parallelism**

At the lowest level, many modern processors have special hardware that allows a single instruction to cause multiple operations to be performed in parallel, a mode known as single-instruction, multiple-data (SIMD) parallelism. 

For example, recent generations of Intel and AMD processors have instructions that can add 8 pairs of single-precision floating-point numbers (C data type float) in parallel.

These SIMD instructions are provided mostly to speed up applications that
process image, sound, and video data.

Although some compilers attempt to automatically extract SIMD parallelism from C programs, a more reliable method is to write programs using special vector data types supported in compilers such as gcc.

We describe this style of programming in Web Aside opt:simd, as a supplement to
the more general presentation on program optimization found in Chapter 5

---

> **The Importance of Abstractions in Computer Systems**


