# 🧩 Section 3: Digital Signatures

Digital signatures are a cryptographic mechanism used to **verify the authenticity and integrity** of digital data.  
They ensure that:
- The message came from the claimed sender (**authentication**)  
- The message has not been tampered with (**integrity**)  

Digital signatures typically use **asymmetric cryptography**, e.g., **RSA** or **ECDSA**.

---

## 🐍 Python Example: RSA Sign/Verify

```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes

# Generate RSA key pair
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)
public_key = private_key.public_key()

# Message to sign
message = b"Sign me"

# Sign the message using RSA-PSS and SHA-256
signature = private_key.sign(
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

# Verify the signature
try:
    public_key.verify(
        signature,
        message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )
    print("✅ Signature is valid")
except Exception as e:
    print("❌ Signature verification failed:", e)
```
#### ✅ Notes
   - rsa.generate_private_key() generates a new RSA key pair.
   - PSS padding with MGF1 and SHA-256 is a secure signature scheme.
   - public_key.verify() will raise an exception if the signature is invalid.
   - This ensures both authenticity and integrity of the message.

#### 💻 OpenSSL Example: Sign/Verify
   1. Sign a file using SHA-256 and a private key:
```bash
openssl dgst -sha256 -sign private.pem -out sign.bin file.txt
```
  2. Verify the signature using the public key:
```bash
openssl dgst -sha256 -verify public.pem -signature sign.bin file.txt
```
#### ✅ Notes
   - dgst means digest; it computes a hash of the file before signing.
   - private.pem is your private RSA key.
   - public.pem is the corresponding public key.
   - sign.bin contains the digital signature.
   - OpenSSL automatically hashes the file (SHA-256 in this case) before signing/verifying.

#### 🧠 Key Points
| Topic                | Description                                                          |
| -------------------- | -------------------------------------------------------------------- |
| Purpose              | Ensure **authenticity** and **integrity** of messages                |
| Algorithm Examples   | RSA, ECDSA                                                           |
| Hash Function        | SHA-256 (common)                                                     |
| Padding Scheme (RSA) | PSS (Probabilistic Signature Scheme)                                 |
| Verification Outcome | Signature valid → message trusted; invalid → message may be tampered |

### 📘 References

- [Python cryptography documentation](https://cryptography.io/en/latest/)
- [OpenSSL dgst command reference](https://www.openssl.org/docs/manmaster/man1/openssl-dgst.html)
- [NIST Digital Signature Standard (DSS)](https://csrc.nist.gov/publications/detail/fips/186/4/final)


#### ✅ Covered in This Section

- Overview of digital signatures
- Python RSA signing & verification
- OpenSSL file signing & verification
- Key points and references