# 🔒 AES, RSA, and ECC Encryption Examples (Python & OpenSSL)

This project demonstrates how to perform **AES**, **RSA**, and **ECC** encryption and decryption using both **Python** and **OpenSSL** command-line tools.

---

## 🧠 Overview

- **AES (Advanced Encryption Standard)** — Symmetric encryption for fast and secure data protection.  
- **RSA (Rivest–Shamir–Adleman)** — Asymmetric encryption widely used for key exchange and digital signatures.  
- **ECC (Elliptic Curve Cryptography)** — A modern form of public-key cryptography offering high security with smaller key sizes.

---

## ⚙️ How It Works

### 🧩 AES (Symmetric Encryption)
AES uses a **single secret key** for both encryption and decryption.

1. A **256-bit random key** and **128-bit IV (Initialization Vector)** are generated.  
2. The plaintext is divided into 16-byte blocks (AES block size).  
3. Each block is encrypted using **AES-256 in CBC mode**.  
4. The same key and IV are used to decrypt the ciphertext back into plaintext.

**Key points:**
- AES is **fast** and suitable for encrypting large amounts of data.  
- Both the sender and receiver must share the same secret key securely.

---

### 🔑 RSA (Asymmetric Encryption)
RSA uses a **pair of keys**: a **public key** (for encryption) and a **private key** (for decryption).

1. A **2048-bit RSA key pair** is generated.  
2. The **public key** encrypts data (or a symmetric key).  
3. Only the **private key** can decrypt it.  
4. This makes RSA ideal for secure key exchange or digital signatures.

**Key points:**
- RSA ensures **confidentiality** without needing to share secret keys in advance.  
- It’s slower than AES, so it's often used to encrypt **keys**, not large files.

---

### 🧮 ECC (Elliptic Curve Cryptography)
ECC is another form of **asymmetric encryption** that provides the same level of security as RSA but with much **smaller keys**.

1. An ECC private key is generated based on a mathematical curve (e.g., `secp256r1`).  
2. The public key is derived from the private key.  
3. These keys can be used for encryption, digital signatures, or key exchange (ECDH).

**Key points:**
- ECC is **lightweight and efficient**, ideal for mobile or embedded systems.  
- It’s the basis for modern cryptographic protocols like **TLS 1.3** and **Bitcoin**.

---

## 🐍 AES Encryption and Decryption (Python Example)

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
import os

# Generate a random 256-bit key and 128-bit IV
key = os.urandom(32)
iv = os.urandom(16)

# Plaintext must be a multiple of 16 bytes for AES-CBC
plaintext = b'Hello, AES World!!'  # 16 bytes exactly

# Create AES cipher in CBC mode
cipher = Cipher(algorithms.AES(key), modes.CBC(iv))

# Encrypt
encryptor = cipher.encryptor()
ciphertext = encryptor.update(plaintext) + encryptor.finalize()

# Decrypt
decryptor = cipher.decryptor()
decrypted = decryptor.update(ciphertext) + decryptor.finalize()

print("Ciphertext:", ciphertext)
print("Decrypted:", decrypted)
```
#### 💡 Note:
- AES-CBC mode requires the plaintext length to be a multiple of 16 bytes.
- For arbitrary-length messages, use PKCS7 padding.
```bash
# Encrypt a plaintext file
openssl enc -aes-256-cbc -in plaintext.txt -out encrypted.txt -k MyPass123

# Decrypt the encrypted file
openssl enc -aes-256-cbc -d -in encrypted.txt -out decrypted.txt -k MyPass123
```
#### 🔧 AES Encryption and Decryption (OpenSSL CLI)
   - You can also perform AES encryption and decryption directly in the terminal using OpenSSL:
```bash
# Encrypt a plaintext file
openssl enc -aes-256-cbc -in plaintext.txt -out encrypted.txt -k MyPass123

# Decrypt the encrypted file
openssl enc -aes-256-cbc -d -in encrypted.txt -out decrypted.txt -k MyPass123
```
#### 🔑 RSA Encryption and Decryption (OpenSSL)
   - RSA is an asymmetric algorithm using public and private key pairs.
```bash
# Generate a 2048-bit RSA private key
openssl genpkey -algorithm RSA -out private.pem -pkeyopt rsa_keygen_bits:2048

# Extract the public key
openssl rsa -in private.pem -pubout -out public.pem

# Create a plaintext file
echo "Hello, RSA!" > file.txt

# Encrypt the file using the public key
openssl rsautl -encrypt -inkey public.pem -pubin -in file.txt -out file_enc.bin

# Decrypt the file using the private key
openssl rsautl -decrypt -inkey private.pem -in file_enc.bin -out file_dec.txt
```
#### 🧮 ECC (Elliptic Curve Cryptography) Key Generation (OpenSSL)
   - ECC provides the same security level as RSA with smaller key sizes, making it faster and more efficient.

```bash
# Generate an ECC private key (using the secp256r1 curve)
openssl ecparam -genkey -name secp256r1 -out ecc_private.pem

# Extract the corresponding public key
openssl ec -in ecc_private.pem -pubout -out ecc_public.pem

```
#### ⚙️ Requirements

   - Python 3.8+
   - OpenSSL
   - cryptography library
   - Install the Python dependency:
```bash
pip install cryptography
```