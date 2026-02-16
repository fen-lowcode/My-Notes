
==It's important to know [[What is GCC and G++]] is entirely and why it pays to understand [[How GCC works under the hood]] (also G++).==

There's couple of multiple reason why it's important to understand how the compilation system work, especially when developing such complicated and big programs. 

>[!Optimization]
>**Optimizing program performance**. Modern compilers are sophisticated tools
> that usually produce good code. As programmers, we do not need to know
> the inner workings of the compiler in order to write efficient code. However,
> in order to make good coding decisions in our C programs, we do need a
> basic understanding of machine-level code and how the compiler translates
> different C statements into machine code. For example, is a switch statement
> always more efficient than a sequence of if-else statements? How much
> overhead is incurred by a function call? Is a while loop more efficient than
> a for loop? Are pointer references more efficient than array indexes? Why
> does our loop run so much faster if we sum into a local variable instead of an
> argument that is passed by reference? How can a function run faster when we
> simply rearrange the parentheses in an arithmetic expression?

>[!Link-time Errors]
>**Understanding link-time errors**. In our experience, some of the most perplexing 
> programming errors are related to the operation of the linker, especially
> when you are trying to build large software systems. For example, what does
> it mean when the linker reports that it cannot resolve a reference? What is the
> difference between a static variable and a global variable? What happens if
> you define two global variables in different C files with the same name? What
> is the difference between a static library and a dynamic library? Why does it
> matter what order we list libraries on the command line? And scariest of all,
> why do some linker-related errors not appear until run time?

>[!Avoiding Security Holes]
> For many years, buffer overflow vulnerabilities have
> accounted for many of the security holes in network and Internet servers.
> These vulnerabilities exist because too few programmers understand the need
> to carefully restrict the quantity and forms of data they accept from untrusted
> sources. A first step in learning secure programming is to understand the con-
> sequences of the way data and control information are stored on the program
> stack. 
