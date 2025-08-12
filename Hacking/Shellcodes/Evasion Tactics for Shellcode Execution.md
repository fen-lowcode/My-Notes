##### **1. Polymorphic Encoding**

- **Description**: The shellcode is transformed using an encoder that changes its appearance while preserving its functionality. A decoder stub is prepended to decode it at runtime.
- **Examples**:
    - Shikata Ga Nai (Metasploit)
    - Alpha2
    - Countdown

---

##### **2. Metamorphic Code**

- **Description**: Unlike polymorphic code, metamorphic code rewrites its own logic and structure entirely, not just encrypting it. This makes detection even harder.
- **Use Case**: Advanced malware that wants to avoid signature-based detection.

---

##### **3. AES/RSA Encryption**

- **Description**: Strong cryptographic algorithms like AES or RSA can be used to encrypt the shellcode. A small decryption stub is included to decrypt it at runtime.
- **Pros**: Much harder to reverse-engineer than XOR.
- **Cons**: Larger stub size and more complex key management.

---

##### **4. Base64 or Custom Encoding**

- **Description**: Shellcode is encoded in Base64 or a custom scheme, then decoded at runtime.
- **Use Case**: Often used in scripts or macros to bypass simple filters.

---

##### **5. Packing and Compression**

- **Description**: Tools like UPX or custom packers compress and encrypt the shellcode, which is then unpacked in memory.
- **Pros**: Reduces size and obfuscates content.
- **Cons**: Can be detected by anti-virus if the packer is known.

---

##### **6. Instruction Substitution / Junk Insertion**

- **Description**: Replace instructions with equivalent ones or insert no-op (NOP) instructions to confuse disassemblers.
- **Example**: Replace `MOV EAX, 1` with `XOR EAX, EAX` followed by `INC EAX`.

---

##### **7. Return-Oriented Programming (ROP) Chains**

- **Description**: Instead of injecting shellcode, use existing code snippets (gadgets) in memory to perform malicious actions.
- **Use Case**: Bypasses non-executable memory protections like DEP.

---

##### **8. Dynamic Shellcode Generation**

- **Description**: Shellcode is generated or downloaded at runtime, avoiding static detection.
- **Example**: A stub that fetches encrypted payload from a remote server.