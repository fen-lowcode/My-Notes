#computer-system #programming

```c
#include <stdio.h>

int main() {
    printf("Hello, World!");
    return 0;
}
```

Given this simple view of a [[Hardware Organization of a Typical System]] and operation, we can begin to understand **what happens when we run our [[Hello World Program]]**. We must omit a lot of details here that will be filled in later, but for now we will be content with **the big picture**.

Initially, the **shell program** is executing its instructions, waiting for us to type a command. As we type the characters `./hello` at the **keyboard**, the shell program reads each one into a **register** and then stores it in **memory**, as shown in Figure 1.5. When we hit the **enter key** on the keyboard, the shell knows that we have finished typing the command.

![](Figure1.5.png)

The shell then loads the executable **hello file** by executing a sequence of instructions that copies the **code and data** in the hello object file from **disk** to **main memory**. The data includes the string of characters `"hello, world\n"` that will eventually be printed out. Using a technique known as **[[Direct Memory Access]] (DMA)**, the data travel **directly from disk to main memory, without passing through the processor**. This step is shown in Figure 1.6.

Once the code and data in the hello object file are loaded into memory, the **processor** begins executing the **machine-language instructions** in the hello program’s main routine. These instructions copy the bytes in the `"hello, world\n"` string from memory to the **register file**, and from there to the **display device**, where they are displayed on the screen. This step is shown in Figure 1.7.

![](Figure1.6.png)
![](Figure1.7.png)

---

**And it's apparent that in this simple example** ==a system spends a lot of time moving information one place to another.== To fix ==it computer designer implemented something that they call [[Cache Memory]].== 