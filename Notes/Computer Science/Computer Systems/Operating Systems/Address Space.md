#operaing-systems 


The range of virtual addresses that the operating system assigns to a user or separately running program is called an **address space**. This is the area of contiguous virtual addresses available for executing instructions and storing data.

The range of virtual addresses in an address space starts at zero and can extend to the highest address permitted by the operating system architecture.

z/OS® provides each user with a unique address space and maintains the distinction between the programs and data belonging to each address space. Within each address space, the user can start multiple tasks, using task control blocks or TCBs that allow multiprogramming.

In some ways a z/OS address space is like a UNIX® process, and the address space identifier (ASID) is like a process ID (PID). Further, TCBs are like UNIX threads in that each operating system supports processing multiple instances of work concurrently.

However, the use of multiple virtual address spaces in z/OS holds some special advantages. Virtual addressing permits an addressing range that is greater than the central storage capabilities of the system. The use of multiple virtual address spaces provides this virtual addressing capability to each job in the system by assigning each job its own separate virtual address space. The potentially large number of address spaces provides the system with a large virtual addressing capacity.

With multiple virtual address spaces, errors are confined to one address space, except for errors in commonly addressable storage, thus improving system reliability and making error recovery easier. Programs in separate address spaces are protected from each other. Isolating data in its own address space also protects the data.

z/OS uses many address spaces. There is at least one address space for each job in progress and one address space for each user logged on through TSO, telnet, rlogin or FTP (users logged on z/OS through a major subsystem, such as CICS® or IMS™, are using an address space belonging to the subsystem, not their own address spaces). There are many address spaces for operating system functions, such as operator communication, automation, networking, security, and so on.

The use of address spaces allows z/OS to maintain the distinction between the programs and data belonging to each address space. The private areas in one user's address space are isolated from the private areas in other address spaces, and this **address space isolation** provides much of the operating system's security.

Yet, each address space also contains a common area that is accessible to every other address space. Because it maps all of the available addresses, an address space includes system code and data as well as user code and data. Thus, not all of the mapped addresses are available for user code and data.

The ability of many users to share the same resources implies the need to protect users from one another and to protect the operating system itself. Along with such methods as "keys" for protecting central storage and code words for protecting data files and programs, separate address spaces ensure that users' programs and data do not overlap.

In a multiple virtual address space environment, applications need ways to communicate between address spaces. z/OS provides two methods of **inter-address space communication**:

- Scheduling a service request block (SRB), an asynchronous process
- Using cross-memory services and access registers, a synchronous process

A program uses an SRB to initiate a process in another address space or in the same address space. The SRB is asynchronous in nature and runs independently of the program that issues it, thereby improving the availability of resources in a multiprocessing environment.

A program uses cross-memory services to access another user's address spaces directly. You might compare z/OS cross-memory services to the UNIX Shared Memory functions, which can be used on UNIX without special authority. Unlike UNIX, however, z/OS cross-memory services require the issuing program to have special authority, controlled by the authorized program facility (APF). This method allows efficient and secure access to data owned by others, data owned by the user but stored in another address space for convenience, and for rapid and secure communication with services like transaction managers and database managers.