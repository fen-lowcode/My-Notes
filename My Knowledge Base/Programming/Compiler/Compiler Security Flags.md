
What Each Flag Does

- **`-fstack-protector-strong`**: Enables the smart stack canary. It protects any function using arrays, buffers, local variable references, or standard structs.

- **`-O2`**: Turns on standard code optimization. This is required because buffer checks and fortify macros rely on compiler optimization to work correctly. 

- **`-D_FORTIFY_SOURCE=3`**: Automatically replaces dangerous string and memory functions (like `memcpy`, `strcpy`, `sprintf`) with safer, size-checked variants.

- **`-fPIE -pie`**: Compiles the app as a Position Independent Executable. This enables the operating system's **ASLR** to completely randomize the program's location in memory.

- **`-Wl,-z,relro,-z,now`**: Enables "Full RELRO" (Read-Only Relocations). It locks down the program's internal binary tables (like the Global Offset Table) immediately at startup so attackers cannot overwrite them.

#### Additional Quality & Safety Flags

To catch potential memory and logic bugs before they ever make it to production

```
-Wall -Wextra -Wconversion -Wformat -Wformat-security
```

Use code with caution.

- **`-Wformat -Wformat-security`**: Warns you if you write code vulnerable to format string exploits (another major memory vulnerability). 

