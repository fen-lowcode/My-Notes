#operaing-systems #linux 

[[What is a Kernel]]  [[Process Context]] 

==Whereas the user interface is the outer-most portion of the operating system, the kernel is the innermost.== 

It is the core internals the software that provides basic services for all other parts of the system, manages hardware, and distributes system resources.

The kernel is sometimes referred to as the ==supervisor, core, or internals of the operating system.==

Typical components of a kernel are :

- interrupt handlers to service interrupt requests
- a scheduler to share processor time among multiple processes
- a memory management system to manage process address spaces	 
- system services such as networking and interprocess communication.

---


On modern systems with protected memory management units, the kernel typically resides in an elevated system state compared to normal user applications.

`This includes a protected memory space and full access to the hardware`

This system state and memory-space is reffered to as the **Kernel Space**, while the user application software resides and executes to **User Space** 

==When executing kernel code, the system is in kernel-space executing in kernel mode. When running a regular process, the system is in user-space executing in user mode.==

==Applications running on the system communicate with the kernel via system calls (see Figure 1.1)==

An application typically calls functions in a library—for example, the C library that in turn rely on the system call interface to instruct the kernel to carry out tasks on the application’s behalf.

![[Pasted image 20260201201005.png]]


Some libraries calls provide many features not found in the system call table but rather a collection of system calls, take `printf()` for example where `write()` syscall is just one of it's steps and some libraries just directly calls a syscall and acting as a wrapper like `open()` 

And take note that not all C libraries use syscall like for example `strcpy()`.

==**When an application executes a system call, we say that the kernel is executing on behalf of the application.**== 

Furthermore, the application is said to be executing a system call in kernel-space, and the kernel is running in process context.

---

#### **Kernel Managing system hardware**

The kernel also manages the system’s hardware. Nearly all architectures, including all systems that Linux supports, provide the ==concept of interrupts==.

==When hardware wants to communicate with the system, it issues an interrupt that literally interrupts the processor==, which in turn interrupts the kernel.

==A number identifies interrupts and the kernel uses this number to execute a specific **interrupt handler**== to process and respond to the interrupt.

For example, ==as you type, the keyboard controller issues an interrupt to let the system know that there is new data in the keyboard buffer.==

The kernel notes the interrupt number of the incoming interrupt and executes the correct interrupt handler.

The interrupt handler processes the keyboard data and lets the keyboard controller know it is ready for more data. ==To provide synchronization, the kernel can disable interrupts either all interrupts or just one specific interrupt number.== 

In many operating systems, including Linux, ==the interrupt handlers do not run in a process context. Instead, they run in a special interrupt context that is not associated with any process==. **This special context exists solely to let an interrupt handler quickly respond to an interrupt, and then exit.**


---

#### **Kernel's mode of activity**

These contexts represent the breadth of the kernel’s activities. In fact, in Linux, we can generalize that each processor is doing exactly one of three things at any given moment:


* **==In user-space, executing user code in a process==**
* **==In kernel-space, in process context, executing on behalf of a specific process==**
* **==In kernel-space, in interrupt context, not associated with a process, handling an interrupt==**


---

#### **Linux compared to class Unix kernels** 


==Owing to their common ancestry and same API, modern Unix kernels share various==
==design traits.== With few exceptions, a Unix kernel is typically a monolithic static binary.

That is, it exists as a single, large, executable image that runs in a single address space. ==Unix systems typically require a system with a paged memory management unit (MMU); this hardware enables the system to enforce memory protection and to provide a unique virtual address space to each process.==


`refer to for more details about`:  [[Memory Management Unit (MMU)]]  `and` [[Address Space]] 

Linux historically has required an MMU, but ==special versions of linux can actually run without one. This is a neat feature, enabling Linux to run on very small MMU-less embedded systems==, but otherwise more academic than practical even simple embedded systems nowadays tend to have advanced features such as memory-management units. 

As Linus and other kernel developers contribute to the Linux kernel, they decide how best to advance Linux without neglecting its Unix roots (and, more important, the Unix API). 

==Consequently, because Linux is not based on any specific Unix variant, Linus and==
==company can pick and choose the best solution to any given problem—or at times,==
==invent new solutions.==

A handful of notable differences exist between the Linux kernel and classic Unix systems:


* ==Linux supports the dynamic loading of kernel modules==. Although the Linux kernel is monolithic, it can dynamically load and unload kernel code on demand.

* ==Linux has symmetrical multiprocessor (SMP) support ==.Although most commercial variants of Unix now support SMP, most traditional Unix implementations did not.

* ==The Linux kernel is preemptive==. Unlike traditional Unix variants, the Linux kernel can preempt a task even as it executes in the kernel. Of the other commercial Unix implementations, Solaris and IRIX have preemptive kernels, but most Unix kernels are not preemptive.

* ==Linux takes an interesting approach to thread support: It does not differentiate between threads and normal processes.== To the kernel, all processes are the same some just happen to share resources.

* ==Linux provides an object-oriented device model with device classes, hot-pluggable events, and a user-space device filesystem (sysfs).==

* Linux ignores some common Unix features that the kernel developers consider poorly designed, such as STREAMS, or standards that are impossible to cleanly implement.

* Linux is free in every sense of the word.The feature set Linux implements is the result of the freedom of Linux’s open development model. If a feature is without merit or poorly thought out, Linux developers are under no obligation to imple- ment it. To the contrary, Linux has adopted an elitist attitude toward changes: Modifications must solve a specific real-world problem, derive from a clean design, and have a solid implementation. Consequently, features of some other modern Unix variants that are more marketing bullet or one-off requests, such as pageable kernel memory, have received no consideration.


==Despite these differences, however, Linux remains an operating system with a strong Unix heritage.==

