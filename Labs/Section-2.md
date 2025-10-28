# 🧩 Section 2: Hashing (SHA Algorithms)

## 🔹 What is SHA-256?

**SHA-256 (Secure Hash Algorithm 256-bit)** is part of the **SHA-2 family** of cryptographic hash functions.  
It takes an input of any size and produces a fixed-length **256-bit (32-byte)** hash.

SHA-256 is commonly used for:
- ✅ Data integrity verification  
- ✅ Password hashing (with salt)  
- ✅ Digital signatures and certificates  
- ✅ Blockchain transactions (e.g., Bitcoin)

---

## 🐍 Python Example

```python
import hashlib

data = b"My Data"  # Input must be in bytes
hash_object = hashlib.sha256(data)
print("SHA256:", hash_object.hexdigest())
```
#### 🧾 Output Example
```txt
SHA256: 9e5c0f5e5b3e26a7e99ceab9b7df23d4a3d3e15a7868b4d60280bb98c17cda84
```
#### ✅ Notes

  - b"My Data" indicates a byte string — required by hashlib.
  - .hexdigest() converts the hash into a readable hexadecimal format.
  - The same input will always produce the same hash output.
  - Even a single character change in the input results in a completely different hash — this is called the avalanche effect.
#### 📂 Hashing File Contents
  - You can compute the SHA-256 hash of a file (e.g., demo.txt) like this:
```python
import hashlib

with open("demo.txt", "rb") as f:
    print("SHA256:", hashlib.sha256(f.read()).hexdigest())
```
#### Example Output
```txt
SHA256: 9e5c0f5e5b3e26a7e99ceab9b7df23d4a3d3e15a7868b4d60280bb98c17cda84
```
#### 💻 Using OpenSSL (Command Line)
  - You can compute the same SHA-256 hash directly from the terminal using OpenSSL:
```bash
openssl dgst -sha256 demo.txt
```
#### Output Example
```bash
SHA256(demo.txt)= 9e5c0f5e5b3e26a7e99ceab9b7df23d4a3d3e15a7868b4d60280bb98c17cda84
```
#### ✅ Additional Notes
  - dgst means digest — the hashing function command in OpenSSL.
  - You can replace the algorithm name for different hashes:

| Command Option                  | Algorithm      | Status      | Notes                    |
| ------------------------------- | -------------- | ----------- | ------------------------ |
| `-sha1`                         | SHA-1          | ❌ Broken    | Obsolete — avoid using   |
| `-sha224`, `-sha384`, `-sha512` | SHA-2 Variants | ✅ Secure    | Different output lengths |
| `-md5`                          | MD5            | ⚠️ Insecure | Legacy only              |

#### 🧠 Summary
| Algorithm | Output Size | Security Level | Common Uses                |
| --------- | ----------- | -------------- | -------------------------- |
| SHA-1     | 160 bits    | ❌ Broken       | Legacy systems             |
| SHA-256   | 256 bits    | ✅ Strong       | File integrity, blockchain |
| SHA-384   | 384 bits    | ✅ Stronger     | TLS certificates           |
| SHA-512   | 512 bits    | ✅ Strongest    | High-security applications |

#### 🛡️ Bonus Tip: Adding Salt for Passwords
   - When hashing passwords, always add a random salt to prevent rainbow table attacks:
```python
import hashlib, os

password = b"MySecretPassword"
salt = os.urandom(16)  # Generate a 16-byte random salt
hash_object = hashlib.sha256(salt + password)
print("Salt:", salt.hex())
print("SHA256 (with salt):", hash_object.hexdigest())
```
#### 🧪 Extra: SHA-3 (Keccak)
   - SHA-3 is the newest member of the Secure Hash Algorithm family, standardized in 2015.
   - It uses the Keccak algorithm and is not a replacement for SHA-2 but an alternative.
#### Python Example:

```python
import hashlib

data = b"My Data"
hash_object = hashlib.sha3_256(data)
print("SHA3-256:", hash_object.hexdigest())
```
#### Key Differences:

   - SHA-3 uses a different internal design (sponge construction).
   - Resistant to certain attacks that could theoretically affect SHA-2.
   - Slightly slower, but more flexible (can output variable-length hashes).

#### 📘 References

   - NIST SHA-2 Standard (FIPS PUB 180-4)
   - NIST SHA-3 Standard (FIPS PUB 202)
   - Python hashlib Documentation
   - OpenSSL dgst Command Reference


---

This file now contains:  
- SHA-256 overview, Python examples, and OpenSSL usage  
- File hashing  
- Salted password hashing  
- SHA-3 (Keccak) overview  
- Summary table and references  

