# Bit-Level Operations in C

Bitwise operations form a critical component of low-level programming in C, providing direct manipulation of binary representations of data. These operations are defined for all integral data types and are commonly employed in system programming, embedded systems, and performance-critical applications. Unlike logical operators, which evaluate Boolean truth values, bitwise operators apply their rules to each individual bit in the operand.

---

## Bitwise Operators and Their Semantics

The four fundamental bitwise operators are defined as follows:

|Operator|Symbol|Definition|
|---|---|---|
|NOT|`~`|Produces the complement of each bit (0 becomes 1, 1 becomes 0).|
|AND|`&`|Produces 1 only when both operand bits are 1.|
|OR|`|`|
|XOR|`^`|Produces 1 when operand bits differ.|

### Truth Tables

Each operator can be formally expressed by a binary truth table:

**Bitwise NOT (`~x`)**

|Input|Output|
|---|---|
|0|1|
|1|0|

**Bitwise AND (`x & y`)**

| x   | y   | Result |
| --- | --- | ------ |
| 0   | 0   | 0      |
| 0   | 1   | 0      |
| 1   | 0   | 0      |
| 1   | 1   | 1      |

**Bitwise OR (`x | y`)**

|x|y|Result|
|---|---|---|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|1|

**Bitwise XOR (`x ^ y`)**

|x|y|Result|
|---|---|---|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|0|

---

## Worked Examples

The following examples demonstrate the evaluation of C expressions involving bit-level operators. Each case is presented by expanding the hexadecimal operand(s) into binary, applying the bitwise operation, and converting the result back to hexadecimal.

### Summary Table

|C Expression|Binary Expansion|Result (Binary)|Result (Hex)|
|---|---|---|---|
|`~0x41`|`0x41 = 0100 0001`|`1011 1110`|`0xBE`|
|`~0x00`|`0x00 = 0000 0000`|`1111 1111`|`0xFF`|
|`0x69 & 0x55`|`0x69 = 0110 1001`, `0x55 = 0101 0101`|`0100 0001`|`0x41`|
|`0x69 \| 0x55`|`0x69 = 0110 1001`, `0x55 = 0101 0101`|`0111 1101`|`0x7D`|
|`0x69 ^ 0x55`|`0x69 = 0110 1001`, `0x55 = 0101 0101`|`0011 1100`|`0x3C`|

---

### Example 1: Bitwise NOT (`~0x41`)

- Operand in binary:  
    `0x41 = 0100 0001`

- Applying bitwise NOT:  
    `~0100 0001 = 1011 1110`

- Convert back to hexadecimal:  
    `1011 1110 = 0xBE`

The bitwise NOT inverts every bit of the operand.

---

### Example 2: Bitwise NOT (`~0x00`)

- Operand in binary:  
    `0x00 = 0000 0000`

- Applying bitwise NOT:  
    `~0000 0000 = 1111 1111`

- Convert back to hexadecimal:  
    `1111 1111 = 0xFF`


In this case, zero becomes the maximum unsigned byte value.

---

### Example 3: Bitwise AND (`0x69 & 0x55`)

- Operands in binary:

``` 
  0x69 = 0110 1001 
  0x55 = 0101 0101 
__________________ 
Result = 0100 0001`
```

- Convert back to hexadecimal:  
    `0100 0001 = 0x41`


The result preserves only those bit positions where both operands contain `1`.

---

### Example 4: Bitwise OR (`0x69 | 0x55`)

- Operands in binary:

```
   0x69 = 0110 1001 
   0x55 = 0101 0101 
 __________________
 Result = 0111 1101`   
``` 

- Convert back to hexadecimal:  
    `0111 1101 = 0x7D`


The result contains `1` wherever either operand has a `1`.

---

### Example 5: Bitwise XOR (`0x69 ^ 0x55`)

- Operands in binary:

    `0x69 = 0110 1001 0x55 = 0101 0101 ---------------- Result = 0011 1100`

- Convert back to hexadecimal:  
    `0011 1100 = 0x3C`


The XOR operator highlights differing bits, producing `1` only where the operands differ.

---

#### Conclusion

Bit-level operations in C provide precise and deterministic manipulation of binary data. By expanding operands into binary, applying well-defined truth tables, and converting results back to hexadecimal, programmers obtain full transparency of the underlying operations. These examples illustrate the systematic nature of bitwise computation and its role as a foundation for systems programming, data encoding, and low-level algorithm optimization.