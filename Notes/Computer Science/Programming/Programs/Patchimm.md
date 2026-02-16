#custom-software

```c
#include <stdio.h>    // Standard I/O functions: fopen, fclose, fwrite, perror, printf
#include <stdlib.h>   // Standard library functions: malloc, free, exit, strtol, strtoul
#include <stdint.h>   // Fixed-width integer types: uint8_t
#include <string.h>   // String functions: strlen

// Simple function to handle fatal errors.
// Prints an error message and exits the program.
void fail(const char *msg) {
    perror(msg);     // Prints msg followed by system error string.
    exit(EXIT_FAILURE); // Exits the program with a failure status.
}

// Converts a hex string (e.g., "DEADBEEF") to raw bytes.
// hexstr: input string containing hex characters.
// buffer: output buffer to store byte values.
// num_bytes: how many bytes to convert from hex string.
void hex_to_bytes(const char *hexstr, uint8_t *buffer, size_t num_bytes) {
    for (size_t i = 0; i < num_bytes; i++) {
        // sscanf reads two hex characters and converts them into a byte.
        // %2hhx tells sscanf to read 2 hex digits and store as unsigned char.
        if (sscanf(hexstr + 2*i, "%2hhx", &buffer[i]) != 1) {
            // If conversion fails, print the error position and exit.
            fprintf(stderr, "Invalid hex character at position %zu\n", i*2);
            exit(EXIT_FAILURE);
        }
    }
}

int main(int argc, char *argv[]) {
    // Check that the user provided exactly 4 arguments:
    // filename, offset in hex, number of bytes, and hex string.
    if (argc != 5) {
        fprintf(stderr, "Usage: %s <file> <offset_hex> <num_bytes> <bytes_hex>\n", argv[0]);
        fprintf(stderr, "Example: %s program 0x113d 5 DEADBEEF01\n", argv[0]);
        return 1;
    }

    // Parse command-line arguments
    const char *filename = argv[1];          // File to patch
    long offset = strtol(argv[2], NULL, 16); // Offset in file (hex) to start patching
    size_t num_bytes = strtoul(argv[3], NULL, 10); // Number of bytes to patch
    const char *hexstr = argv[4];            // Hex string to write

    // Ensure that the hex string length matches the number of bytes
    if (strlen(hexstr) != num_bytes * 2) {
        fprintf(stderr, "Error: hex string length does not match num_bytes\n");
        return 1;
    }

    // Allocate memory for bytes to patch
    uint8_t *patch_bytes = malloc(num_bytes);
    if (!patch_bytes) fail("malloc"); // If allocation fails, exit

    // Convert the hex string into raw bytes
    hex_to_bytes(hexstr, patch_bytes, num_bytes);

    // Open the target file in read+write binary mode
    FILE *f = fopen(filename, "r+b");
    if (!f) fail("fopen"); // If file cannot be opened, exit

    // Move the file pointer to the specified offset
    if (fseek(f, offset, SEEK_SET) != 0) fail("fseek");

    // Write the patch bytes to the file at the offset
    if (fwrite(patch_bytes, 1, num_bytes, f) != num_bytes) fail("fwrite");

    // Close the file and free allocated memory
    fclose(f);
    free(patch_bytes);

    // Print a success message
    printf("Patched %zu bytes at offset 0x%lx in %s\n", num_bytes, offset, filename);

    return 0; // Exit normally
}
```

**Detailed explanation:**

- `argc` and `argv`: Handle command-line input. `argc` is the argument count, `argv` is an array of strings. You expect 4 user inputs after the program name.
- `strtol` and `strtoul`: Convert strings to integers. `strtol` for signed long (hex offset), `strtoul` for unsigned long (number of bytes).
- `malloc`: Allocates memory for the bytes that will overwrite the file content.
- `fopen` with `"r+b"`: Opens the file in binary read/write mode. `"b"` is important for non-text files.
- `fseek`: Moves the file pointer to the exact offset where patching starts.
- `fwrite`: Writes the patch bytes into the file. Ensures the correct number of bytes is written.
- `sscanf` with `%2hhx`: Converts 2-character hex strings to a single byte.
- Checks: Length of hex string matches the number of bytes, and proper error handling is used. 

This program lets you patch arbitrary bytes into a binary file safely, with proper error reporting and memory management.
