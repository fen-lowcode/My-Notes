#computer-system 

### What is a clock cycle?

**A clock cycle is the smallest unit of time in a processor, defined by its clock.**

- The **processor clock** is like a metronome that ticks at a fixed frequency (e.g., 3 GHz = 3 billion ticks per second).

- Each tick is a **clock cycle**.

- All operations inside the CPU (fetching instructions, moving data, doing calculations) are synchronized to these cycles.

- Example:

	- A 1 GHz clock → 1 billion cycles per second.
	- If one instruction takes 4 cycles, that instruction takes 4 nanoseconds.

### Simple analogy

**Think of a clock cycle as a "beat" in music. The CPU works in rhythm with these beats.**

- Old CPUs needed several beats to finish even a simple step.

- Modern CPUs use multiple instruments (pipelines) to play several notes at once, so more work gets done in the same time.