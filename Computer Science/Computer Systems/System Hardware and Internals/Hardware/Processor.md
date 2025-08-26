#computer-system #hardware

---

>[!Processor] 
>In this model, instructions execute in strict sequence and executing a single instruction involves performing a series of steps. 

---

**The processor reads the instruction from [[Main Memory]] pointed at by the [[Program counter]] (PC), interprets the bits in the instruction, performs some simple operation dictated by the instruction, and then updates the PC to point to the next instruction**, which may or may not be contiguous in memory to the instruction that was just executed.

There are only few of these simple operations, and they revolve around the ***[[Main Memory]]***, ***[[Register File]]***, and the ***[[Arithmetic/Logic Unit]]*** **( ALU )**.

**The Register File is a small storage device that consists of a collection of word-size 
[[Register]]**(s), each with its own unique name. 

---

>[!Alu] 
>The ALU computes new data and address values. Here are some examples of the simple operations that the CPU might carry out at the request of an instruction:

* **Load:** Copy a byte or a word from main memory into a register, overwriting the previous contents of the register.

* **Store:** Copy a byte or a word from a register to a location in main memory, overwriting the previous contents of that location.

* **Operate:** Copy the contents of two registers to the ALU, perform an arithmetic operation on the two words, and store the result in a register, overwriting the previous contents of that register.

* **Jump:** Extract a word from the instruction itself and copy that word into the program counter (PC), overwriting the previous value of the PC
---

We say that a processor appears to be a simple implementation of its instruction set architecture that is capable of [[Running a simple Hello World Program]], but in fact modern processors use far more complex mechanisms to speed up program execution. 

Thus, we can distinguish the processor’s instruction set architecture, describing the effect of each machine-code instruction, from its microarchitecture, describing how the processor is actually implemented.
