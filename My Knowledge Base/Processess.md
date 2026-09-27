#CSAPP 


#### 1.7.1 Processes

When a program such a [hello world program](app://obsidian.md/A%20Tour%20of%20Computer%20Systems%20-%20Chapter%201%20CSAPP#*figure%201.1*) runs on a modern system, The operating system provides the illusion that the program is the only one running on the system.

==The program appears to have exclusive use of both the Processor, Main memory and IO Devices==

`The processor appears to execute the instruction in the program, one after other, without interruptions. And the code and data of the program appears to be the only objects in the system's memory.`

These illusions are provided by the notion of a process, one of the most important and successful ideas in computer science.

**A Process is the operating system's abstraction for running program.**

==Multiple processes can run concurrently on the same system, and each process appears to have exclusive use of the hardware.==

By concurrently we mean that the instruction of one process are interleaved with the instructions of another process. In most systems, there are more processes to run than there are CPU(s) to run them.

Traditional systems could only execute one program at a time, while newer multi-core processors can execute several programs simultaneously.

In either case, a single CPU can appear to execute multiple processes concurrently by having the processor switch among them.

The operating system performs this interleaving with a mechanism known as context switching.

To simplify the rest of this discussion, we consider only a uni processor system containing a single CPU. We will return to the discussion of multiprocessor systems in Section 1.9.2.

The operating system keeps track of all the state information that the process needs in order to run. This state, which is known as the [context](app://obsidian.md/Process%20Context), includes information such as the current values of the PC, the register file, and the contents of main memory.

At any point in time, a uni processor system can only execute the code for a single process. When the operating system decides to transfer control from the current process to some new process, it performs a context switch by saving the context of the current process, restoring the context of the new process, and then passing control to the new process. The new process picks up exactly where it left off.


---
#### Process Context Switch
###### *Figure 1.12* 

Figure 1.12 shows the basic idea for our example hello scenario.


![](Figure1.12.png)



There are two concurrent processes in our example scenario, The shell process and the hello process. Initially, the shell process is running alone, waiting for input on the command line. When we ask it to run the hello program, the shell carries out our request by invoking a special function known as a system call that passes control to the operating system. 

The operating system saves the shell’s context, creates a new hello process and its context, and then passes control to the new hello process. 

After hello terminates, the operating system restores the context of the shell process and passes control back to it, where it waits for the next command-line input.

As Figure 1.12 indicates, the transition from one process to another is managed by the operating system kernel. 

The [[Kernel]] is the portion of the operating system code that is always resident in memory. When an application program requires some action by the operating system, such as to read or write a file, it executes a special system call instruction, transferring control to the kernel.

The kernel then performs the requested operation and returns back to the application program.

---
> **NOTE** 
```
That the kernel is not a separate process. Instead, it is a collection of code
and data structures that the system uses to manage all the processes.
```
---

Implementing the process abstraction requires close cooperation between both the low-level hardware and the operating system software. We will explore how this works, and how applications can create and control their own processes, in Chapter 8.