
#windows #operaing-systems #hacking 


``` C
#include <windows.h>


// --- CUSTOM STB TYPES FOR MANJARI/MINGW ENVIRONMENT ---

typedef struct _CUSTOM_UNICODE_STRING {

    USHORT Length;
    USHORT MaximumLength;
    PWSTR  Buffer;

} CUSTOM_UNICODE_STRING, *PCUSTOM_UNICODE_STRING;

  

typedef struct _CUSTOM_PEB_LDR_DATA {

    ULONG                   Length;
    BOOLEAN                 Initialized;
    HANDLE                  SsHandle;
    LIST_ENTRY              InLoadOrderModuleList;
    LIST_ENTRY              InMemoryOrderModuleList;
    LIST_ENTRY              InInitializationOrderModuleList;

} CUSTOM_PEB_LDR_DATA, *PCUSTOM_PEB_LDR_DATA;

  

typedef struct _CUSTOM_LDR_DATA_TABLE_ENTRY {

    LIST_ENTRY              InLoadOrderLinks;
    LIST_ENTRY              InMemoryOrderLinks;
    LIST_ENTRY              InInitializationOrderLinks;
    PVOID                   DllBase;
    PVOID                   EntryPoint;
    ULONG                   SizeOfImage;
    CUSTOM_UNICODE_STRING   FullDllName;
    CUSTOM_UNICODE_STRING   BaseDllName;

} CUSTOM_LDR_DATA_TABLE_ENTRY, *PCUSTOM_LDR_DATA_TABLE_ENTRY;

  

typedef struct _CUSTOM_PEB {

    BYTE                    Reserved1[2];
    BYTE                    BeingDebugged;
    BYTE                    Reserved2[21];
    PCUSTOM_PEB_LDR_DATA    Ldr;

} CUSTOM_PEB, *PCUSTOM_PEB;

  

// --- DYNAMIC FUNCTION SIGNATURES ---

typedef FARPROC(WINAPI* fnGetProcAddress)(HMODULE hModule, LPCSTR lpProcName);
typedef HANDLE(WINAPI* fnGetStdHandle)(DWORD nStdHandle);
typedef BOOL(WINAPI* fnWriteFile)(HANDLE hFile, LPCVOID lpBuffer, DWORD nNumberOfBytesToWrite, LPDWORD lpNumberOfBytesWritten, LPOVERLAPPED lpOverlapped);

  

// --- CUSTOM INLINE STRING UTILITIES (Bypassing libc dependencies) ---

int CustomStrCmp(const char* s1, const char* s2) {
    while (*s1 && (*s1 == *s2)) {
        s1++;
        s2++;
    }
    return *(const unsigned char*)s1 - *(const unsigned char*)s2;
}

  

bool CustomWideStrContainsKernel32(const wchar_t* hstack) {

    if (!hstack) return false;

    while (*hstack) {
        if ((hstack[0] == L'k' || hstack[0] == L'K') &&
            (hstack[1] == L'e' || hstack[1] == L'E') &&
            (hstack[2] == L'r' || hstack[2] == L'R') &&
            (hstack[3] == L'n' || hstack[3] == L'N') &&
            (hstack[4] == L'e' || hstack[4] == L'E') &&
            (hstack[5] == L'l' || hstack[5] == L'L') &&
            (hstack[6] == L'3') &&
            (hstack[7] == L'2')) {
            return true;
        }
        hstack++;
    }

    return false;
}

  

// --- SYSTEM PARSING FUNCTIONS ---

void* GetKernel32Base() {

    PCUSTOM_PEB peb = (PCUSTOM_PEB)__readgsqword(0x60);
    PCUSTOM_PEB_LDR_DATA ldr = peb->Ldr;
    PLIST_ENTRY listHead = &ldr->InLoadOrderModuleList;
    PLIST_ENTRY currentEntry = listHead->Flink;

  

    while (currentEntry != listHead) {

        PCUSTOM_LDR_DATA_TABLE_ENTRY moduleEntry = CONTAINING_RECORD(currentEntry, CUSTOM_LDR_DATA_TABLE_ENTRY, InLoadOrderLinks);

        if (moduleEntry->BaseDllName.Buffer != nullptr) {
            if (CustomWideStrContainsKernel32(moduleEntry->BaseDllName.Buffer)) {
                return moduleEntry->DllBase;
            }
        }

        currentEntry = currentEntry->Flink;
    }

    return nullptr;

}

  

FARPROC GetProcAddressManually(void* dllBase) {

    unsigned char* base = (unsigned char*)dllBase;
    PIMAGE_DOS_HEADER dosHeader = (PIMAGE_DOS_HEADER)base;
    PIMAGE_NT_HEADERS ntHeaders = (PIMAGE_NT_HEADERS)(base + dosHeader->e_lfanew);
    IMAGE_DATA_DIRECTORY exportDir = ntHeaders->OptionalHeader.DataDirectory[IMAGE_DIRECTORY_ENTRY_EXPORT];
    PIMAGE_EXPORT_DIRECTORY exports = (PIMAGE_EXPORT_DIRECTORY)(base + exportDir.VirtualAddress);

  

    DWORD* names = (DWORD*)(base + exports->AddressOfNames);
    DWORD* functions = (DWORD*)(base + exports->AddressOfFunctions);
    WORD* ordinals = (WORD*)(base + exports->AddressOfNameOrdinals);

  

    for (DWORD i = 0; i < exports->NumberOfNames; i++) {

        char* funcName = (char*)(base + names[i]);
        if (CustomStrCmp(funcName, "GetProcAddress") == 0) {
            WORD ordinal = ordinals[i];
            return (FARPROC)(base + functions[ordinal]);
        }
    }

    return nullptr;
}

  

// --- NAKED EXECUTION ENTRYPOINT ---

extern "C" void raw_entry() {

    // 1. Locate Kernel32 Base address internally

    void* kernel32Base = GetKernel32Base();
    if (!kernel32Base) return;

  
    // 2. Resolve GetProcAddress manually via Export Table parsing
    fnGetProcAddress CustomGetProcAddress = (fnGetProcAddress)GetProcAddressManually(kernel32Base);
    if (!CustomGetProcAddress) return;
  
    // 3. Resolve required terminal standard I/O functions

    fnGetStdHandle CustomGetStdHandle = (fnGetStdHandle)CustomGetProcAddress((HMODULE)kernel32Base, "GetStdHandle");

    fnWriteFile CustomWriteFile = (fnWriteFile)CustomGetProcAddress((HMODULE)kernel32Base, "WriteFile");

  

    if (CustomGetStdHandle && CustomWriteFile) {

        // Grab the raw stdout channel handle identifier

        HANDLE hStdout = CustomGetStdHandle(STD_OUTPUT_HANDLE);

        if (hStdout != INVALID_HANDLE_VALUE) {

            const char* message = "Hello World from raw terminal execution!\n";

            DWORD bytesWritten = 0;

            // Output string directly to the active console terminal

            CustomWriteFile(hStdout, message, 41, &bytesWritten, NULL);

        }

    }

}
```


COMPILE WITH: `-nostdlib -mconsole -Wl,-e,raw_entry` FLAGS

This is a fascinating piece of code. It accomplishes the simplest task in programming—printing "Hello World"—using one of the most complex, low-level techniques available in Windows.

Normally, when you write a C++ program, the compiler and the Windows Operating System do a lot of hand-holding. They automatically find and load the system files (DLLs) your program needs to run. This code completely rejects that help. Instead, it manually digs into the hidden, internal memory structures of Windows to find what it needs. In cybersecurity and low-level systems programming, this is known as writing **Position Independent Code (PIC)** or a **Shellcode-like implementation**.

Here is a highly detailed, beginner-friendly breakdown formatted for your Obsidian vault.

**Introduction: The Core Concept**

Imagine a normal program as a guest arriving at a hotel. They go to the front desk (the Windows OS Loader), and the receptionist hands them a map, a room key, and a list of hotel services (Standard Libraries and system tools).

The provided code operates like a spy breaking through a window. It doesn't check in, it doesn't talk to the receptionist, and it brought its own lockpicks. Because it bypasses the normal Windows setup process, it must manually hunt down the tools it needs to print text to the screen.

The most critical tool it needs is **kernel32.dll**, the master toolbox in Windows that handles memory, files, and hardware. Once it finds `kernel32.dll`, it needs to find a specific function inside it called `GetProcAddress`. Once it has `GetProcAddress`, it can find literally any other function in the system.

**Part 1: The Custom Structures (The Cheat Sheets)**

_Refers to the `_CUSTOM_UNICODE_STRING`, `_CUSTOM_PEB`, etc. blocks_

Windows keeps a hidden ledger in memory for every single program that is currently running. This ledger contains everything about the program: where it lives in memory, what files it has open, and what system DLLs it is using. This ledger is called the **Process Environment Block (PEB)**.

Because this code is bypassing standard libraries, it doesn't have the official definitions for what the PEB looks like. So, the programmer had to manually define the shapes of these memory blocks.

- **CUSTOM_PEB:** The main ledger for the program.
    
- **CUSTOM_PEB_LDR_DATA (Loader Data):** A specific page in the ledger that keeps track of all the DLLs (toolboxes) loaded into the program.
    
- **CUSTOM_LDR_DATA_TABLE_ENTRY:** A single entry on that page. It acts like a linked list (a chain of sticky notes). Each note says, "Here is a loaded DLL, here is its name, and here is the string leading to the next note."
    

**Part 2: Custom String Utilities (Living Off the Land)**

_Refers to `CustomStrCmp` and `CustomWideStrContainsKernel32`_

Normally, programmers use tools like `strcmp()` to compare text. But those tools live in the C Standard Library (libc). If you use them, your code relies on the compiler to link them, which defeats the purpose of being fully independent.

- **CustomStrCmp:** Manually checks if two words are identical, letter by letter.
    
- **CustomWideStrContainsKernel32:** A highly specific, manual check to look for the word "kernel32". It is written to be case-insensitive (looking for 'k' or 'K') so it doesn't accidentally miss the file if Windows names it "KERNEL32.DLL".
    

**Part 3: Finding the Master Toolbox**

_Refers to `GetKernel32Base()`_

This is where the real magic happens. How does a program find `kernel32.dll` without asking Windows where it is?

1. **Reading the CPU's Mind:** It starts with `__readgsqword(0x60)`. In 64-bit Windows, the CPU has a special memory register called `GS`. Microsoft secretly guarantees that if you look exactly 0x60 bytes into the `GS` register, you will find the memory address of the Process Environment Block (PEB).
    
2. **Walking the Chain:** Once it has the PEB, the code looks at the Loader Data (`Ldr`) and finds the `InLoadOrderModuleList`. This is a circular chain of those "sticky notes" we mentioned earlier.
    
3. **The Hunt:** The code loops through every note in the chain. For every note, it looks at the name of the loaded file. It passes the name to `CustomWideStrContainsKernel32`.
    
4. **The Prize:** When it finds the note for `kernel32.dll`, it grabs the `DllBase`—the exact physical memory address where the `kernel32.dll` toolbox starts.
    

**Part 4: Reading the Index**

_Refers to `GetProcAddressManually()`_

Now the code knows exactly where `kernel32.dll` is located in memory. But `kernel32.dll` is massive; it contains thousands of functions. The code specifically needs `GetProcAddress`, a function that acts like a search engine for other functions.

To find it, the code has to parse the raw structure of the `.dll` file directly in memory. Windows programs are formatted in the **PE (Portable Executable)** format.

1. **Navigating the Headers:** It reads the `DOS_HEADER` (a legacy from the MS-DOS days) to find the `NT_HEADERS` (the modern Windows map).
    
2. **The Export Directory:** Inside the headers, it finds the "Export Directory." This is the table of contents for the DLL. It contains three lists:
    
    - A list of function names.
        
    - A list of function memory addresses.
        
    - A list of "ordinals" (index numbers connecting the names to the addresses).
        
3. **Matching:** The code loops through the list of function names. Using `CustomStrCmp`, it looks for the exact phrase "GetProcAddress".
    
4. **Resolution:** Once it finds the name, it uses the index numbers to find the exact memory address where the code for `GetProcAddress` actually lives, and returns it.
    

**Part 5: Execution and Output**

_Refers to `raw_entry()`_

This is the main function where everything comes together. Because this program is weird, it doesn't use standard names like `main()`. It uses `raw_entry()`, meaning it is the raw, naked starting line for the CPU.

1. **Step 1 & 2:** It runs the tools created above to find `kernel32.dll` and resolve `GetProcAddress`.
    
2. **Step 3:** It uses its newly acquired `GetProcAddress` tool to ask `kernel32.dll` for two more tools:
    
    - `GetStdHandle`: A tool to grab the command prompt window.
        
    - `WriteFile`: A tool to push raw bytes of data into a file (in Windows, the command prompt window is treated as a "file").
        
3. **The Output:** It gets the "handle" (the ID card) for the active terminal output (`STD_OUTPUT_HANDLE`). Finally, it uses `WriteFile` to push the 41 characters of "Hello World from raw terminal execution!\n" directly to the screen.
    

**Summary**

This script is a masterclass in Windows internals. It demonstrates how to manually traverse undocumented Windows memory structures (the PEB) and parse binary file formats (the PE Export Directory) dynamically in memory. While "Hello World" is benign, this exact methodology is the foundation of how low-level reverse engineering, anti-cheat engines, and sophisticated malware operate to stay hidden from traditional analysis.