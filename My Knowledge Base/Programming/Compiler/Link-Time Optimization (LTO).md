

tags: [compilers, gcc, clang, optimization, linking, build-systems] 
aliases: [LTO, -flto, IPO, interprocedural optimization]

# Link-Time Optimization (LTO)

> [!summary] LTO defers the final code-generation phase of compilation until **link time**, when the compiler can see _every_ translation unit (TU) at once. This lets it perform whole-program (interprocedural) optimizations, most importantly **cross-file inlining**, that are impossible when each `.c` file is compiled to machine code in isolation.

---

## 1. The Problem LTO Solves

### 1.1 The classic compilation model

C/C++ uses **separate compilation**: each `.c`/`.cpp` file is a _translation unit_ compiled independently into a `.o` file. The compiler only sees one TU at a time.

```
main.c  ──► [compiler] ──► main.o  ─┐
utils.c ──► [compiler] ──► utils.o ─┼──► [linker] ──► program
math.c  ──► [compiler] ──► math.o  ─┘
```

When `main.c` calls `add()` defined in `utils.c`, the compiler sees only a **declaration** (`int add(int, int);`). It has no idea what `add` does, so it must:

- emit a real `call add` instruction,
- follow the calling convention (arguments in registers/stack, save/restore registers),
- assume `add` could do _anything_ (modify globals, have side effects, not return).

The linker later patches the call address, but the linker only works with **machine code and relocations**. It has no understanding of program structure, so it cannot inline.

### 1.2 What is lost at the file boundary

|Optimization|Why it fails across files|
|---|---|
|Inlining|Callee body not visible|
|Constant propagation|Compiler can't see that a function always receives/returns a constant|
|Dead-code elimination|Compiler can't know a non-`static` function is never called from anywhere|
|Alias / side-effect analysis|Must assume an external call may touch any memory|
|Devirtualization (C++)|Can't prove the dynamic type of an object across TUs|
|Identical code folding|Duplicate functions in different TUs can't be merged by the compiler|

### 1.3 Workarounds LTO makes unnecessary

- Putting small functions in headers as `static inline`.
- **Unity / jumbo builds** (`#include "a.c"` into one giant TU).
- Manually reorganizing code purely for performance.

LTO gives most of the benefit of a unity build while keeping normal source organization.

---

## 2. Benefits in Detail

### 2.1 Cross-module inlining

The callee body is substituted at the call site. This removes:

- argument setup (register moves / stack pushes),
- the `call` / `ret` pair and the associated branch prediction pressure,
- callee-saved register spills.

### 2.2 Secondary optimizations unlocked by inlining

Inlining is a "gateway" optimization. After inlining, the optimizer can see through the boundary and apply:

- **Constant propagation / folding**: `add(2, 3)` → `5`.
- **Dead-code elimination**: branches that depend on now-known constants vanish.
- **Common subexpression elimination** and **loop optimizations** across what used to be a call.
- **Better register allocation** since the combined body is analyzed as one unit.

### 2.3 Whole-program interprocedural analyses (IPA)

Even _without_ inlining, GCC's IPA passes use the global view:

- **IPA constant propagation (IPA-CP)**: if every caller passes the same constant, specialize/clone the function.
- **IPA scalar replacement of aggregates (IPA-SRA)**: pass struct fields in registers instead of by pointer.
- **Pure/const discovery**: prove a function has no side effects so calls can be removed or reordered.
- **Visibility privatization**: a non-`static` function with no outside users is effectively made `static`, allowing non-standard calling conventions and aggressive removal.
- **Identical Code Folding**: merge byte-identical functions.
- **Devirtualization** (C++): resolve virtual calls once the class hierarchy is known to be closed.

### 2.4 Code size and cache behavior

> [!note] Nuance The original claim "better i-cache efficiency" is _situational_. LTO can improve locality (hot callers and callees placed close together, dead code removed), but heavy inlining can also **grow** code and hurt the instruction cache. Always measure.

---

## 3. How GCC Implements It

### 3.1 Pipeline without LTO

```
source ─► parse ─► GENERIC ─► GIMPLE ─► (optimize) ─► RTL ─► assembly ─► .o (machine code)
                                                                      │
                                                          linker only relocates/glues
```

- **GIMPLE** is GCC's main mid-level IR (a simplified three-address form).
- **RTL** is the low-level, machine-oriented IR used for instruction selection and register allocation.
- By the time a `.o` file exists, the high-level structure is gone.

### 3.2 Pipeline with `-flto`

```
Compile step (per TU):
source ─► parse ─► GIMPLE ─► (early optimizations) ─► stream GIMPLE into .o  [STOP: no machine code needed]

Link step (whole program):
all .o files ─► linker plugin ─► read all GIMPLE ─► merge into one program graph
             ─► IPA (inlining, cloning, ...) ─► RTL ─► assembly ─► final machine code ─► executable
```

Key details:

1. **Compile phase**: GCC runs the frontend and _early_ optimizations, then **serializes GIMPLE** (not "bytecode" in the JVM sense, it is a streamed form of the IR) into special ELF sections in the `.o` file, named `.gnu.lto_*`.
2. **Link phase**: GCC is invoked again (via the linker) to read those sections, perform whole-program optimization, and generate real machine code.
3. **The linker plugin** (`liblto_plugin.so`) is how the linker cooperates with GCC:
    - the linker loads the plugin when it sees LTO objects,
    - the plugin reports which input files contain LTO IR,
    - the linker tells the plugin **symbol resolution results** (which definition "wins", which symbols are referenced from outside the LTO set, e.g. from a shared library or non-LTO object),
    - this resolution info is what allows safe dead-code elimination and visibility privatization.

### 3.3 Slim vs. fat LTO objects

|Mode|Flag|Contents of `.o`|Notes|
|---|---|---|---|
|**Slim** (default in modern GCC)|`-fno-fat-lto-objects`|GIMPLE only|Smaller, faster compile. Object **cannot** be linked without LTO support (needs the plugin).|
|**Fat**|`-ffat-lto-objects`|GIMPLE **and** regular machine code|Larger, slower compile. Works as a normal object if LTO isn't used at link time. Useful for distributing static libraries.|

### 3.4 Parallel LTO: WHOPR

Optimizing an entire large program in one process would be slow and memory-hungry. GCC uses the **WHOPR** ("whole program optimizer") model:

1. **WPA (Whole Program Analysis)**: single-threaded; reads _summaries_ of all functions, decides inlining/cloning, and **partitions** the program.
2. **LTRANS (Local Transformation)**: each partition is optimized and compiled to machine code, in **parallel**.
3. Results are linked into the final binary.

Controls:

- `-flto=N` : use N parallel LTRANS jobs.
- `-flto=auto` : pick job count automatically (GCC 10+); also integrates with the make jobserver (`-flto=jobserver`).
- `-flto-partition=<balanced|one|none|max|1to1>` : how to split the program. `none` disables partitioning (best quality, no parallelism, most memory).

### 3.5 Clang/LLVM equivalent

||GCC|Clang|
|---|---|---|
|Full LTO|`-flto`|`-flto` (or `-flto=full`)|
|Scalable LTO|WHOPR (`-flto=N`)|**ThinLTO**: `-flto=thin`|
|IR stored in `.o`|GIMPLE|LLVM bitcode|
|Linker plugin|`liblto_plugin.so`|`LLVMgold.so` (gold) or built-in support in lld|

> [!tip] ThinLTO ThinLTO builds a compact global _index_ of all modules, makes cross-module import decisions from it, then optimizes each module **in parallel and incrementally**. It gets most of full-LTO's gains with far lower memory and build time, and supports incremental caching (`-Wl,--thinlto-cache-dir=...`). Prefer it for large projects.

---

## 4. Usage

### 4.1 The golden rule

> [!important] Pass `-flto` **and your optimization level** (`-O2`/`-O3`) at **both** compile and link time. The heavy optimization happens in the _link_ step, so the link command must carry the `-O` flags and any `-march`/`-f...` codegen flags too. If you omit them at link time, the final code generation can silently use defaults.

### 4.2 Command line

```bash
# 1. Compile each TU into an LTO object (IR stored, minimal machine code)
gcc -O3 -flto -c main.c  -o main.o
gcc -O3 -flto -c utils.c -o utils.o

# 2. Link: this is where whole-program optimization actually runs
gcc -O3 -flto main.o utils.o -o my_program

# Parallel link-time optimization using all available cores
gcc -O3 -flto=auto main.o utils.o -o my_program
```

### 4.3 Makefile

```makefile
CC      = gcc
CFLAGS  = -O3 -flto=auto
LDFLAGS = -O3 -flto=auto

my_program: main.o utils.o
	$(CC) $(LDFLAGS) main.o utils.o -o my_program

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@
```

### 4.4 CMake

Modern CMake calls LTO **Interprocedural Optimization (IPO)**. Check support first rather than assuming:

```cmake
cmake_minimum_required(VERSION 3.9)
project(MyProject C)

include(CheckIPOSupported)
check_ipo_supported(RESULT ipo_ok OUTPUT ipo_msg)

add_executable(my_program main.c utils.c)

if(ipo_ok)
  # Per-target (preferred)
  set_property(TARGET my_program PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
  # Or globally:
  # set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
else()
  message(WARNING "IPO/LTO not supported: ${ipo_msg}")
endif()
```

Notes:

- Policy `CMP0069` (CMake 3.9+) makes CMake honor IPO for GCC/Clang.
- CMake typically applies IPO to the **Release** configuration cleanly; verify what flags appear with `cmake --build . --verbose`.

### 4.5 Static libraries (important gotcha)

Plain `ar`, `nm`, `ranlib` do **not** know about the LTO plugin. Use the wrappers:

```bash
gcc-ar  rcs libfoo.a a.o b.o     # instead of ar
gcc-ranlib libfoo.a              # instead of ranlib
gcc-nm  libfoo.a                 # instead of nm
```

Otherwise the archive symbol index may be empty/wrong and linking fails with undefined references. (Clang equivalents: `llvm-ar`, `llvm-ranlib`, `llvm-nm`.)

### 4.6 Controlling LTO selectively

```bash
gcc -O2 -flto -fno-lto -c legacy.c      # exclude one TU from LTO
```

Per-function: `__attribute__((noinline))`, `__attribute__((optimize("no-lto")))` (limited), or `__attribute__((used))` to keep a symbol alive.

---

## 5. Inspecting What LTO Did

```bash
# See the LTO IR sections inside an object
objdump -h main.o | grep lto
readelf -S main.o | grep gnu.lto

# Keep intermediate files to inspect LTRANS output
gcc -O3 -flto main.o utils.o -o my_program -save-temps

# Dump the IPA inlining decisions (written next to the output)
gcc -O3 -flto main.o utils.o -o my_program -fdump-ipa-inline

# Confirm a function was inlined: look in the disassembly
objdump -d --no-show-raw-insn my_program | less
nm -C my_program | grep add            # symbol disappears if fully inlined and removed
```

Useful warnings:

- `-Wlto-type-mismatch` (on by default): detects ODR-style violations, the same name with incompatible types in different TUs. LTO sees _all_ TUs, so it can catch bugs the normal compiler never could.

---

## 6. Trade-offs

| Pros                                                                                              | Cons                                                                                         |
| ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Typically ~5-20% speedup on call-heavy code (workload dependent; sometimes 0%, occasionally more) | Much longer **link** time, since compilation work is shifted to the link step                |
| Can _shrink_ binaries via global dead-code elimination, ICF, and privatization                    | Can _grow_ binaries if aggressive inlining duplicates large functions                        |
| Keep modular source layout without performance penalty                                            | High RAM use at link time (full program IR in memory)                                        |
| Catches cross-TU type mismatches (ODR violations)                                                 | Harder debugging: inlined/merged code, confusing backtraces, less accurate line info         |
| Enables devirtualization and cross-file IPA                                                       | Incremental builds are slower: changing one file can re-trigger large amounts of LTRANS work |
| Works with PGO (profile-guided optimization) for even better inlining decisions                   | Toolchain friction: needs plugin support, `gcc-ar`, matching compiler versions               |

---

## 7. Common Pitfalls and Gotchas

1. **Forgetting `-flto` at link time**: slim objects then fail to link, or LTO silently doesn't run.
2. **Missing `-O` flags at link time**: the final codegen step uses the _link command's_ flags.
3. **Mismatched compiler versions** between compile and link: the IR format is version-specific and not stable across GCC releases.
4. **Using plain `ar`/`ranlib`**: use `gcc-ar`/`gcc-ranlib`.
5. **Symbols used only from inline assembly or linker scripts**: LTO may think they're unused and delete them. Mark with `__attribute__((used))`, or keep the file out of LTO.
6. **Symbols referenced from non-LTO code / shared libs**: the linker plugin normally handles this, but wrong visibility settings can cause surprises. Use `-fvisibility=hidden` + explicit exports for best results.
7. **Memory exhaustion on huge projects**: use `-flto=auto`, `-flto-partition=balanced`, or switch to Clang **ThinLTO**.
8. **Undefined behavior surfaces**: more visibility lets the optimizer exploit UB more aggressively. Code that "worked" may break. Test with `-fsanitize=undefined,address`.
9. **Debugging**: use `-g` plus `-Og`/`-O1` for debug builds and keep LTO for release builds only.
10. **Embedded / kernel / bare-metal**: startup code and interrupt vectors are often only referenced by the linker script, a classic LTO casualty.

---

## 8. When to Use It

- ✅ **Release builds** of performance-sensitive programs (servers, game engines, compilers, databases).
- ✅ Projects with many small functions spread across files.
- ✅ Combined with **PGO** (`-fprofile-generate` / `-fprofile-use`) for best results.
- ❌ Day-to-day debug/iteration builds where link time matters.
- ❌ Tiny programs (single file), where there is nothing to cross.

---

## 9. Quick Reference

|Goal|Flag|
|---|---|
|Enable LTO|`-flto`|
|Parallel LTO jobs|`-flto=auto` / `-flto=N`|
|ThinLTO (Clang)|`-flto=thin`|
|Fat objects (usable without LTO)|`-ffat-lto-objects`|
|Slim objects (default)|`-fno-fat-lto-objects`|
|Disable LTO for one file|`-fno-lto`|
|Choose partitioning|`-flto-partition=balanced`|
|Keep temporaries|`-save-temps`|
|Dump inlining decisions|`-fdump-ipa-inline`|
|Archive tools|`gcc-ar`, `gcc-ranlib`, `gcc-nm`|

---

## 10. Related Notes

- [[Compilation Pipeline]]
- [[Linkers and Relocations]]
- [[Function Inlining]]
- [[Profile-Guided Optimization (PGO)]]
- [[ELF Format]]
- [[Calling Conventions]]