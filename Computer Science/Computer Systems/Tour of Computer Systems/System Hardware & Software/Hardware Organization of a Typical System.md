#computer-system #hardware
![](Figure1.4.png)

---

>  **Buses**

==Running throughout the system is a collection of electrical conduits called== **Buses** that carries bytes of information back and forth between the components.

**Buses** are typically designed to transfer fixed-sized chunks known as ***words***. The number of bytes in a word (the word size) is a fundamental system parameter that varies across systems. Most machines today have word size of either 4 bytes (32 bits) or 8 bytes(64 bits).

---

>  **[[IO Devices]]**


==Input/Output (I/O) devices are the systems connection to the external world.== Our example system has four I/O devices: **Keyboard** and **Mouse** for user input, a **Display** for user output, and a **Disk Drive** (or simply Disk) for long-term storage of data and programs. Initially, the executable file resides on the disk.

==Each I/O device is connected to the I/O bus by either a controller or an adapter.== The distinction between the two is mainly one of packaging.

==Controllers are chip sets in the device itself== or on the system’s main printed circuit board (often called the ***[[Motherboard]]***). 

==An adapter is a card that plugs into a slot on the motherboard==. Regardless, the purpose of each is to transfer information back and forth between the I/O bus and an I/O device.

---

> **[[Main Memory]]**

The **main memory** is a ==temporary storage device that holds both a program and the data it manipulates while the processor is executing the program==. 

Physically, ==main memory consists of a collection of dynamic random access memory== 
( [[DRAM]] ) chips. 

==Logically, **memory is organized as a linear array of bytes==**, each with its own unique address 
( [[array index]] )  starting at zero. ==In general, each of the machine instructions that constitute a program can consist of a variable number of bytes.== 

==The sizes of data items that correspond to C program variables vary according to type==. For example, on an ***[[x86-64]]*** machine running Linux, data of type short require 2 bytes, types int and float 4 bytes, and types long and double 8 bytes.

---

> **[[Processor]]**

==The **central processing unit** (***CPU*** ), or simply processor, is the engine that interprets (or executes) instructions stored in main memory==

At its core is a word-size storage device (or ***[[Register]]*** ) called the ***[[program counter]]*** (PC). At any point in time, the PC points at (contains the address of) some ***[[machine-language instruction]]*** in main memory.

From the time that power is applied to the system until the time that the power is shut off, a **processor repeatedly executes the instruction pointed at by the program counter and updates the program counter to point to the next instruction.**

==A processor appears to operate according to a very simple instruction execution model, defined by its ***instruction set architecture***==.

---

In general all of this works all together just for [[Running a simple Hello World Program]] or any other program in the computer.