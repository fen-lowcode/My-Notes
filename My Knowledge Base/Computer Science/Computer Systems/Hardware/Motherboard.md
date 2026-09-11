#computer-system  #hardware

---

<sub>references: https://www.siyavula.com/read/za/information-technology/grade-11/hardware/01-hardware
</sub>


> **Goal**

* Describe the motherboard
* Describe the purpose and role of the motherboard
* Describe the purpose and role of the components of the motherboard
* Describe the purpose and role of expansion cards
* Explain how data is transferred between computer components

---

> **Hardware In A Nutshell**

Computer hardware refers to the physical components that make up a computer. This includes devices that stand on their own like monitors and keyboards, but also components inside the computer like the **CPU** (also known as the **[[Central Processing Unit (CPU)]]**) and motherboard. 

**Hardware can be grouped into the following categories:**

- **Input devices:** any device that allows you to add data to a computer, like a keyboard, mouse, camera or microphone.

- **[[ Main Memory]]**. the storage space in a computer where data is temporarily kept while it is being processed. 

- **Output devices:** a device that takes data from a computer and makes it available to the user in a way the user understands. Examples include monitors, speakers and printers.

- **Storage devices:** a device that stores data permanently. Different storage devices like hard disk drives, CDs and flash drives simply store different amounts of data.

- **Processing devices:** the computer devices responsible for implementing instructions and doing calculations. You learned about two processing devices: **CPU** and **[[GPU]]**.

	
	**CPU** – the processing unit responsible for processing general instructions
	
	**GPU** – the part of a computer responsible for processing the instructions that create a picture on the screen
	

- **Communications devices:** devices, such as modems and routers, that allow a computer to connect to other computers on a **[[Computer Network]]**.

---

All these devices are connected inside a computer by the motherboard, a **printed circuit board** (or **PCB**).

---

### Motherboard

Imagine you are a computer. As a computer, your body parts act as input devices. **They take signals from your environment, translate them into electrical signals and send them to your brain.** 

For example, if it is too cold or warm. **Your brain is like the processing device (or CPU). It receives millions of signals from your environment, interprets them, makes decisions and finally sends commands to your body.** 

**These commands are received by output devices that convert the decisions from your brain into something visible that people can see.** For example, you will put a long-sleeved top on if it’s cold.

In a similar way, a **computer’s motherboard is responsible for connecting all the hardware devices and sending signals from one device to the next.**

---

### PURPOSE AND ROLE OF THE MOTHERBOARD

**The motherboard is a large printed circuit board with specific slots for every hardware component**, which have been designed so that only the correct components will fit. In principle, **the motherboard performs four key tasks**. It:

- ==Provides physical structure for other hardware.==
- ==Connects the hardware.==
- ==Provides power to the hardware.==
- ==Sends signals between the hardware.==

---

#### COMPONENTS AS PART OF THE MOTHERBOARD

The following image shows where most of the important devices are connected to the motherboard.

<sub> Figure: 1.1 </sub>

![](https://www.siyavula.com/read/za/information-technology/grade-11/hardware/images/pg003-01.jpg)

**key takeaways**: ==_A motherboard is a large PCB with slots for all the hardware_==

---

**The motherboard connects all the hardware**, if you look closely in the image below, you will notice that **there are lines all over the motherboard**. 

These are the **circuits that transfer data between different components**.

<sub> Figure: 1.2 </sub>

![](Figure1.2.jpeg)


**Fact**: ==Small zig-zags are added to routes to make sure the signals arrive at the same time==


**Looking at Figure 1.2** you may also notice that the **circuits are not always the same width**. This is **because the width of a circuit determines how much data it can carry**, with **wider circuits being able to transport more data**. 

For example, a component like the **GPU requires very wide circuits because it is responsible for receiving and sending millions of instructions** each second, **while a 3,5 mm sound port might require a lower width circuit.**

---

**Processing devices which do the hard work of manipulating the data and performing the calculations needed by your computer.** 

**These devices receive the data from the RAM**, **perform sets of instructions, and returns the processed data to the RAM.** 

**The two most important processing devices** in modern computers are **Central Processing Unit** and **Graphics Processing Unit.** 

 The most important components that can be connected to the motherboard are shown in the table below.

| COMPONENT                      | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[[BIOS Chip]]**              | A computer’s BIOS (or Basic Input/Output System), is the first set of instructions that run every time a computer is started up. The BIOS is stored on your computer’s ROM (or read-only memory) and is responsible for making sure your computer starts up correctly. However, the BIOS application can also be accessed by users to specify the hardware settings to the motherboard.  |
| **[[Central Processing Unit (CPU)]]**              | The Central Processing Unit (CPU) is the most important component of the computer. It is the brain of the computer which runs all programs and processes all software instructions. Central Processing Unit (CPU) is responsible for processing general instructions. Every application makes use of the CPU to collect, decode and execute instructions as required by the application. |
| **[[GPU]]**                    | Graphics Processing Unit (GPU) is responsible for processing the instructions that create the pictures on your screen, for example, three-dimensional games rely heavily on the GPU to create their images.                                                                                                                                                                              |
| **[[Main Memory]]**            | The RAM (or random-access memory) is the short-term memory of the computer which can store and retrieve active programs and data at very high speeds. Your computer can access and utilise the data in the RAM immediately without delay. All data is erased from your RAM when your computer is turned off.                                                                             |
| **[[VRAM]]**                   | Video Random Access Memory is the memory used to store image data that the computer displays. It acts as a buffer between the CPU and the video card. When a picture is to be displayed on the screen, the image is first read by the processor and then written to the VRAM.                                                                                                            |
| **[[ROM]]** (Read-only memory) | The ROM (or read-only memory) stores the motherboard’s operating software, called the BIOS.                                                                                                                                                                                                                                                                                              |

---
#### MOTHERBOARD SLOTS

To connect these components (as well as other, expansion components) to the motherboard, **motherboards contain several slots**. These slots include:

- **CPU slot:** ==The CPU socket (also called the ZIF socket) is used to connect the CPU to the motherboard. Intel and AMD each have their own sockets, and each brand occasionally change their standard socket when a new generation of CPUs is released.==

- **DIMM slot:** ==The DIMM slot is used to connect the computer’s RAM to the motherboard==.

- **PCI/PCIe:** ==The Peripheral Component Interconnect (PCI) or PCI express slots allow you to plug additional hardware like a GPU, sound card, ethernet card or Wi-Fi card into the computer.==

- **GPU/PCIe x16:** ==this is a high-speed slot generally used for the computer’s graphics card since it receives and sends the most data.==

- **SATA:** ==The Serial AT Attachment (SATA) port is used to connect internal storage devices such as hard drives to the computer.==

- **Power:** ==the main power connection connects the motherboard to the power supply which connects to a normal household power outlet. This supplies power to the motherboard and all connected components.==

<sub> Figure 1.3 </sub>

![](https://www.siyavula.com/read/za/information-technology/grade-11/hardware/images/pg005-02.jpg)


---

#### MOTHERBOARD BUSSES

The path connecting different components on a motherboard is called a **bus**. **Buses are not just the electrical circuit, but the whole communication system between two devices, including the hardware components (such as the port and the circuit), and the electrical conductors, the communication format and the software**. 

**A single bus can allow multiple simultaneous connections between components**. For example, a single USB (universal serial bus) can receive and send information to multiple USB devices at the same time.

**Key Takeaways:** ==**bus** – a communication system transferring data between components inside a computer, or between computers==

In other sections of the motherboard, **data may be transferred using point-to-point connections**. These **connections are made directly between two components on the motherboard and do not allow additional connections to be made**. **These point-to-point connections are typically built into the motherboard and CPU to allow them to communicate more easily.**

The **motherboard bus is a set of wires that connects one part of the motherboard to communicate with other parts of the motherboard. It also serves as an interface between the CPU and various external devices.** 

> **==The motherboard bus can be one of two types:==**

- **An Internal Bus** - is the communication highway of the motherboard, sending data and instructions to the different parts within the motherboard. ==It links the different parts of the computer to the CPU and the main memory.==

- **The External Type of Motherboard Bus (Expansion Bus)** - i==s the interface for peripheral devices like hard disks, CD-ROM drives, and flash drives to get connected to the CPU.== **The shape of each interface is unique preventing one from plugging a device to a wrong port, which could cause damage to the device while being connected to the CPU.**


> ==**All Buses Have:**==

- **A Control Bus**, ==which is used by the CPU to send signals to the different parts of the computer system to keep the actions of the different parts coordinated.==

- **A Data Bus**, ==which provides the path to transfer data and instructions among the different components of the computer.== It is assisted by the address bus, which provides the physical address of data in the system memory to help data transfers.

- **A Power Bus**, ==which energies the different components of the computer system.==

---

### **MODULAR DESIGN**

==Computers are built using a modular design.== This means that, rather than being made from a single component, ==it is built from several different components (or modules).== 

Because components are not built into the motherboard (or soldered onto the motherboard), ==you can remove or replace these components with new components at any time.== 

This ==allows you to create thousands of different computers with different components, different speeds and different abilities.==


**<sub>Table 1.1: Comparison between two computers</sub>**

| COMPONENTS           | **BASIC COMPUTER**        | BETTER COMPUTER                          |
| :------------------- | :------------------------ | :--------------------------------------- |
| **Motherboard**      | The same Motherboard      | The same motherboard                     |
| **Case**             | The same case or chassis  | The same case or chassis                 |
| **CPU**              | Core i3-8100( 4 x 3,6GHz) | Core i7-8700k ( 12 x 4,7GHz )            |
| **GPU**              | Built in GPU              | 2 x GeForce RTX 2080 Ti                  |
| **RAM**              | 1 x 4 GB ( 2000 MHz )     | 2 x 8GB ( 3000MHz )                      |
| **Storage**          | 500 GB Hard Drive         | 1T Solid State Drive                     |
| **Other Item**       | N/A                       | Liquid Cooling System 900 W power supply |
| **Approximate Cost** | R9 500                    | R82 000                                  |

---

#### PURPOSE AND ROLE OF EXPANSION CARDS

As you saw in the list of motherboard slots, the PCI and PCI-express slots allow you to add (or expand) the features of your computer through the use of expansion cards. These are circuit boards for computer components that are purchased separately and use the PCI or PCI-express slots to connect to your computer.

**<sub>Figure 1.4: An example of an expansion card</sub>**

![](https://www.siyavula.com/read/za/information-technology/grade-11/hardware/images/pg007-02.jpg)


**Popular examples of expansion cards include:**

- **Ethernet card** ==that allows you to connect to a wired network.==
- **Wi-Fi card** ==that allows you to connect to a wireless network.==
- **Sound card** ==for improved sound quality or surround sound capabilities.==
- **TV Capture card** ==that allows you to capture videos and store them on your computer.==
- **Storage controller** ==that allows you to add additional or faster hard drives.==

---

#### FLOW/TRANSFER OF DATA BETWEEN COMPONENTS

==After reading the information on the components of the motherboard we will see that the basic routes the motherboard uses to transfer these signals are shown below:==


![](https://www.siyavula.com/read/za/information-technology/grade-11/hardware/images/pg008-01.jpg)