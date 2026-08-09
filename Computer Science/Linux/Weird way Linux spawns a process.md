# Complete Guide: fork and execv - Process Creation in Linux

## Table of Contents

1. Overview of Process Creation
2. The fork() Function - Detailed Explanation
3. The execv() Family of Functions
4. How fork() and execv() Work Together
5. Complete Function Signatures and Parameters
6. Practical Examples
7. Error Handling and Best Practices

---

## 1. Overview of Process Creation in Linux

In Linux, creating a new process is a two-step operation:

1. **fork()** - Duplicates the current process
2. **execv()** (or one of its variants) - Replaces the process image with a new program

This is fundamentally different from operating systems like Windows, where process creation is a single atomic operation.

### Why Two Steps?

- **fork()** creates a copy of the current process with all its resources
- **execv()** replaces the process memory with a new program while preserving the process ID and other attributes
- This design allows for shell functionality, I/O redirection, and process control

---

## 2. The fork() Function - Detailed Explanation

### What fork() Does

`fork()` creates a new process by duplicating the calling process. The new process is called the **child process**, and the original is the **parent process**.

### Key Characteristics of fork()

#### **Process Duplication**

- The child process receives a copy of the parent's:
    - Memory space (entire heap and stack)
    - File descriptors (open files)
    - Environment variables
    - Signal handlers
    - Process flags
    - Resource limits (ulimit values)

#### **What is NOT Copied**

- Process ID (PID) - child gets a new, unique PID
- Parent Process ID (PPID) - child's PPID is the parent's PID
- Process execution time
- File locks
- Pending signals (queued signals are not inherited)
- Memory locks
- Child process is not a "real" copy of memory (uses Copy-on-Write - COW)

### Copy-on-Write (COW) Mechanism

When fork() is called, the kernel doesn't immediately duplicate the entire memory:

- Parent and child initially share the same pages
- When either process writes to a page, the kernel creates a separate copy
- This is more efficient than copying the entire memory space

### How fork() Returns (Critical!)

`fork()` has a unique behavior: it returns **twice**

- **In parent process**: Returns the **child's process ID (PID)** (positive integer)
- **In child process**: Returns **0** (zero)
- **On error**: Returns **-1** (in parent process only), child is not created

This dual return is the key to differentiating parent and child behavior in the same code!

### Visual Representation

```
Before fork():
┌─────────────────────┐
│   Parent Process    │
│   PID: 1000         │
│   fork() called     │
└─────────────────────┘
          │
          ├─ fork() returns 1001 (child PID)
          │
After fork():
┌─────────────────────┐         ┌─────────────────────┐
│   Parent Process    │         │   Child Process     │
│   PID: 1000         │         │   PID: 1001         │
│   fork() returns    │         │   fork() returns    │
│   1001 (child PID)  │         │   0                 │
└─────────────────────┘         └─────────────────────┘
```

---

## 3. The execv() Function and Its Family

### What execv() Does

`execv()` replaces the entire memory image of the calling process with a new program. Critically:

- **Does NOT create a new process**
- **Does NOT fork** - uses the same process ID
- **Overwrites everything**: code, data, heap, stack
- Point of no return - execution continues in the new program

### The exec() Family

There are multiple variants of exec functions with different capabilities:

|Function|Arguments|Environment|Path Search|
|---|---|---|---|
|`execv()`|Array|Inherit parent's|Full path required|
|`execvp()`|Array|Inherit parent's|Searches PATH|
|`execve()`|Array|Custom array|Full path required|
|`execvpe()`|Array|Custom array|Searches PATH|
|`execl()`|Variadic|Inherit parent's|Full path required|
|`execlp()`|Variadic|Inherit parent's|Searches PATH|
|`execle()`|Variadic|Custom array|Full path required|
|`execlpe()`|Variadic|Custom array|Searches PATH|

**Naming Convention:**

- **v** = arguments as array
- **l** = arguments as list (variadic)
- **e** = environment as third parameter
- **p** = search in PATH environment variable

### Key Characteristics of execv()

#### **What is Preserved**

- Process ID (PID)
- Parent Process ID (PPID)
- Process group and session
- Signal handlers and dispositions
- Resource limits
- Current working directory
- Umask
- File descriptors (unless marked with FD_CLOEXEC)
- Real and effective user/group IDs

#### **What is Replaced**

- Program text (code segment)
- Initialized and uninitialized data
- Heap
- Stack
- Command-line arguments
- Environment variables (unless execv() - then inherited)
- Signal handlers (reset to default)

### Return Behavior

- **On success**: exec() does NOT return. The new program begins execution at main()
- **On error**: Returns -1 and sets errno. Parent process continues normally
- **No cleanup needed**: Kernel handles resource management

---

## 4. How fork() and execv() Work Together

### The Typical Pattern: fork() + execv()

```
1. Parent process calls fork()
         │
         ├─→ Child process created
         │
2. Parent checks fork() return value
   ├─→ If return value > 0: Continue as parent
   │
3. Child checks fork() return value
   ├─→ If return value == 0: Call execv()
   │
4. execv() replaces child's memory with new program
   │
5. Parent and Child run independently
```

### Complete Lifecycle Example

```
Process A (Parent)
├─ fork() called
│  ├─ fork() returns Child's PID (let's say 2000)
│  │
│  ├─ Parent continues with original program
│  │  (Can wait for child, do other tasks, or exit)
│  │
│  └─ Eventually calls wait() or waitpid()
│     to collect child's exit status
│
Process B (Child) - Created by fork()
├─ fork() returns 0
│ 
├─ Child calls execv("/bin/ls", ...)
│  (Memory completely replaced)
│
└─ New program (ls) runs
   ├─ Performs its function
   └─ Exits, returns status to parent
```

---

## 5. Complete Function Signatures and Parameters

### 5.1 fork() Function

```c
#include <unistd.h>

pid_t fork(void);
```

#### **Parameters**

- **None** - fork() takes no parameters

#### **Return Value**

```
Type: pid_t (process identifier type, typically int or long)

Returns:
  - Positive integer (child's PID) in parent process
  - 0 in child process
  - -1 on error (and errno is set), parent process continues
```

#### **Error Conditions** (when returns -1)

```c
EAGAIN   - The system lacked the necessary resources to create another process
ENOMEM   - Insufficient memory to fork
ENOTSUP  - fork() is not supported by this platform (rare)
```

#### **Common Errno Values**

- **EAGAIN**: Too many processes already running, or user has exceeded process limit
- **ENOMEM**: System out of memory or process memory limit exceeded

### 5.2 execv() Function

```c
#include <unistd.h>

int execv(const char *path, char *const argv[]);
```

#### **Parameters Detailed**

**1. `const char *path`**

- **Type**: Pointer to null-terminated string
- **Purpose**: Complete path to the executable file
- **Requirements**: Must be executable by the calling process
- **Must be absolute path** (e.g., "/bin/ls", "/usr/bin/python3")
- **Relative paths**: Not recommended - use execvp() if you need PATH search
- **Example**: "/bin/ls", "/usr/bin/env", "/home/user/myprogram"

**2. `char *const argv[]`**

- **Type**: Array of pointers to null-terminated strings
- **Purpose**: Command-line arguments to pass to the new program
- **Requirements**:
    - **argv[0]**: By convention, should be the program name (or command name)
    - **argv[1]...argv[n]**: Actual arguments to the program
    - **argv[n+1]**: **MUST be NULL** (null pointer) to terminate the array
- **Important**: Array must end with NULL, not just any terminator
- **Example**:
    
    ```c
    char *argv[] = {    "/bin/ls",        // argv[0]: program name    "-l",             // argv[1]: first argument    "-a",             // argv[2]: second argument    "/home/user",     // argv[3]: third argument    NULL              // MUST end with NULL pointer};
    ```
    

#### **Return Value**

```
Type: int

Returns:
  - Does NOT return on success (new program takes over)
  - -1 on error (parent process continues executing)
  - errno is set on error

Note: Successful execv() never returns to the calling code
```

#### **Error Conditions** (when returns -1)

```c
EACCES   - No execute permission on the path or file doesn't exist
ENOENT   - Path or executable file not found
ENOEXEC  - File exists but is not a regular file or not executable/not in executable format
ENOMEM   - Insufficient memory to execute the program
ENOTSUP  - Executable is in unsupported format
ETXTBSY  - File is open for writing by another process
ELOOP    - Too many symbolic links in path
ENAMETOOLONG - Path or argument list too long
EIO      - I/O error while reading the file
EISDIR   - Path points to a directory instead of executable file
ELIBBAD  - Dynamic linker found an unsupported ELF format
EINVAL   - argv array is invalid (typically: argc == 0)
```

### 5.3 execvp() Function (Searches PATH)

```c
#include <unistd.h>

int execvp(const char *file, char *const argv[]);
```

#### **Parameters**

**1. `const char *file`**

- **Type**: Pointer to null-terminated string
- **Purpose**: Name of the executable or partial path
- **Different from execv()**: Does NOT require absolute path
- **If PATH exists in file**: Uses that (e.g., "./myprogram", "/usr/local/bin/prog")
- **If no / in file**: Searches directories listed in PATH environment variable
- **Example**: "ls", "python3", "gcc" (will search PATH)

**2. `char *const argv[]`**

- Identical to execv() function
- **argv[0]**: Program name
- **argv[1]...argv[n]**: Arguments
- **argv[n+1]**: Must be NULL

#### **Return Value**

- Same as execv(): -1 on error, doesn't return on success

#### **Additional Error**

```c
EACCES   - File found in PATH but not executable
ENOENT   - File not found in any PATH directory
```

### 5.4 execve() Function (Custom Environment)

```c
#include <unistd.h>

int execve(const char *path, char *const argv[], char *const envp[]);
```

#### **Parameters**

**1. `const char *path`**

- Same as execv()
- Must be absolute or full path

**2. `char *const argv[]`**

- Same as execv()
- Array terminated with NULL

**3. `char *const envp[]`**

- **Type**: Array of pointers to null-terminated strings
- **Purpose**: Environment variables for the new program
- **Format**: "NAME=value" strings
- **Termination**: Must end with NULL pointer
- **Replaces**: Parent's environment completely
- **Example**:
    
    ```c
    char *envp[] = {    "PATH=/bin:/usr/bin",    "HOME=/home/user",    "LANG=en_US.UTF-8",    NULL  // MUST end with NULL};
    ```
    

#### **Return Value**

- Same as execv(): -1 on error, doesn't return on success

### 5.5 execvpe() Function (PATH Search + Custom Environment)

```c
#include <unistd.h>

int execvpe(const char *file, char *const argv[], char *const envp[]);
```

#### **Parameters**

- **file**: Like execvp() - searches PATH if no "/" present
- **argv**: Like execv() - terminated with NULL
- **envp**: Like execve() - custom environment, terminated with NULL

#### **Return Value**

- Same as others: -1 on error, doesn't return on success

---

## 6. Practical Examples

### Example 1: Simple fork() and execv()

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    pid_t pid = fork();
    
    if (pid == -1) {
        // Error: fork failed
        perror("fork");
        exit(1);
    }
    else if (pid == 0) {
        // Child process
        char *argv[] = {"/bin/ls", "-la", "/home", NULL};
        
        if (execv("/bin/ls", argv) == -1) {
            perror("execv");
            exit(1);
        }
        // execv() doesn't return on success, so this is unreachable
    }
    else {
        // Parent process
        printf("Parent: Forked child with PID %d\n", pid);
        
        int status;
        pid_t child = wait(&status);
        
        if (WIFEXITED(status)) {
            printf("Parent: Child exited with status %d\n", 
                   WEXITSTATUS(status));
        }
    }
    
    return 0;
}
```

### Example 2: Using execvp() to Search PATH

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child: run "python3" without full path
        char *argv[] = {"python3", "-c", "print('Hello from Python')", NULL};
        
        execvp("python3", argv);  // Searches PATH
        
        perror("execvp");
        exit(1);
    }
    else if (pid > 0) {
        // Parent: wait for child
        wait(NULL);
        printf("Child completed\n");
    }
    else {
        perror("fork");
        exit(1);
    }
    
    return 0;
}
```

### Example 3: Using execve() with Custom Environment

```c
#include <unistd.h>
#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child: Set custom environment
        char *argv[] = {"/usr/bin/env", NULL};
        
        char *envp[] = {
            "PATH=/usr/bin:/bin",
            "HOME=/tmp",
            "USER=testuser",
            "LANG=en_US.UTF-8",
            "CUSTOM_VAR=my_value",
            NULL
        };
        
        execve("/usr/bin/env", argv, envp);
        
        perror("execve");
        exit(1);
    }
    else if (pid > 0) {
        wait(NULL);
    }
    else {
        perror("fork");
        exit(1);
    }
    
    return 0;
}
```

### Example 4: Complete Shell-like Implementation

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char command[256];
    
    while (1) {
        printf("shell> ");
        fflush(stdout);
        
        if (fgets(command, sizeof(command), stdin) == NULL)
            break;
        
        // Remove newline
        command[strcspn(command, "\n")] = 0;
        
        // Fork child process
        pid_t pid = fork();
        
        if (pid == -1) {
            perror("fork");
            continue;
        }
        
        if (pid == 0) {
            // Child: Execute command
            char *argv[] = {"/bin/sh", "-c", command, NULL};
            execvp("/bin/sh", argv);
            
            perror("execvp");
            exit(127);
        }
        else {
            // Parent: Wait for child
            int status;
            waitpid(pid, &status, 0);
            
            if (WIFEXITED(status)) {
                printf("[Process exited with status %d]\n", 
                       WEXITSTATUS(status));
            }
        }
    }
    
    return 0;
}
```

---

## 7. Error Handling and Best Practices

### Best Practice 1: Always Check fork() Return Value

```c
pid_t pid = fork();

if (pid == -1) {
    perror("fork failed");
    exit(1);
}
else if (pid == 0) {
    // Child process
} 
else {
    // Parent process
}
```

### Best Practice 2: Use errno and perror()

```c
#include <errno.h>
#include <string.h>

if (execv(path, argv) == -1) {
    fprintf(stderr, "execv failed: %s\n", strerror(errno));
    exit(1);
}
```

### Best Practice 3: Always NULL-Terminate argv

```c
// CORRECT
char *argv[] = {"/bin/ls", "-l", NULL};

// WRONG - Missing NULL terminator
char *argv[] = {"/bin/ls", "-l"};  // Undefined behavior!
```

### Best Practice 4: Never Assume execv() Returns

```c
// execv() only returns if it fails
execv("/bin/ls", argv);

// This code will only execute if execv() fails
perror("execv");
exit(1);

// If you need cleanup code, do it BEFORE execv()
```

### Best Practice 5: Resource Cleanup Before execv()

```c
// Close unnecessary file descriptors
for (int i = 3; i < 1024; i++) {
    close(i);  // Close all except stdin(0), stdout(1), stderr(2)
}

// Now call execv()
execv("/bin/ls", argv);
```

### Best Practice 6: Handle File Descriptor Inheritance

```c
// Set close-on-exec flag for sensitive file descriptors
int flags = fcntl(fd, F_GETFD);
fcntl(fd, F_SETFD, flags | FD_CLOEXEC);

// Now when execv() is called, this fd will be automatically closed
execv(path, argv);
```

### Best Practice 7: Use waitpid() for Better Control

```c
#include <sys/wait.h>

pid_t pid = fork();

if (pid == 0) {
    execv(path, argv);
    perror("execv");
    exit(127);
}
else {
    int status;
    pid_t result = waitpid(pid, &status, 0);
    
    if (result == -1) {
        perror("waitpid");
    }
    else if (WIFEXITED(status)) {
        printf("Exit status: %d\n", WEXITSTATUS(status));
    }
    else if (WIFSIGNALED(status)) {
        printf("Killed by signal %d\n", WTERMSIG(status));
    }
}
```

---

## Summary Table: fork() vs execv()

|Aspect|fork()|execv()|
|---|---|---|
|**Creates process**|Yes|No|
|**Returns**|Twice (once per process)|Once (only on error)|
|**Preserves PID**|No (child gets new PID)|Yes|
|**Preserves memory**|Yes (copy)|No (replaces)|
|**Copies file descriptors**|Yes|Yes (unless FD_CLOEXEC)|
|**Copies environment**|Yes (or custom with execve)|Yes (or custom)|
|**Typical use**|Create new process|Load new program|
|**Error return**|-1 (in parent)|-1 (original process continues)|
|**Success return**|PID in parent, 0 in child|Doesn't return|

---

## Complete C Headers and Includes

```c
#include <unistd.h>      // fork(), execv(), execvp(), execve(), execvpe()
#include <sys/types.h>   // pid_t type definition
#include <sys/wait.h>    // wait(), waitpid(), WIFEXITED(), WEXITSTATUS()
#include <stdio.h>       // printf(), perror(), fprintf()
#include <stdlib.h>      // exit(), malloc(), free()
#include <string.h>      // strlen(), strcpy(), strerror()
#include <errno.h>       // errno variable
#include <fcntl.h>       // fcntl(), FD_CLOEXEC (for file descriptor control)
```

---

## Key Takeaways

1. **fork() duplicates** the entire process, creating a parent-child relationship
2. **execv() replaces** the process memory with a new program
3. **fork() returns twice**, differentiating parent and child via return value
4. **execv() doesn't return on success**, only on failure
5. **argv must be NULL-terminated**, this is critical
6. **execv() requires absolute paths**, use execvp() for PATH search
7. **fork() + execv()** is the POSIX way to spawn new processes
8. **Always check return values** for both functions
9. **File descriptors persist** through execv() unless marked FD_CLOEXEC
10. **Use wait()/waitpid()** in parent to collect child's exit status