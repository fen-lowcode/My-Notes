> [!Question]
As a learner with intermediate knowledge, I thought that when the preprocessor inserts `#include <stdio.h>` into the source code, the actual source code of the `printf` function is also included. Then, during compilation, the compiler processes the `printf` code along with the rest of the source and includes it directly in the resulting object file. Is this how it works?

---
> [!Answer]  
> This is a common and understandable assumption, but it’s not exactly how the compilation process works.  
>  
> When you include a header file such as `#include <stdio.h>`, the preprocessor copies only the **declarations** (function prototypes and macros) from the header into your source code. In the case of `stdio.h`, it provides a declaration for `printf`, informing the compiler about its return type, parameters, and existence — but **not** its actual implementation.  
>  
> During the **compilation phase**, your code is converted into an object file (e.g., `hello.o`). This object file contains machine code for the functions you defined, as well as unresolved references (symbols) to functions like `printf`. These references are placeholders; the compiler expects the actual implementation to be provided later.  
>  
> The actual implementation of `printf` resides in the **standard C library**, typically in a precompiled form (such as `libc.a` or `libc.so`). During the **linking phase**, the linker (`ld`) resolves the `printf` symbol by locating its definition in the standard library. It then merges your object file with the necessary parts of the library to produce a complete executable that includes everything needed to run your program.  
>  
> **In summary**, the source code of `printf` is **not** included during preprocessing or compilation. Instead, the linker resolves its implementation from the standard library during the final stage of program generation.
