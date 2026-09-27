#CSAPP 


#### 1.7 The Operating System Manages the Hardware

Back to our [[A Tour of Computer Systems - Chapter 1 CSAPP#*figure 1.1* | hello world program]] example. When the shell loaded and ran the hello program, and when the hello program printed its message, neither the program accessed the keyboard, display, or main memory directly.


##### *figure 1.10*
![no-float](Figure1.10.png)


##### *figure 1.11*
![700](Figure1.11.png)


Rather, they relied on the services provided by the [[Operating system]]. We can think 
We can think of the operating system as a layer of software interposed between the application program and the hardware as shown in [[The Operating System Manages the Hardware#*figure 1.10*| Figure 1.10]].

All attempts by an application program to manipulate the hardware must go through the operating system

**The operating system has two primary purposes:** 

1. To protect the hardware from misuse by runaway applications.

2. To provide applications with simple and uniform mechanisms for manipulating  complicated and often wildly different low-level hardware devices

The operating system achieves both goals via the fundamental abstractions shown in [[The Operating System Manages the Hardware#*figure 1.11* | Figure 1.1]]

[[Processes]], [[Virtual Memory]], and [[Files]]. As this figure suggests, Files are abstractions for [[IO Devices]], Virtual Memory is an abstraction for both the [[Main Memory]] and disk [[IO Devices]], and [[Processess]] are abstractions for the [[Central Processing Unit (CPU) | Processor]], [[Main Memory]] and [[IO Devices]].


[^2]: this is how processess work
