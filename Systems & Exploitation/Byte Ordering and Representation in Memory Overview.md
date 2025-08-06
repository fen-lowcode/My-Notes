
### Addressing Multi-Byte Program Objects

For program objects that span multiple bytes, two conventions must be established:

1. **Object Address**  
    In virtually all machines, a multi-byte object is stored as a **contiguous sequence of bytes**, with the object’s address being the **smallest byte address** it occupies.
    
    > Example:  
    > Suppose a variable `x` of type `int` has address `0x100`. Assuming `int` is 32 bits (4 bytes), the bytes of `x` will be stored in memory at locations:  
    > `0x100`, `0x101`, `0x102`, and `0x103`.
    
2. **Byte Ordering**  
    Two common conventions are used to determine how multi-byte values are stored:
    
    - **Big Endian**: Stores the **most significant byte first** (at the lowest address).
    
    - **Little Endian**: Stores the **least significant byte first**.
    
	**Refer to [[Visualizing Data Types at the Byte Level]]**

---

### Endianness in Practice

Consider a 32-bit integer `0x01234567`. The high-order byte is `0x01`, and the low-order byte is `0x67`.

- **Big Endian:**

    ```
    Address:  0x100  0x101  0x102  0x103
    Bytes:    0x01   0x23   0x45   0x67
    ```

- **Little Endian:**

    ```
    Address:  0x100  0x101  0x102  0x103
    Bytes:    0x67   0x45   0x23   0x01
    ```


Most Intel-compatible machines use **little-endian** byte ordering. In contrast, many IBM and Oracle machines use **big-endian** ordering. Some modern processors (e.g., ARM) are **bi-endian**, meaning they support both conventions, though operating systems typically choose one and enforce it.

---

##### Historical Context: The Origin of "Endianness"

The terms **big endian** and **little endian** originate from **Jonathan Swift's _Gulliver’s Travels_**. In the story, two warring factions disagreed over which end of an egg to break — the big or the small end. This satire mirrors the arbitrary but contentious debate in computing over byte ordering.

> **Quote from _Gulliver’s Travels_ (1726)**:  
> "It is allowed on all hands, that the primitive way of breaking eggs... was upon the larger end... the emperor published an edict commanding all his subjects... to break the smaller end of their eggs..."

---

### When Byte Ordering Matters

Although endianness is mostly invisible to application programmers, it becomes important in the following scenarios:

##### 1. **Networking**

When binary data is sent between different architectures (e.g., from a little-endian to a big-endian system), byte order mismatches can cause errors.

- **Solution**: Use **network byte order** conventions.

- Convert data at the sender to network format, and at the receiver back to host format.

##### 2. **Inspecting Machine-Level Programs**

Assembly instructions are often viewed as byte sequences, and on little-endian machines, these sequences appear reversed.

> **Example:**
> 
> ```
> Address:    4004d3
> Bytes:      01 05 43 0b 20 00
> Instruction: add %eax, 0x200b43(%rip)
> ```
> 
> The address `0x200b43` is stored in reverse byte order as `43 0b 20 00`.

##### 3. **Low-Level Type Manipulation**

In C, using `cast` or `union` allows referencing an object with a different type, revealing its byte-level representation. This is generally discouraged in application programming but is useful in system-level programming.

---

### C Code Example: Printing Byte Representations

```c
#include <stdio.h>
typedef unsigned char *byte_pointer;

void show_bytes(byte_pointer start, size_t len) {
    for (int i = 0; i < len; i++)
        printf(" %.2x", start[i]);
    printf("\n");
}

void show_int(int x) {
    show_bytes((byte_pointer) &x, sizeof(int));
}

void show_float(float x) {
    show_bytes((byte_pointer) &x, sizeof(float));
}

void show_pointer(void *x) {
    show_bytes((byte_pointer) &x, sizeof(void *));
}
```

This code demonstrates how to access and print the byte-level representation of `int`, `float`, and pointer types using type casting.

---

## Example Usage and Output

### Test Function

```c
void test_show_bytes(int val) {
    int ival = val;
    float fval = (float) ival;
    int *pval = &ival;
    show_int(ival);
    show_float(fval);
    show_pointer(pval);
}
```

### Machine Results for `val = 12345` (0x00003039)

|Machine|Value|Type|Byte Representation (hex)|
|---|---|---|---|
|Linux 32|12345|`int`|`39 30 00 00`|
|Windows|12345|`int`|`39 30 00 00`|
|Sun|12345|`int`|`00 00 30 39`|
|Linux 64|12345|`int`|`39 30 00 00`|
|Linux 32|12345.0|`float`|`00 e4 40 46`|
|Windows|12345.0|`float`|`00 e4 40 46`|
|Sun|12345.0|`float`|`46 40 e4 00`|
|Linux 64|12345.0|`float`|`00 e4 40 46`|

**Note**:

- All differences are due to **endianness**, not numerical value.

- `float` and `int` encodings differ due to distinct representation formats.


---

### Notes on C Programming

##### `typedef` for Readability

```c
typedef int *int_pointer;
int_pointer ip;  // Equivalent to int *ip;
```

##### `printf` Formatting

- `%d`: Decimal integer

- `%f`: Floating-point number

- `%c`: Character

- `%.2x`: Hexadecimal, 2 digits minimum


##### Pointers and Arrays

C allows array-style access using pointers:

```c
start[i] // Equivalent to *(start + i)
```

### Address-of and Casting

- `&x` gets the address of variable `x`.

- `(byte_pointer)&x` treats the address as a pointer to bytes (`unsigned char *`).


---

### Additional Tip: Viewing ASCII Table

To display the ASCII character table in Linux:

```sh
man ascii
```

This command will show a complete table of ASCII codes.

---

