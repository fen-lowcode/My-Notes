#CSAPP 


#### 1.4 Processors Read and Interpret Instructions Stored in Memory

==At this point, our hello.c source program has been translated by the compilation system into an executable object file called hello that is stored on disk.== To run the executable file on a Unix system, we type its name to an application program known as a shell:

``` bash

linux> ./hello 
hello, world 
linux>

```



==The shell is a command-line interpreter that prints a prompt,== waits for you to type a command line, and then performs the command. If the first word of the command line does not correspond to a built-in shell command, then the shell assumes that it is the name of an executable file that it should load and run. 



##### *`figure 1.4`* 

![](Figure1.4.png)


So in this case, the shell loads and runs the hello program and then waits for it to terminate. The hello program prints its message to the screen and then terminates. The shell then prints a prompt and waits for the next input command line.


#### 1.4.1 Hardware Organization of a System 

To understand what happens to our hello program when we run it, we need to understand the hardware organization of a typical system, which is shown in Figure 1.4. This particular picture is modeled after the family of recent Intel systems, but all systems have a similar look and feel. Don’t worry about the complexity of this figure just now. We will get to its various details in stages throughout the course of the book.


---

This architecture follows the principles of **[[Von-Neumann architecture]]**



* **Buses**

	* ==Running throughout the system is a collection of electrical conduits called== **Buses** that carries bytes of information back and forth between the components.  
	
	* Buses are typically designed to transfer fixed-sized chunks known as ***words***. The number of bytes in a word (the word size) is a fundamental system parameter that varies across systems. Most machines today have word size of either 4 bytes (32 bits) or 8 bytes(64 bits),  
	
	* In this book, we do not assume any fixed definition of word size. Instead, we will specify what we mean by a “word” in any context that requires this to be defined.



* **[[IO Devices]]**


	* ==Input/Output (I/O) devices are the systems connection to the external world.== Our example system has four I/O devices: **Keyboard** and **Mouse** for user input, a **Display** for user output, and a **Disk Drive** (or simply Disk) for long-term storage of data and programs. Initially, the executable file resides on the disk.
	
	* ==Each I/O device is connected to the I/O bus by either a controller or an adapter.== The distinction between the two is mainly one of packaging.
	
	* ==Controllers are chip sets in the device itself== or on the system’s main printed circuit board  (often called the ***[[Motherboard]]***). 
	
	* ==An adapter is a card that plugs into a slot on the motherboard==. Regardless, the purpose of each is to transfer information back and forth between the I/O bus and an I/O device.
	
	Chapter 6 has more to say about how I/O devices such as disks work.  In Chapter 10, you will learn how to use the Unix I/O interface to access devices from your application programs. We focus on the especially interesting class of devices known as networks, but the techniques generalize to other kinds of devices as well.

---
 * ***[[Main Memory]]**

	* The **main memory** is a ==temporary storage device that holds both a program and the data it manipulates while the processor is executing the program==. 
	
	* Physically, ==main memory consists of a collection of dynamic random access memory==  ( [[DRAM]] ) chips. 
	
	* ==Logically, **memory is organized as a linear array of bytes==**, each with its own unique address  ( [[array index]] )  starting at zero. ==In general, each of the machine instructions that constitute a program can consist of a variable number of bytes.== 
	
	* ==The sizes of data items that correspond to C program variables vary according to type==. For example, on an ***[[x86-64]]*** machine running Linux, data of type short require 2 bytes, types int and float 4 bytes, and types long and double 8 bytes.

Chapter 6 has more to say about how memory technologies such as DRAM chips work, and how they are combined to form main memory.

---

> **[[Central Processing Unit (CPU)]]**

==The **central processing unit** (***CPU*** ), or simply processor, is the engine that interprets (or executes) instructions stored in main memory==

At its core is a word-size storage device (or ***[[Register]]*** ) called the ***[[Program counter]]*** (PC). At any point in time, the PC points at (contains the address of) some ***[[machine-language instruction]]*** in main memory.

From the time that power is applied to the system until the time that the power is shut off, a **processor repeatedly executes the instruction pointed at by the program counter and updates the program counter to point to the next instruction.**

==A processor appears to operate according to a very simple instruction execution model, defined by its ***instruction set architecture***==.

---

In general all of this works all together just for [[Running a simple Hello World Program]] or any other program in the computer.