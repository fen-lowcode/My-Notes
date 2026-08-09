---
linux:
---

==The kernel is the core of an operating system. In a traditional design, it is responsible for memory management, I/O, interrupt handling, and various other things.==

When hardware and software needs to interact with each other then the kernel will act as a middle-man for both, the kernel is the interface between software and hardware to interact with each other.

#### **Where the kernel fits in an operating system**

You can break most [operating systems](https://www.codecademy.com/resources/blog/operating-system/) into these three layers:

- **[[Hardware Organization of a Typical System | Hardware]]** This comprises the physical parts of a computer, including its **RAM**, **[[Central Processing Unit (CPU) | CPU ]]** and input/output devices like hard drives, network devices, and graphic cards. The CPU performs the computations for the computer and reads and writes memory.

- **Kernel**. The kernel is the core of the operating system. It is software that stays in memory and it sends instructions to the CPU and other hardware.

- **User Processes**. User processes (or just “processes”) are the running applications in the computer. Another of the kernel’s jobs is to manage these processes and allow them to communicate with each other.

Kernel mode and user mode are the two ways a computer’s **CPU** can execute code. When code is run in kernel mode, it has unlimited access to all the hardware on the computer. User mode restricts this access to the CPU. The computer’s memory is also separated this way and divided into user space and kernel space.

Because of this separation, when a process fails while running in user mode, the damage it can inflict on the system is minimal, and the kernel can recover the process. A process that runs in kernel mode, on the other hand, can crash the computer. This separation of user and kernel processes is a safeguard that keeps the system running and stable even if a user process fails.

#### **What is kernel panic?**

Errors and crashes can still happen on a computer system, even with this separation between user and kernel processes. Since the kernel has complete control over the system, a crash in a kernel process will bring down the entire system. ==A kernel panic is a safety measure that an operating system takes when it detects a crash or if continuing to run could cause data loss or hardware damage.==

Kernel panics are usually caused by communication issues between devices and hardware. To recover a system in kernel panic, it must be rebooted.

The term “kernel panic” is used in Unix, [Linux](https://www.codecademy.com/article/linux-os), Mac OS X, and other Unix-like systems. On the Microsoft Windows operating system, the same type of event is called a “stop error,” but it’s more commonly called the Blue Screen of Death (BSOD).

#### **What are the functions of a kernel?**

The kernel performs many jobs in a computer system. Its main responsibilities include:

- **Resource management**: The kernel allocates a computer’s resources between the various applications running on it and also controls communication between processes.
- **Memory management**: The kernel has complete control of a computer’s memory or RAM. Every computer process requires some memory, and the kernel uses virtual addressing to allocate memory to running processes. Virtual addressing segments the memory and keeps the memory used by each process from overlapping the memory used by other processes.
- **Device management**: The kernel keeps a list of available devices connected to the system and communicates with these devices using their drivers. Device drivers are computer applications that run in kernel mode and allow the OS to communicate with devices like mice, keyboards, and monitors. The kernel manages all communications between user processes and these devices via these drivers.


## **Types of kernel**

While the features of kernels are similar, the designs of specific kernels can vary widely depending on what they are used for. Kernels come in five main types.

### **Monolithic kernel**

In a monolithic kernel, there’s no separation between user mode and kernel mode. Both user processes and kernel processes use the same memory space and run in the same thread.

Most of the work in a monolithic kernel is done with direct system calls. This gives applications quick access to hardware and resources and can increase the performance of user processes. Monolithic kernels are also smaller applications and take up less memory since they don’t have to handle both user and kernel modes.

But, monolithic kernels also have a few drawbacks. If a process crashes, it can bring down the whole system since it doesn’t have the safeguards of a separate kernel mode. Also, to add new services to the operating system, the operating system has to be modified and rebooted.

[AIX](https://www.ibm.com/products/aix), [HP-UX](https://www.hpe.com/us/en/servers/hp-ux.html), [Linux](https://www.linux.org/), [Unix](https://unix.org/), and [Solaris](https://www.oracle.com/solaris/solaris11/) all use monolithic kernels.

### **Micro kernel**

In a micro kernel, the user processes are executed in the user space, and system processes are executed in the kernel space. Processes in the user space can make calls to various operating system services to obtain access to system resources like RAM. Other services like networking run as programs in the userspace.

Micro kernels are easier to maintain than monolithic kernels, but the number of system calls and message passing will come with more overhead and make the system slower. Still, micro kernels can recover when user processes crash, and new software can be tested on them without having to reboot the system.

Mach, L4, AmigaOS, Minix, and K42 all use micro kernels.

### **Hybrid kernel**

Hybrid kernels are also known as modular kernels and combine the features of both monolithic and micro kernels. They’re used in most commercial operating systems, and they extend the functionality of micro kernels with additional code in kernel space to increase performance.

[Windows](https://www.microsoft.com/en-us/windows?r=1), [Netware](http://www.novell.com/documentation/nw65/os_svr_adm_nw/data/ac18nsa.html), and BeOS all use hybrid kernels.

### **Nano kernel**

A nano kernel is extremely small and only provides direct access to hardware and resources. To save memory space and increase performance, nano kernels delegate all services to device drivers and don’t have any system services. The Mac OS operating system used a nano kernel in its Power PC days for versions 7 through 9 before the OS X versions.

### **Exo kernel**

Exo kernels are an experimental kernel design being developed at MIT. In this kernel, the management of resources is separated from their protection. This allows the kernel to be customized for specific applications. They provide no abstractions on top of the hardware they interact with, which allows developers to decide how to allocate resources to applications running on the system.

Exo kernels are also really small but provide developers with a library operating system that has all the functionality of a traditional operating system. This means that users of an exo kernel must build the rest of the operating system from scratch, which can be complex and comes with a lot of risks. This is a major reason why exo kernels have not become mainstream.
