#hardware #computer-system #programming

A **register** is a small, fast storage location inside a [[Processor]] or **[[Microcontroller]]**  
It holds data temporarily for processing. Unlike RAM, registers are directly connected to the processor’s logic, making them much faster.

Registers are measured in **bits** (commonly 8, 16, 32, or 64).  
The size of the register often defines the **word size** of the CPU (e.g., a 64-bit CPU has 64-bit registers). 

**`Reference:`** [[32 bit and 64 bit - what in the world is a bit?]]

#### Hardware View of Registers

##### Purpose

- Store operands before execution.
- Hold intermediate results during execution.
- Store memory addresses for instructions or data.
- Control CPU operations (status, flags, instruction decoding).

##### Characteristics

- **Volatile**: Data is lost when power is off.
- **Ultra-fast access**: Faster than cache or RAM.
- **Fixed size**: Limited in number (a CPU may have only dozens of registers).

----

#### **Types of Hardware Registers**

 **General Purpose Registers (GPRs)** 
 
Used for arithmetic, logic, and temporary data.
Examples: 
	`AX`, `BX`, `CX`, `DX` in x86; `R0–R31` in ARM.

**Special Purpose Registers**

Control or support CPU functions.

Examples:
- [[Program counter]] (PC): Holds address of the next instruction.
- [[Stack Pointer]] (SP): Points to top of stack memory.
- [[Status Register / Flags Register (SR/FR)]]: Holds condition flags    

- **[[Instruction Register]] (IR)**

    - Holds the current instruction being decoded and executed.

- **[[Memory Address Register]] (MAR)**

    - Holds the memory address of data or instruction to be fetched or stored.

- **[[Memory Data Register]] (MDR)**

    - Holds the actual data fetched from or written to memory.

---

#### Software View of Registers

From a programmer’s perspective, registers appear as **variables inside the CPU**.  
They are accessed through **assembly instructions** or compiler optimizations.

### Roles in Software

- **Performance optimization**: ==Compilers use registers to store frequently used variables.==

- **Parameter passing**: ==In many calling conventions, function arguments are passed via registers instead of the stack.==

- **System-level programming**: ==OS kernels and device drivers manipulate registers for hardware control.==

- **Flags & status checks**==: Software uses condition flags (in the status register) after arithmetic operations.==


#### Example in Assembly

``` Assembly
MOV AX, 5      ; Load value 5 into AX register 
ADD AX, 3      ; 
Add 3 to AX (AX = 8) MOV BX, AX     ; Copy result into BX
```
`
Here, `AX` and `BX` are registers directly manipulated by instructions.

#### Registers vs Other Memory

|Feature|Registers|Cache|RAM|Storage (SSD/HDD)|
|---|---|---|---|---|
|Speed|Fastest|Fast|Medium|Slow|
|Size|Few bytes|KB–MB|GBs|TBs|
|Location|Inside CPU|On CPU die|On motherboard|External device|
|Purpose|Immediate use|Frequently used data|Main program data|Long-term storage|


---

#### Practical Use in Programming

- ==**C language with inline assembly** lets you manipulate registers directly.==
- **Compilers** map variables into registers automatically for efficiency.
- **Low-level programming** (OS, embedded systems) requires explicit register usage for:

    - Configuring peripherals (UART, GPIO, timers).
    - Writing device drivers.
    - Handling interrupts.

---
#### ==Registers in Processors and Context Switching==

**Registers as Physical Hardware**

- ==A **register** is a physical storage element inside the CPU core.==
- ==Each **core** has its own set of registers.==
- ==The registers belong to the CPU, not directly to any process.==
- ==They store data only while the CPU is executing instructions.==


**Registers and Processes

- ==On a **single-core processor**, only **one process** runs at any given moment.==
- ==Each process uses the same physical registers, but never at the same time.==
- ==The **operating system scheduler** decides which process runs.==


==**Context Switching**==

==When the OS switches from one process to another:== 


1. The OS **saves the current state** of the running process.

    - This includes the values of all registers (general purpose, program counter, stack pointer, flags).
    - These values are stored in a **Process Control Block (PCB)** in memory.

2. The OS **loads the saved state** of the next process.

    - The registers are restored with that process’s data.

3. The CPU continues execution as if the new process had been running all along.


This mechanism is called a **context switch**. [[Concurrency and Parallelism | Click Here for more]]


==**Registers in Multi-Core CPUs**==

- Each **core** has its own set of registers and can execute its own thread.
- If a CPU has 4 cores, it has 4 independent register sets.
- Multiple processes can run in parallel, one on each core.
- Even in multi-core systems, the OS still performs context switching when there are more processes than cores.


**Key Idea**

- ==**One core = one physical set of registers.**==
- ==**One process at a time per core.**==
- ==The OS saves and restores register states so multiple processes appear to run “at once.”==

---
#### Summary

- Registers are the **fastest memory in a computer**, built into the CPU.
- They exist both as **hardware storage elements** and **software-accessible resources**.
- Critical for executing instructions, storing addresses, and controlling CPU state.
- Limited in size and number but essential for all computation.