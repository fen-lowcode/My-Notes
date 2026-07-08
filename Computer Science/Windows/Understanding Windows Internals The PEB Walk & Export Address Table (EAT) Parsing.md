#windows #hacking 

When developing native applications for Windows, the operating system loader automatically parses an executable's **Import Address Table (IAT)** at startup to map required Dynamic Link Libraries (DLLs) into memory. However, advanced systems software, game anti-cheat engines, security tools, and malware frequently bypass this standard loader mechanism entirely.

This technique is known as a **PEB Walk** combined with **Manual Export Address Table (EAT) Parsing**. It allows a program to dynamically resolve functions directly from system memory at runtime, leaving the static Import Address Table completely clean when viewed in a disassembler like **IDA** or **Cutter**.

## 1. What is a PEB Walk?

Every process running under the Windows operating system contains an internal, user-mode data structure called the **Process Environment Block (PEB)**. The PEB is initialized and maintained by the kernel and resides within the application's virtual memory space. It acts as a master ledger for the process, storing details such as:

- Command-line arguments and environment variables.

- Active debugging flags (e.g., `BeingDebugged`).

- An explicit tracking ledger of every single DLL currently mapped into the process's RAM.


A **PEB Walk** is the manual traversal of these deeply nested kernel structures using low-level memory offsets and CPU segment registers. Instead of asking Windows where a library is, the application reads its own execution context to find out where the OS loader stored it.

## 2. Where Does It Get `kernel32.dll`?

To locate a critical subsystem library like `kernel32.dll` without making an API call, the code taps directly into the hardware configuration of the CPU:

- **x64 Architecture:** The CPU utilizes the **`GS` register** to track the Thread Environment Block (TEB). The pointer to the PEB structure is guaranteed by the Windows architecture to live at offset **`0x60`** relative to the `GS` base pointer (`gs:[0x60]`).
    
- **x86 (32-bit) Architecture:** The CPU utilizes the **`FS` register**, where the PEB pointer is located at offset **`0x30`** (`fs:[0x30]`).
    

By executing a raw CPU register read, the program finds the root pointer to the PEB. From there, it navigates a doubly-linked list called the `InLoadOrderModuleList` which contains sequentially linked nodes tracking every loaded module (`.exe` and `.dll`). The code jumps from link to link until it locates the node whose string descriptor matches `"kernel32.dll"`.

## 3. Why Must We Bypass MinGW Headers?

If you try to compile standard PEB traversal code using standard developer toolchains like **MinGW** or even the official **Microsoft Windows SDK**, the compiler will throw serious syntax errors claiming structures like `InLoadOrderModuleList` or `BaseDllName` do not exist.

### The Obstacle: Redacted Headers

Because the PEB is an internal, volatile operating system structure, Microsoft explicitly redacts or alters its definitions inside public developer headers (`<winternl.h>`). Microsoft does this to discourage normal desktop application developers from relying on fixed memory layouts that could theoretically change during major Windows structural updates.

### The Engineering Solution: Custom Struct Mapping

To make the code compile on any system without relying on the restrictive system headers, we explicitly reconstruct our own copies of the true, unredacted structures (`CUSTOM_PEB`, `CUSTOM_PEB_LDR_DATA`, etc.) at the top of our source code. This forces the compiler to treat the virtual memory layout exactly as it physically exists inside your RAM stick at runtime.

## 4. The Complete Blueprint Implementation

Save the following source code as `main.cpp`. It contains our custom unredacted structure layouts, the physical register lookup execution, and the low-level parsing logic required to extract `GetProcAddress`.

C++

``` C
#include <windows.h>
#include <iostream>

// ============================================================================
//   1. CUSTOM UNREDACTED WINDOWS INTERNAL STRUCTURES
// ============================================================================
// These definitions manually map the true physical layout of Windows memory,
// completely bypassing the redacted limitations of the default MinGW headers.

typedef struct _CUSTOM_UNICODE_STRING {
    USHORT Length;
    USHORT MaximumLength;
    PWSTR  Buffer;
} CUSTOM_UNICODE_STRING, *PCUSTOM_UNICODE_STRING;

typedef struct _CUSTOM_PEB_LDR_DATA {
    ULONG                   Length;
    BOOLEAN                 Initialized;
    HANDLE                  SsHandle;
    LIST_ENTRY              InLoadOrderModuleList; // Redacted by MinGW, but present in RAM
    LIST_ENTRY              InMemoryOrderModuleList;
    LIST_ENTRY              InInitializationOrderModuleList;
} CUSTOM_PEB_LDR_DATA, *PCUSTOM_PEB_LDR_DATA;

typedef struct _CUSTOM_LDR_DATA_TABLE_ENTRY {
    LIST_ENTRY              InLoadOrderLinks;
    LIST_ENTRY              InMemoryOrderLinks;
    LIST_ENTRY              InInitializationOrderLinks;
    PVOID                   DllBase;               // Base memory address of the loaded library
    PVOID                   EntryPoint;
    ULONG                   SizeOfImage;
    CUSTOM_UNICODE_STRING   FullDllName;
    CUSTOM_UNICODE_STRING   BaseDllName;            // Contains the string name of the module
} CUSTOM_LDR_DATA_TABLE_ENTRY, *PCUSTOM_LDR_DATA_TABLE_ENTRY;

typedef struct _CUSTOM_PEB {
    BOOLEAN                 InheritedAddressSpace;
    BOOLEAN                 ReadImageFileExecOptions;
    BOOLEAN                 BeingDebugged;
    union {
        BOOLEAN BitFields;
        struct {
            BOOLEAN ImageUsesLargePages : 1;
            BOOLEAN IsProtectedProcess : 1;
            BOOLEAN IsImageDynamicallyRelocated : 1;
            BOOLEAN SkipPatchingUser32Forwarders : 1;
            BOOLEAN IsPackagedProcess : 1;
            BOOLEAN IsAppContainer : 1;
            BOOLEAN IsProtectedProcessLight : 1;
            BOOLEAN IsLongPathAwareProcess : 1;
        };
    };
    HANDLE                  Mutant;
    PVOID                   ImageBaseAddress;
    PCUSTOM_PEB_LDR_DATA    Ldr;                   // Pointer to our module ledger tracking list
} CUSTOM_PEB, *PCUSTOM_PEB;

// Define a function pointer signature mirroring the original GetProcAddress API
typedef FARPROC(WINAPI* fnGetProcAddress)(HMODULE hModule, LPCSTR lpProcName);

// ============================================================================
//   2. THE PEB WALK FUNCTION
// ============================================================================
void* GetKernel32Base() {
#if defined(_WIN64)
    // Read the GS register at offset 0x60 to obtain the 64-bit PEB pointer
    PCUSTOM_PEB peb = (PCUSTOM_PEB)__readgsqword(0x60);
#else
    // Read the FS register at offset 0x30 if compiled for 32-bit x86 environments
    PCUSTOM_PEB peb = (PCUSTOM_PEB)__readfsdword(0x30);
#endif

    // Access the loader data tracking structure inside the PEB
    PCUSTOM_PEB_LDR_DATA ldr = peb->Ldr;

    // Isolate the head pointer of the doubly-linked list of loaded modules
    PLIST_ENTRY listHead = &ldr->InLoadOrderModuleList;
    PLIST_ENTRY currentEntry = listHead->Flink;

    // Walk sequentially down the link nodes until we circle back to the start
    while (currentEntry != listHead) {
        // Calculate the raw absolute entry location using structural alignments
        PCUSTOM_LDR_DATA_TABLE_ENTRY moduleEntry = CONTAINING_RECORD(currentEntry, CUSTOM_LDR_DATA_TABLE_ENTRY, InLoadOrderLinks);
        
        // Safety verification check: verify the module tracking name string buffer isn't void
        if (moduleEntry->BaseDllName.Buffer != nullptr) {
            // Analyze the string data to isolate kernel32.dll
            if (wcsstr(moduleEntry->BaseDllName.Buffer, L"kernel32.dll") != nullptr || 
                wcsstr(moduleEntry->BaseDllName.Buffer, L"KERNEL32.DLL") != nullptr) {
                
                // Extraction point: return the true base address allocation of kernel32 in RAM
                return moduleEntry->DllBase;
            }
        }
        // Advance to the next node in the chain
        currentEntry = currentEntry->Flink;
    }
    return nullptr;
}

// ============================================================================
//   3. THE EXPORT ADDRESS TABLE (EAT) PARSING FUNCTION
// ============================================================================
FARPROC GetProcAddressManually(void* dllBase) {
    unsigned char* base = (unsigned char*)dllBase;

    // Parse the legacy DOS Header to confirm identity and locate structural gaps
    PIMAGE_DOS_HEADER dosHeader = (PIMAGE_DOS_HEADER)base;
    
    // Jump to the definitive Portable Executable (PE) header via its designated offset
    PIMAGE_NT_HEADERS ntHeaders = (PIMAGE_NT_HEADERS)(base + dosHeader->e_lfanew);

    // Pull out the Export Data Directory locating export allocations inside the image
    IMAGE_DATA_DIRECTORY exportDir = ntHeaders->OptionalHeader.DataDirectory[IMAGE_DIRECTORY_ENTRY_EXPORT];
    PIMAGE_EXPORT_DIRECTORY exports = (PIMAGE_EXPORT_DIRECTORY)(base + exportDir.VirtualAddress);

    // Cast parallel structural arrays containing the symbol phonebook data
    DWORD* names = (DWORD*)(base + exports->AddressOfNames);
    DWORD* functions = (DWORD*)(base + exports->AddressOfFunctions);
    WORD* ordinals = (WORD*)(base + exports->AddressOfNameOrdinals);

    // Loop through the alphanumeric name arrays published by the library
    for (DWORD i = 0; i < exports->NumberOfNames; i++) {
        char* funcName = (char*)(base + names[i]);
        
        // Execute a raw match check seeking the symbol label "GetProcAddress"
        if (strcmp(funcName, "GetProcAddress") == 0) {
            // Use the corresponding index via the ordinal array to match function pointers
            WORD ordinal = ordinals[i];
            
            // Final calculation: Add the export offset directly onto the module base pointer
            return (FARPROC)(base + functions[ordinal]); 
        }
    }
    return nullptr;
}

// ============================================================================
//   4. PROGRAM RUNTIME ENTRYPOINT
// ============================================================================
int main() {
    std::cout << "[+] Performing PEB Walk..." << std::endl;
    void* kernel32 = GetKernel32Base();
    std::cout << "[+] Found kernel32.dll loaded at RAM Address: " << kernel32 << std::endl;

    std::cout << "[+] Parsing Export Address Table manually..." << std::endl;
    fnGetProcAddress MyGetProcAddress = (fnGetProcAddress)GetProcAddressManually(kernel32);
    std::cout << "[+] Successfully resolved GetProcAddress at: " << (void*)MyGetProcAddress << std::endl;

    return 0;
}
```

## 5. In-Depth Code Explanation

### Section 1: The Structures

C++

```
typedef struct _CUSTOM_PEB_LDR_DATA { ... LIST_ENTRY InLoadOrderModuleList; ... };
```

We redefine the internal structures manually. By using native primitive variables (`ULONG`, `BOOLEAN`, `HANDLE`), we lay out the variables sequentially in memory to perfectly mirror what Windows executes inside user-mode RAM. This forces the compiler to recognize hidden properties like `InLoadOrderModuleList` which standard MinGW versions of `<winternl.h>` omit.

### Section 2: Thread Register Extraction

C++

```
PCUSTOM_PEB peb = (PCUSTOM_PEB)__readgsqword(0x60);
```

`__readgsqword` is a compiler intrinsic function that compiles down to a single hardware CPU read: `mov rax, gs:[60h]`. This bypasses all Windows APIs and pulls the base memory address of the Process Environment Block directly out of the processor execution context.

### Section 3: The List Iterator Loop

C++

```
while (currentEntry != listHead) { ... currentEntry = currentEntry->Flink; }
```

The loader registers linked modules sequentially using a circular doubly-linked list. `Flink` stands for _Forward Link_ (a pointer to the next module in line). The loop tracks from node to node until it circles completely back to the initial list head.

### Section 4: Structural Target Extraction

C++

```
PCUSTOM_LDR_DATA_TABLE_ENTRY moduleEntry = CONTAINING_RECORD(currentEntry, CUSTOM_LDR_DATA_TABLE_ENTRY, InLoadOrderLinks);
```

Because `currentEntry` is merely pointing to a nested list variable _inside_ a larger structure block, the `CONTAINING_RECORD` macro executes hexadecimal subtraction math to determine the true, absolute starting location of the entire module entry record within RAM.

### Section 5: Portable Executable Header Traversal

C++

```
PIMAGE_NT_HEADERS ntHeaders = (PIMAGE_NT_HEADERS)(base + dosHeader->e_lfanew);
```

All Windows modules align to the Portable Executable (PE) blueprint. `e_lfanew` is a structural offset located inside the legacy DOS header ('MZ'). Adding this offset to the library's starting base address accurately lands our pointer directly on top of the modern 'PE' signature headers.

### Section 6: Name Array Traversal

C++

```
for (DWORD i = 0; i < exports->NumberOfNames; i++) { ... if (strcmp(funcName, "GetProcAddress") == 0) ... }
```

The Export Address Table exposes an internal database index string array (`AddressOfNames`). The loop acts like an analytical telephone book scanner, testing each public string layout exported by `kernel32.dll` sequentially until it detects an explicit text match for `"GetProcAddress"`.

### Section 7: Final Pointer Calculation

C++

```
WORD ordinal = ordinals[i];
return (FARPROC)(base + functions[ordinal]);
```

Once the array index `i` matches the name string, the program reads the parallel **Ordinal Array** (`AddressOfNameOrdinals`) at that exact index to obtain the true function address lookup identifier. It then references the **Function Pointer Table** (`AddressOfFunctions`) using that ordinal index to pull out the relative virtual address offset. Adding this offset directly back onto the library base memory allocation address (`base`) outputs the definitive 64-bit RAM pointer leading directly to the executable instructions of `GetProcAddress`.

## 6. How to Compile the Blueprints

To compile this code cleanly into a functioning Windows native application, run the following command directly from your terminal interface:

Bash

```
g++ main.cpp -o pedwalk.exe -static
```

### Explanation of flags:

- **`-o pedwalk.exe`**: Formats the final output file specifically as a Windows Portable Executable.
    
- **`-static`**: Forces the compiler to integrate all foundational C++ runtime libraries directly within the standalone binary executable, eliminating missing dynamic module dependencies if transferred into a test sandbox.
    

## 7. Analysis of the Result in IDA Pro

If you take your newly compiled `pedwalk.exe` and open it inside **IDA Pro** or **Cutter**:

1. Check the **Imports** tab. `GetProcAddress` will be completely absent from the `KERNEL32.dll` section.
    
2. Go to the **Disassembly View** (`IDA View-A`) and trace your `GetKernel32Base` logic block. You will find that your C++ code was perfectly optimized down into the direct hardware register lookup instruction:
    
    Code snippet
    
    ```
    mov     rax, gs:[60h]
    ```
    

By utilizing a **PEB Walk** and manual **Export Parsing**, you have completely taken over the duties of the native operating system loader, giving you deep visibility into exactly how low-level Windows structures and processes organize themselves in memory.