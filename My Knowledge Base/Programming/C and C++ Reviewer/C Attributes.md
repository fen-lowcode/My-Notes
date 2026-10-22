#programming #compiler #linux

### Standard C23 attributes (`[[...]]` syntax)

| Attribute                                  | Purpose                                                                                         |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| `[[deprecated]]` / `[[deprecated("msg")]]` | Warn when the entity is used                                                                    |
| `[[fallthrough]]`                          | Mark an intentional switch-case fallthrough                                                     |
| `[[maybe_unused]]`                         | Suppress unused-variable/parameter/function warnings                                            |
| `[[nodiscard]]` / `[[nodiscard("msg")]]`   | Warn if the return value is ignored                                                             |
| `[[noreturn]]`                             | Function never returns (replaces `_Noreturn`)                                                   |
| `[[unsequenced]]`                          | Function is stateless and idempotent, so calls can be reordered or merged                       |
| `[[reproducible]]`                         | Function has no side effects on observable state, but may read state that changes between calls |

Any of these can also be spelled `[[__name__]]`, and vendor attributes use a prefix, such as `[[gnu::unused]]`.

### GNU attributes (`__attribute__((...))`)

**Optimization and inlining**

- `always_inline`: force inlining
- `noinline`: forbid inlining
- `noclone`: don’t clone the function for optimization
- `noipa`: disable interprocedural optimizations involving it
- `flatten`: inline every call inside this function
- `hot` / `cold`: optimization and placement hints
- `pure`: no side effects, may read globals
- `const`: no side effects, reads no globals (stricter than `pure`)
- `optimize("...")`: per-function optimization flags
- `leaf`: function doesn’t call back into the current translation unit
- `gnu_inline`: use GNU89 inline semantics
- `artificial`: debugger treats inlined body as one line
- `no_icf`: don’t merge with identical functions
- `no_reorder`: keep the definition’s position in the output

**Calling and returns**

- `noreturn`: never returns
- `nothrow`: doesn’t throw exceptions
- `returns_twice`: like `setjmp`
- `malloc`: return value doesn’t alias other pointers
- `returns_nonnull`: never returns NULL
- `warn_unused_result`: warn if result is ignored
- `nonnull(...)`: pointer args must not be NULL
- `sentinel`: variadic list must end with NULL
- `alloc_size(n)` / `alloc_align(n)`: describe allocator results
- `assume_aligned(n)`: returned pointer is aligned
- `access(mode, ref, size)`: describe how a pointer arg is read/written
- `fd_arg(n)`: argument is a file descriptor
- `format(printf, fmt, args)` / `format_arg(n)`: printf/scanf-style format checking
- `simd`: generate vector variants

**Diagnostics and constructors**

- `deprecated("msg")`, `unused`, `used`, `retain`
- `warning("msg")` / `error("msg")`: emit a diagnostic if the call survives optimization
- `constructor` / `destructor`: run before `main` / after exit
- `cleanup(fn)`: call `fn` when the variable goes out of scope
- `fallthrough`: statement form of the standard attribute
- `copy(other)`: copy attributes from another declaration
- `nonstring`: char array is not NUL-terminated
- `tainted_args`: mark untrusted-input functions for analysis

**Linkage, symbols, and sections**

- `alias("target")`: define a symbol as an alias
- `weak` / `weakref`: weak symbol linkage
- `visibility("default|hidden|protected|internal")`
- `section("name")`: place in a given section
- `ifunc("resolver")`: choose the implementation at load time
- `symver("name@VER")`: ELF symbol versioning
- `externally_visible`: keep it visible under LTO
- `noplt`: call through the GOT directly
- `common` / `nocommon`: tentative-definition handling
- `noinit` / `persistent`: place in a non-zeroed or persistent section (embedded)

**Layout and types**

- `aligned(n)`: alignment
- `packed`: no padding
- `mode(...)`: choose integer/float width by machine mode
- `vector_size(n)`: SIMD vector types
- `transparent_union`: union args accept any member type
- `may_alias`: type can alias anything (like `char`)
- `designated_init`: require designated initializers
- `scalar_storage_order("big-endian"|"little-endian")`: set struct byte order
- `uninitialized`: don’t auto-initialize under `-ftrivial-auto-var-init`

**Safety, sanitizers, and instrumentation**

- `no_sanitize("address"|"undefined"|...)`, `no_sanitize_address`, `no_sanitize_thread`, `no_sanitize_undefined`
- `no_instrument_function`: skip `-finstrument-functions` hooks
- `no_profile_instrument_function`
- `no_stack_protector` / `stack_protect`
- `no_split_stack`
- `zero_call_used_regs("...")`: clear registers on return
- `patchable_function_entry(N,M)`: reserve NOPs for live patching
- `nocf_check` / `cf_check`: control-flow-protection behavior

**Target-specific (x86 examples)**

- `target("...")` and `target_clones("...")`: ISA features and multiversioning
- `ms_abi` / `sysv_abi`: Windows or System V calling convention
- `regparm(n)`, `stdcall`, `fastcall`, `cdecl`, `thiscall`, `sseregparm`: 32-bit conventions
- `force_align_arg_pointer`: realign the stack on entry
- `interrupt` / `no_caller_saved_registers`: interrupt handlers
- `naked`: no prologue or epilogue, body is pure asm
- `ms_struct` / `gcc_struct`: struct layout rules
- `indirect_branch`, `function_return`: retpoline-style mitigations
- `ms_hook_prologue`, `fentry_name`: hot-patching support

ARM, AArch64, RISC-V, AVR, MIPS, PowerPC and others each add their own set (`interrupt`, `signal`, `long_call`, `section`-related, and so on). Clang also supports most of the above plus its own (`availability`, `overloadable`, `cleanup`, `analyzer_noreturn`, and more).
