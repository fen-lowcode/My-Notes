#computer-system 
## The Virtual Memory

when the program is called, the sections are mapped into chunks or segments in the process, and the segments are loaded into memory described by the elf file.

> **Buffer**

![[buffer_overflow_1.webp]]

* .**text**, this section contains the actual assembly instructions of the program. This region can be read-only to prevent the process from accidentally modifying it's own instruction. Any attempt to write to this area will inevitably result an error, Segmentation fault.

* ***.data** is the ==section that contains any global and static variables that are explicitly initialized by the program.==

* ***.bss**, some compilers and linkers use the .bss section as part of the data segment, ==this region Contains global and static variables that are uninitialized or initialized to zero.==

* ***Heap*** The heap is allocated from this area. ==This area starts at the end of the .bss segment and grows to the higher memory addresses.== Dynamic memory allocated during program execution using functions like `malloc()`.

* ***Stack*** is used for local variables and function call management. ==Stack memory is a Last-In-First-Out Data structure== in which the return address, parameters, and depending on the compiler option, frame pointers are stored. C/C++ local variables are stored here and you can copy code to the stack.


Visual Example: 

![[b0ee7723-3682-4c00-9283-8cbbe09879ee_2460x2478.webp]]