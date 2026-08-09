#windows #operaing-systems

#### The .NET Framework

The .NET Framework is part of Windows. .NET Framework is a software development framework for building and running applications on Windows.

The .NET Framework consists of two major components:

![[Pasted image 20260120021331.png]]

**The Common Language Runtime** (CLR) 
	
* This is the run-time engine for .NET and includes a Just In Time (JIT) compiler that translates Common Intermediate Language (CIL) instructions to the underlying hardware CPU machine language, a garbage collector, type verification, code access security, and more. It’s implemented as a COM in-process server (DLL) and uses various facilities provided by the Windows API.
	
**The .NET Framework Class Library** (FCL) 
	
* This is a large collection of types that implement functionality typically needed by client and server applications, such as user interface services, networking, database access, and much more.


[Click here to read more about .NET Framework](https://learn.microsoft.com/en-us/dotnet/framework/get-started/?WT.mc_id=dotnet-35129-website#Introducing)

---


#### Services, functions, and routines

Several terms in the Windows user and programming documentation have different meanings in different contexts. For example, the word service can refer to a callable routine in the OS, a device driver, or a server process. 

The following list describes what certain terms mean in this book:

**Windows API Functions** 
	These are documented, callable subroutines in the Windows API. Examples include CreateProcess, CreateFile, and GetMessage. 

**Native System Services** (or system calls)
	These are the undocumented, underlying services in the OS that are callable from user mode. For example, NtCreateUserProcess is the internal system service the Windows CreateProcess function calls to create a new process.

**Kernel Support Functions** (or routines) 
	These are the subroutines inside the Windows OS that can be called only from kernel mode (defined later in this chapter). For example, ExAllocatePoolWithTag is the routine that device drivers call to allocate memory from the Windows system heaps (called pools).

**Windows Services** 
	These are processes started by the Windows service control manager. For example, the Task Scheduler service runs in a user-mode process that supports the schtasks command (which is similar to the UNIX commands at and cron). (Note that although the registry defines Windows device drivers as “services,” they are not referred to as such in this book.)

**Dynamic Link Libraries** (DLLs) 
	These are callable subroutines linked together as a binary file that can be dynamically loaded by applications that use the subroutines. Examples include Msvcrt.dll (the C run-time library) and Kernel32.dll (one of the Windows API subsystem libraries). Windows user-mode components and applications use DLLs extensively. The advantage DLLs provide over static libraries is that applications can share DLLs, and Windows ensures that there is only one in-memory copy of a DLL’s code among the applications that are referencing it. Note that library .NET assemblies are compiled as DLLs but without any unmanaged exported subroutines. Instead, the CLR parses compiled metadata to access the corresponding types and members.

---
#### Processes

Although programs and processes appear similar on the surface, they are fundamentally different. A
program is a static sequence of instructions, whereas a process is a container for a set of resources used
when executing the instance of the program.

At the highest level of abstraction, a Windows process comprises the following:

**A Private Virtual Address Space**
	This is a set of virtual address space that the process can use.

**An Executable Program**
	This defines initial code and data and is mapped into the process’s virtual address space.

**A List of open handles** 
	These map to various system resources such as semaphores, synchronization objects, and files that are accessible to all threads in the process.

**A Security Context** 
	This is an access token that identifies the user, security groups, privileges, attributes, claims, capabilities, User Account Control (UAC) virtualization state, session, and limited user account state associated with the process, as well as the AppContainer identifier and its related sandboxing information.

**A process ID** 
	This is a unique identifier, which is internally part of an identifier called a client ID

**At least one thread of execution** 
	Although an “empty” process is possible, it is (mostly) not useful.