#CSAPP 


##### 1.4.2 Running the hello Program

```c
#include <stdio.h>

int main() {
    printf("Hello, World!");
    return 0;
}
```

Given this simple view of a [[Hardware Organization of a Typical System| system's hardware orgnization]] and operation, we can begin to understand **what happens when we run our [[A Tour of Computer Systems - Chapter 1 CSAPP#*figure 1.1* | Hello World Program]]**. We must omit a lot of details here that will be filled in later, but for now we will be content with **the big picture**.

Initially, the **shell program** is executing its instructions, waiting for us to type a command. As we type the characters `./hello` at the **keyboard**, the shell program **reads each one into a register and then stores it in memory,** as shown in [[Running a simple Hello World Program#*figure1.5* | figure 1.5]]. When we hit the **enter key** on the keyboard, the shell knows that we have finished typing the command.

###### *figure1.5*

![](Figure1.5.png)

The shell then loads the executable **hello file** by executing a sequence of instructions that copies the **code and data** in the **hello object file** from **disk** to **main memory**. The data includes the string of characters `"hello, world\n"` that will eventually be printed out. Using a technique known as **[[Direct Memory Access]] (DMA)** ==`will be discussed in chapter 6`== , the data travel **directly from disk to main memory, without passing through the processor**. This step is shown in [[Cache Memory#*figure1.6* | figure 1.6]].

Once the code and data in the hello object file are loaded into memory, the **processor** begins executing the **machine-language instructions** in the hello program’s main routine. These instructions copy the bytes in the `"hello, world\n"` string from memory to the **register file**, and from there to the **display device**, where they are displayed on the screen. This step is shown in [[Cache Memory#*figure1.7*| figure 1.7]].
