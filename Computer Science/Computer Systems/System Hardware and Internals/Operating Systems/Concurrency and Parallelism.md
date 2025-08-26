#computer-system #programming 

>[!important]
>Concurrency and Parallelism are important and powerful concept that have been existing since the early history of computer evolution.

`Reference:`  [[What is concurrency?]],  [[Clock Cycle]],  [[What is GCC and G++]]

==Throughout the history of digital computers, two demands have been constant forces in driving improvements: we want them to do more, and we want them to run faster. Both of these factors improve when the processor does more things at once.== 

==We use the term concurrency to refer to the general concept of a system with multiple, simultaneous activities, and the term parallelism to refer to the use of concurrency to make a system run faster.== 

**Parallelism can be exploited at multiple levels of abstraction in a computer system.** 

==We highlight three levels here, working from the highest to the lowest level in the system hierarchy.==

---
#### **Thread-Level Concurrency**

==Building on the process abstraction, we are able to devise systems where multiple programs execute at the same time, leading to concurrency.==

>[!important] 
>**With threads, we can even have multiple control flows executing within a single process.** 

Support for concurrent execution has been found in computer systems since the advent of time-sharing in the early 1960. 

Traditionally, this concurrent execution was only simulated, by having a single computer rapidly switch among its executing processes, much as a juggler keeps multiple balls flying through the air. 

This form of concurrency allows multiple users to interact with a system at the same time, such as when many people want to get pages from a single Web server. 

It also allows a single user to engage in multiple tasks concurrently, such as having a Web browser in one window, a word processor in another, and streaming music playing at the same time. 

==Until recently, most actual computing was done by a single processor, even if that processor had to switch among multiple tasks. This configuration is known as a== **uniprocessor system.** 

==When we construct a system consisting of multiple processors all under the control of a single operating system kernel, we have a==  **multiprocessor system.** 

Such systems have been available for large-scale computing since the 1980, but they have more recently become commonplace with the advent of **[[multi-core processors]]** and **[[hyperthreading.]]**

**`Figure 1.16` shows a taxonomy of these different processor types.**

![](different-processor-configuration.png)

> **Multi-core processors**

==Multi-core processors have several CPU (referred to as “cores”) integrated onto a single integrated-circuit chip.==

`Figure 1.17` illustrates the organization of a typical multi-core processor,  

==where the chip has four CPU cores, each with its own L1 and L2 caches,== 
==and with each L1 cache split into two parts—one to hold recently fetched instructions and one to hold data.==


![](multi-core-proccessor.png)


**The cores share higher levels of cache as well as the interface to main memory.** Industry experts predict that they will be able to have dozens, and ultimately hundreds, of cores on a single chip.

>[!Important]
>**Hyperthreading**, sometimes called simultaneous multi-threading, is a technique that allows a single CPU to execute multiple flows of control. 
>

It involves having multiple copies of some of the CPU hardware, such as **[[Program counter]]** and [[Register File]], while having only single copies of other parts of the hardware, such as the units that perform floating-point arithmetic.

Whereas a conventional processor requires around 20,000 clock cycles to shift between different threads.

**A hyperthreaded processor decides which of its threads to execute on a cycle-by-cycle basis. It enables the CPU to take better advantage of its processing resources.**

For example, if one thread must wait for some data to be loaded into a cache, the CPU can proceed with the execution of a different thread. 

As an example, the Intel Core i7 processor can have each core executing two threads, and so a four-core system can actually execute eight threads in parallel.

The use of multiprocessing can improve system performance in two ways. 

* First, it reduces the need to simulate concurrency when performing multiple tasks.
   As mentioned, even a personal computer being used by a single person is expected
  to perform many activities concurrently. 

* Second, it can run a single application program faster, but only if that program is expressed in terms of multiple threads that can effectively execute in parallel. 

Thus, although the principles of concurrency have been formulated and studied for over 50 years, the advent of multi-core and hyperthreaded systems has greatly increased the desire to find ways to write application programs that can exploit the thread-level parallelism available with the hardware. 

---
#### **Instruction-Level Parallelism**

==At a much lower level of abstraction, modern processors can execute multiple instructions at one time, a property known as instruction-level parallelism.== 

For example, early microprocessors, such as the 1978-vintage Intel 8086, required
multiple (typically 3–10) clock cycles to execute a single instruction. ==More recent==
==processors can sustain execution rates of 2–4 instructions per clock cycle.== 

Any given instruction requires much longer from start to finish, perhaps 20 cycles or
more, but the processor uses a number of clever tricks to process as many as 100
instructions at a time. 

Jump into the concept of  [[pipelining]] , where the actions required to execute an instruction are partitioned into different steps and the processor hardware is organized as a series of stages, each performing one of these steps. 

The stages can operate in parallel, working on different parts of different instructions. We will see that a fairly simple hardware design can sustain an execution rate close to 1 instruction per clock cycle.

**Processors that can sustain execution rates faster than 1 instruction per cycle are known as super-scalar processors.** 

Most modern processors support super-scalar operation. In Chapter 5, we will describe a high-level model of such processors.

==**We will see that application programmers can use this model to understand the performance of their programs. They can then write programs such that the generated code achieves higher degrees of instruction-level parallelism and therefore runs faster.**==


>[! Digestible Summary]


- **Instruction-level parallelism (ILP):** Modern processors don’t always execute instructions one after another. They work on many instructions at the same time.

- **Old processors (e.g., Intel 8086, 1978):** Needed 3–10 clock cycles just to finish one instruction.

- **Modern processors:** Can process 2–4 instructions in a single clock cycle. 

- **Trick:** Even though one instruction may take 20 cycles from start to finish, the processor overlaps work (like an assembly line) so that hundreds of instructions are in progress at once.

- **Pipelining:** Splitting instruction execution into stages (fetch, decode, execute, memory access, write back). Each stage works in parallel on different instructions.

- **Super-scalar processors:** Processors that can execute multiple instructions per cycle (more than 1) using multiple pipelines.

- **For programmers:** If you understand this, you can write code that takes advantage of ILP, making programs run faster.


---

#### **Single-Instruction, Multiple-Data (SIMD) Parallelism**

At the lowest level, many ==modern processors have special hardware that allows a single instruction to cause multiple operations to be performed in parallel, a mode known as single-instruction, multiple-data (SIMD) parallelism.==

==For example, recent generations of Intel and AMD processors have instructions that can add 8 pairs of single-precision floating-point numbers (C data type float) in parallel.== 

These ==SIMD== instructions are provided mostly to speed up applications that process image, sound, and video data. **==Although some compilers attempt to automatically extract SIMD parallelism from C programs, a more reliable method is to write programs using special vector data types supported in compilers such as GCC.==** 

`We describe this style of programming in Web Aside opt:simd, as a supplement to the more general presentation on program optimization `

>[!Notes from chatGPT]
>##### Prompt
> **Is doing opt:simd optionally needs to be manually implemented if you really need i**t?
> 
> ##### Answer
>
> **Yes, SIMD (Single Instruction, Multiple Data) optimizations often need manual effort if you want to fully benefit from them. Here’s the breakdown:**
>
> #### 1. Automatic SIMD (compiler auto-vectorization)
>
>- Modern compilers (like GCC, Clang, MSVC) try to detect loops and operations that can be vectorized.
>- Example: A simple loop adding two arrays is often auto-vectorized without you doing anything.
>- But compilers are conservative. If the code is complex, has branching, or memory alignment issues,  they often **skip vectorization**.
>
> #### 2. Manual SIMD
>
> - You can use **intrinsics** (functions that map directly to SIMD instructions, like Intel AVX or ARM NEON).
> - Or you use **libraries** (like OpenMP, BLAS, or Eigen) that are SIMD-optimized.
> - Manual SIMD gives higher performance, but it makes code less portable and harder to maintain.
> #### 3. When do you need manual SIMD?
> 
> - If your workload is very performance-sensitive (e.g., image processing, cryptography, signal processing, AI inference).
> - If profiling shows the compiler didn’t auto-vectorize critical loops.    
> - If you want to squeeze the maximum speed from modern CPUs.
>
>---
>
> **Summary:**
>
> - For normal applications, rely on compiler auto-vectorization.
> - For performance-critical code, manual SIMD pays off.
