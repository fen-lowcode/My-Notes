#theory #computer-system 


The architecture of the `Von-Neumann` was developed by the Hungarian mathematician John von Neumann, and it consists of four functional units:

- `Memory`
- `Control Unit`
- `Arithmetical Logical Unit`
- `Input/Output Unit`



![[Von-Neumann-Architecture-7-2048.webp]]
![[von_neumann3.webp]]


In this type of architecture, the most important units are the Arithmetical Logical ([[Arithmetic Logic Unit|ALU]]) Unit and the Control Unit ([[Contrl Unit|CU]]) which is combined all together in an actual Central Processing Unit ([[Central Processing Unit (CPU) |CPU]]).


The CPU is responsible for executing the instructions one by one and for flow control and the commands and data is fetched from the memory by the CU.

The connection between the processor, memory and input/output devices is called a [[Motherboard#MOTHERBOARD BUSSES|Bus System]] which is not mentioned in the original Von-Neumann architecture but plays an essential role in practice.

In this architecture, all instructions and data flow and transferred via the Bus System.

---
## ==Memory==

The memory can be divided into two different categories:

- `Primary Memory`
- `Secondary Memory`

#### The Primary Memory 

The primary memory is the [[Cache Memory]] and [[Main Memory|Random Access Memory]]. Logically thinking, memory is nothing more than a place to store information. 

It's like leaving something in one of our friend's place to later pick it up but with this we have to know the address of our friend to pick up what we left behind. It is the same thing as RAM.

RAM describes a memory type whose memory allocations can be accessed directly and randomly by their `memory addresses`.

And `cache` is integrated into the processor and serves as a buffer, accessing `cache` rather than the `RAM` is faster so it's the best case to always supply the `CPU` with data.

Before a program instruction and data is supplied into the processor, it is first stored in the RAM to serve as data storage.

The larger the `RAM` the more `data` it can store, however when the `ram` loses power, everything in it are lost.


##### The Secondary Memory

The secondary memory is a mass external data storage, such as HDD/SSD, flash drives, CD etc of a computer which is not accessed by the `CPU`, but via the I/O Interfaces.

the use of it is to permanently store data  that is not needed to be processed by the CPU for the mean time, and even without power the secondary device can still preserve the data that it holds but often much slower than `RAM`

---

## ==Control Unit==

The `Control Unit` (`CU`) is responsible for the correct inter working of the processor's individual parts. An internal bus connection is used for the tasks of the `CU`. The tasks of the `CU` can be summarized as follows:

- Reading data from the RAM
- Saving data in RAM
- Provide, decode and execute an instruction
- Processing the inputs from peripheral devices
- Processing of outputs to peripheral devices
- Interrupt control
- Monitoring of the entire system

The `CU` contains the Instruction Register (IR), which contains all instructions that the processor decodes and executes accordingly.  The instruction decoder translates the instructions and passes them to the execution unit, which then executes the instruction. 

The execution unit transfers the data to the `ALU` for calculation and receives the result back from there. The data used during execution is temporarily stored in `registers`.

To understand the difference between the `ALU` and `CU`, the picture below is a Stack exchange post that explains just that. 

[Source here](https://electronics.stackexchange.com/questions/731506/how-does-the-control-unit-cu-actually-work)

![[Screenshot_20250928_111835.png]]

---
## Central Processing Unit

The `Central Processing Unit` (`CPU`) is the functional unit in a computer that provides the actual processing power. It is responsible for processing information and controlling the processing operations. To do this, the `CPU` fetches commands from memory one after the other and initiates data processing.

The processor is also often referred to as a `Microprocessor` when placed in a single electronic circuit, as in our PCs.

Each `CPU` has an architecture on which it was built. The best-known `CPU architectures` are:

- `x86`/`i386` - (AMD & Intel)
- `x86-64`/`amd64` - (Microsoft & Sun)
- `ARM` - (Acorn)

Each of these CPU architectures is built in a specific way, called `Instruction Set Architecture` (`ISA`), which the CPU uses to execute its processes. `ISA`, therefore, describes the behavior of a CPU concerning the instruction set used. The instruction sets are defined so that they are independent of a specific implementation. Above all, ISA gives us the possibility to understand the unified behavior of `machine code` in `assembly language` concerning `registers`, `data types`, etc.

There are four different types of `ISA`:

- `CISC` - `Complex Instruction Set Computing`
- `RISC` - `Reduced Instruction Set Computing`
- `VLIW` - `Very Long Instruction Word`
- `EPIC` - `Explicitly Parallel Instruction Computing`

---

#### Instruction Cycle

The instruction set describes the totality of the machine instructions of a processor. The scope of the instruction set varies considerably depending on the processor type. Each CPU may have different instruction cycles and instruction sets, but they are all similar in structure, which we can summarize as follows:

|**Instruction**|**Description**|
|---|---|
|`1. FETCH`|The next machine instruction address is read from the `Instruction Address Register` (`IAR`). It is then loaded from the `Cache` or `RAM` into the `Instruction Register` (`IR`).|
|`2. DECODE`|The instruction decoder converts the instructions and starts the necessary circuits to execute the instruction.|
|`3. FETCH OPERANDS`|If further data have to be loaded for execution, these are loaded from the cache or `RAM` into the working registers.|
|`4. EXECUTE`|The instruction is executed. This can be, for example, operations in the `ALU`, a jump in the program, the writing back of results into the working registers, or the control of peripheral devices. Depending on the result of some instructions, the status register is set, which can be evaluated by subsequent instructions.|
|`5. UPDATE INSTRUCTION POINTER`|If no jump instruction has been executed in the EXECUTE phase, the `IAR` is now increased by the length of the instruction so that it points to the next machine instruction.|
