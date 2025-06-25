# Custom-File-Encryption-Decryption-Tool


Project Overview
This project is a standalone Python-based tool designed to provide robust file encryption and decryption capabilities. Its primary goal is to secure sensitive files using industry-standard cryptographic algorithms, ensuring data confidentiality and integrity. The tool empowers users to encrypt and decrypt files using a passphrase, demonstrating a practical application of modern cryptography for data protection.


Key Features
Symmetric Encryption (AES-256): Implements the Advanced Encryption Standard (AES) with a 256-bit key in CBC (Cipher Block Chaining) mode. This provides strong, efficient, and widely recognized encryption for bulk data.
Secure Key Derivation: Utilizes PBKDF2 (Password-Based Key Derivation Function 2) with SHA256 and a high iteration count (e.g., 100,000) to derive a strong cryptographic key from a user-provided passphrase. This makes brute-forcing passwords significantly harder.
Random Salt Generation: Employs a unique, cryptographically strong random salt for each encryption operation. The salt is stored alongside the ciphertext, ensuring that identical passwords encrypt to different ciphertexts, and protecting against rainbow table attacks.
Initialization Vector (IV) Management: Generates a unique, random Initialization Vector (IV) for each encryption. The IV is crucial for CBC mode to prevent identical plaintext blocks from producing identical ciphertext blocks, enhancing security.
Padding (PKCS7): Implements PKCS7 padding to ensure plaintext data always aligns with the block size of the AES algorithm, which is necessary for correct encryption and decryption.
File Stream Processing: Designed to encrypt and decrypt files chunk by chunk, making it suitable for large files without loading the entire content into memory.
Robust Error Handling: Includes comprehensive error handling for file operations (e.g., FileNotFoundError), incorrect passwords (via padding validation errors), and general cryptographic exceptions.
User-Defined Passphrases: Allows users to define their own strong passwords for encryption, providing personalized control over data security.


Technologies Used
Core Language: Python 3.x
Cryptography Library: cryptography
Chosen for its focus on modern, secure cryptographic primitives and its adherence to best practices. It provides a higher-level API compared to raw os.urandom and bitwise operations, reducing implementation errors.
Specifically, cryptography.hazmat.primitives.ciphers for AES and CBC mode.
cryptography.hazmat.primitives.kdf.pbkdf2 for PBKDF2HMAC key derivation.
cryptography.hazmat.primitives.padding for PKCS7 padding.
Randomness: Python's built-in secrets module for generating cryptographically strong random numbers for salts and IVs.
File I/O: Standard Python file handling for reading input files and writing output files in binary mode.
Hash Functions: hashlib for general hashing if needed, though cryptography's primitives often include hashing as part of KDF.


Architectural Highlights & Design Choices
Command-Line Interface (CLI): Provides a simple, interactive command-line interface for ease of use, guiding the user through encryption/decryption steps.
Symmetric Encryption Focus: Primarily focuses on AES for file encryption due to its speed and suitability for encrypting large amounts of data. RSA (asymmetric) is typically used for key exchange or digital signatures, not direct file encryption due to its performance characteristics.
Component Separation: Logic is modularized into distinct functions for key derivation, encryption, decryption, and the main CLI interaction, promoting readability and maintainability.
Security by Design:
No Hardcoded Keys: Passwords are user-provided and ephemeral for key derivation.
Unique Salt and IV per Encryption: Ensures that even identical plaintexts encrypted with the same password result in different ciphertexts, increasing resistance to various attacks.
Padding Oracle Attack Mitigation: Using authenticated modes like AES-GCM (though not implemented in the provided solution for simplicity, it's a key future enhancement) would offer protection against padding oracle attacks in addition to CBC mode's inherent properties.
Memory Efficiency: Processing files in chunks prevents memory overflow when dealing with very large files.


Challenges and Solutions
Challenge: Secure Key Management: Deriving a strong, consistent key from a user's potentially weak passphrase.
Solution: Employed PBKDF2HMAC, which intentionally adds computational cost (iterations) to the key derivation process. This makes brute-forcing the password, even if the hash is known, computationally infeasible.
Challenge: Maintaining Integrity and Authenticity: Ensuring that the encrypted file has not been tampered with and that it originates from the expected source.
Solution (Current): AES-CBC provides confidentiality but not inherent integrity or authenticity. Padding validation during decryption helps detect some tampering that corrupts the padding.
Future Solution: Implementing an Authenticated Encryption with Associated Data (AEAD) mode like AES-GCM would inherently provide both confidentiality and integrity/authenticity.
Challenge: Handling File Size and Padding: Ensuring that files of any size can be encrypted correctly and that decryption can reconstruct the original data precisely.
Solution: PKCS7 padding ensures that the final block of plaintext is correctly sized for encryption. Chunk-based reading and writing, along with careful handling of the final (possibly partial) block and padding application/removal, addresses arbitrary file sizes.


Future Enhancements
Authenticated Encryption (AES-GCM): Upgrade the encryption mode to AES-GCM for integrated confidentiality, integrity, and authenticity, providing stronger protection against tampering.
Asymmetric Encryption (RSA) for Key Exchange: For more advanced scenarios, implement RSA to encrypt the AES key, allowing secure key exchange when sharing files with specific recipients without sharing the passphrase directly.
Metadata Storage: Encrypt and store file metadata (original filename, size, checksum) within the encrypted file's header.
Graphical User Interface (GUI): Develop a user-friendly GUI using libraries like Tkinter or PyQt for easier interaction.
Directory Encryption: Extend functionality to encrypt entire directories, maintaining the directory structure.
Performance Optimization: For very large files, explore techniques like multi-threading for I/O operations (though Python's GIL can limit CPU-bound crypto operations).
Cross-Platform Compatibility: Ensure the tool runs seamlessly across different operating systems.
