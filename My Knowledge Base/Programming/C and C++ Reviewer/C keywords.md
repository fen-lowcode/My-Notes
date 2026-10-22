

This is a list of reserved keywords in C. Since they are used by the language, these keywords are not available for re-definition. _(since C23)_ As an exception, they are not considered reserved in [attribute-tokens](https://claude.ai/chat/c/language/attributes).

|||||
|---|---|---|---|
|`alignas` (C23)|`extern`|`sizeof`|`_Alignas` (C11, deprecated in C23)|
|`alignof` (C23)|`false` (C23)|`static`|`_Alignof` (C11, deprecated in C23)|
|`auto`|`float`|`static_assert` (C23)|`_Atomic` (C11)|
|`bool` (C23)|`for`|`struct`|`_BitInt` (C23)|
|`break`|`goto`|`switch`|`_Bool` (C99, deprecated in C23)|
|`case`|`if`|`thread_local` (C23)|`_Complex` (C99)|
|`char`|`inline` (C99)|`true` (C23)|`_Decimal128` (C23)|
|`const`|`int`|`typedef`|`_Decimal32` (C23)|
|`constexpr` (C23)|`long`|`typeof` (C23)|`_Decimal64` (C23)|
|`continue`|`nullptr` (C23)|`typeof_unqual` (C23)|`_Generic` (C11)|
|`default`|`register`|`union`|`_Imaginary` (C99)|
|`do`|`restrict` (C99)|`unsigned`|`_Noreturn` (C11, deprecated in C23)|
|`double`|`return`|`void`|`_Static_assert` (C11, deprecated in C23)|
|`else`|`short`|`volatile`|`_Thread_local` (C11, deprecated in C23)|
|`enum`|`signed`|`while`||

The most common keywords that begin with an underscore are generally used through their convenience macros:

|Keyword|Used as|Defined in|
|---|---|---|
|`_Alignas` (C11, deprecated in C23)|`alignas` (until C23, removed)|`stdalign.h`|
|`_Alignof` (C11, deprecated in C23)|`alignof` (until C23, removed)|`stdalign.h`|
|`_Atomic` (C11)|`atomic_bool`, `atomic_int`, ...|`stdatomic.h`|
|`_BitInt` (C23)|(no macro)||
|`_Bool` (C99, deprecated in C23)|`bool` (until C23, removed)|`stdbool.h`|
|`_Complex` (C99)|`complex`|`complex.h`|
|`_Decimal128` (C23)|(no macro)||
|`_Decimal32` (C23)|(no macro)||
|`_Decimal64` (C23)|(no macro)||
|`_Generic` (C11)|(no macro)||
|`_Imaginary` (C99)|`imaginary`|`complex.h`|
|`_Noreturn` (C11, deprecated in C23)|`noreturn`|`stdnoreturn.h`|
|`_Static_assert` (C11, deprecated in C23)|`static_assert` (until C23, removed)|`assert.h`|
|`_Thread_local` (C11, deprecated in C23)|`thread_local` (until C23, removed)|`threads.h`|

_(since C23)_ Some keywords are deprecated and retained as alternative spellings for compatibility purposes. These can be used wherever the keyword can.

|Keyword|Alternative spelling|
|---|---|
|`alignas` (C23)|`_Alignas` (C11, deprecated in C23)|
|`alignof` (C23)|`_Alignof` (C11, deprecated in C23)|
|`bool` (C23)|`_Bool` (C99, deprecated in C23)|
|`static_assert` (C23)|`_Static_assert` (C11, deprecated in C23)|
|`thread_local` (C23)|`_Thread_local` (C11, deprecated in C23)|

It is unspecified whether any of the spellings of these keywords, their alternate forms, or `true` and `false` is implemented as a predefined macro.

Each name that begins with a double underscore `__` or an underscore `_` followed by an uppercase letter is reserved: see [identifier](https://claude.ai/chat/c/language/identifier#Reserved_identifiers) for details.

Note that digraphs `<%`, `%>`, `<:`, `:>`, `%:`, and `%:%:` provide an [alternative way to represent standard tokens](https://claude.ai/chat/c/language/operator_alternative).

The following tokens are recognized by the [preprocessor](https://claude.ai/chat/c/preprocessor) when they are used _within_ the context of a preprocessor directive:

|||||
|---|---|---|---|
|`if`|`ifdef`|`include`|`defined`|
|`elif`|`ifndef`|`embed` (C23)|`__has_include` (C23)|
|`else`|`elifdef` (C23)|`line`|`__has_embed` (C23)|
|`endif`|`elifndef` (C23)|`error`|`__has_c_attribute` (C23)|
||`define`|`warning` (C23)||
||`undef`|`pragma`||

The following tokens are recognized by the preprocessor when they are used _outside_ the context of a preprocessor directive:

- `_Pragma` (C99)

The following additional keywords are classified as extensions and conditionally-supported:

- `asm`
- `fortran`

## References

- **C23 standard (ISO/IEC 9899:2024)**
    - 6.4.1 Keywords (p. 53)
    - J.5.9 The fortran keyword (p. 601)
    - J.5.10 The asm keyword (p. 601)
- **C17 standard (ISO/IEC 9899:2018)**
    - 6.4.1 Keywords (p. 42-43)
    - J.5.9 The fortran keyword (p. 422)
    - J.5.10 The asm keyword (p. 422)
- **C11 standard (ISO/IEC 9899:2011)**
    - 6.4.1 Keywords (p. 58-59)
    - J.5.9 The fortran keyword (p. 580)
    - J.5.10 The asm keyword (p. 580)
- **C99 standard (ISO/IEC 9899:1999)**
    - 6.4.1 Keywords (p. 50)
    - J.5.9 The fortran keyword (p. 514)
    - J.5.10 The asm keyword (p. 514)
- **C89/C90 standard (ISO/IEC 9899:1990)**
    - 3.1.1 Keywords
    - G.5.9 The fortran keyword
    - G.5.10 The asm keyword

## See also

- [C++ keywords](https://claude.ai/chat/cpp/keyword)