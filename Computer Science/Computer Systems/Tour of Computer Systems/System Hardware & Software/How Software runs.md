#computer-system 

---

Let's say the [[Hello World Program]] has been translated by the compilation system  ***( refer to [[Why it's important to understand the compilation system]] )*** into a **[[executable object file]]**, this executable file is stored on disk.

Now to execute the executable file on a **[[Unix System]]**, we type it's name to an application program know as a **[[Shell]]**.

### 🐧 Running a Program on Linux

``` terminal
$ ./hello
Hello, World

```

---



==The [[Shell]] is a command-line interpreter that prints a prompt, waits for the user to type a command line, and then performs the commands.== 

If the first word of the command-line does not correspond to a built-in shell command, then the shell assumes that it is the name of a executable file that it should load and run.

So in that case, the shell loads and runs the program and then wait for it to terminate.

To dive deeper into what happens to a software when it runs, we need to understand the **[[Hardware Organization of a Typical System]]**, This particular picture below  is modeled after the family of recent Intel systems, but all systems have a similar look and feel. Don’t worry about the complexity of this figure just now. We will get to its various details in stages throughout the course of the book.

![](Figure1.4.png)

