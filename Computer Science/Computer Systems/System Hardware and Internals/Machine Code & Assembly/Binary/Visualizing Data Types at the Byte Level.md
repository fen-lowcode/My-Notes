

```c
#include <stdio.h>

typedef unsigned char *byte_pointer;

void show_bytes(byte_pointer start, size_t len) {
    int i;
    for (i = 0; i < len; i++) {
        printf(" %.2x", start[i]);
    }

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


>[!Important]
This program demonstrates how to inspect and print the byte-level memory representation of different C data types (`int`, `float`, and `void *`). It uses type casting to treat the memory of any variable as a sequence of bytes, which is helpful for understanding how data is stored in memory, how endianness works, and how memory layout varies across machine architectures (32-bit vs. 64-bit, little vs. big endian). It's a valuable learning tool for systems programming and low-level debugging.

---
#### Demonstrate 

```c
  
int main() {

	show_int(40);
	show_float(40.0);
	show_pointer((void*)40);

return 0;

}
```

> **Output**

```c
○ → ./main 
 28 00 00 00
 00 00 20 42
 28 00 00 00 00 00 00 00

```


---
### Interpretation of Values Byte Representations

|Value|Type|Hex Value|Little-Endian Byte Order (8 bytes for pointer)|
|---|---|---|---|
|`a = 40`|`int`|`0x00000028`|`28 00 00 00`|
|`b = 40.0f`|`float`|`0x42200000`|`00 00 20 42`|
|`p = &a`|`void *`|`0x0000000000000028`|`28 00 00 00 00 00 00 00` (pointer to address `0x28`)|

##### Final Summary Table — Little Endian, 64-bit Linux (Intel)

|**Expression**|**Data Type**|**Memory Representation (Hex Bytes)**|**Interpretation**|
|---|---|---|---|
|`a = 40`|`int` (4 bytes)|`28 00 00 00`|Decimal 40|
|`b = 40.0`|`float` (4 bytes)|`00 00 20 42`|IEEE 754 float for 40.0|
|`p = &a`|`void *` (8 bytes)|`28 00 00 00 00 00 00 00`|Address of `a` (`0x0000000000000028`)|

All three values represent **40**, but in **different contexts**:

- As an `int` literal

- As a `float` in IEEE 754 format

- As a **memory address** (pointer to `a`) — which just happens to be `0x28` in this case

---