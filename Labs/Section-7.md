# Section 7: TLS/SSL, X.509, SSH, and S/MIME

This section covers essential cryptographic tools and protocols for securing communications and managing certificates.

---

## 🔒 TLS/SSL (OpenSSL)

**Purpose:**  
TLS (Transport Layer Security) and SSL (Secure Sockets Layer) are cryptographic protocols used to secure communications over a network.  
OpenSSL is a command-line toolkit for generating, managing, and inspecting certificates and keys.

### Generate a Self-Signed Certificate

```bash
# Generate a new RSA private key (2048 bits) and a self-signed X.509 certificate valid for 365 days
openssl req -x509 -newkey rsa:2048 -days 365 -nodes -keyout tls.key -out tls.crt -subj "/CN=localhost"

- -x509 → Create a self-signed X.509 certificate.
- -newkey rsa:2048 → Generate a new RSA key of 2048 bits.
- -nodes → Do not encrypt the private key.
- -subj → Provide subject information non-interactively.

### View Certificate Details
```bash
openssl x509 -in tls.crt -text -noout
```
## 🔑 SSH (Secure Shell)

#### Purpose:
SSH is used for secure remote login and command execution. Public key authentication improves security by replacing password-based logins.

#### Generate an SSH Key Pair
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- -t rsa → Specify the key type (RSA).
- -b 4096 → Set key length (4096 bits).
- -C → Add a comment (often your email address).

#### Copy Public Key to Remote Host
```bash
ssh-copy-id user@host
```

This installs your public key on the remote host for passwordless SSH authentication.

### ✉️ S/MIME (Secure/Multipurpose Internet Mail Extensions)

#### Purpose:
S/MIME provides cryptographic security services for email — allowing users to sign and encrypt messages using X.509 certificates.

**Steps:**

**1. Obtain a Certificate**

- Get a personal S/MIME certificate from a trusted Certificate Authority (CA) (e.g., Comodo, DigiCert, or GlobalSign).

**2. Import Certificate to Email Client**

- **For Outlook:**
   - Open File → Options → Trust Center → Trust Center Settings → Email Security
   - Import the certificate under Digital IDs (Certificates).

**3. Sign and Encrypt Email**

 - When composing a message, choose Options → Sign or Encrypt to apply S/MIME protection.

## 🐙 Bitbucket SSH Setup

SSH allows secure, passwordless communication with Bitbucket repositories.

#### 1. Generate an SSH Key
##### Linux / macOS
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

#### Windows (Git Bash / WSL)

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
```txt
Default location is ~/.ssh/id_rsa (Linux/macOS) or /c/Users/YourUsername/.ssh/id_rsa (Windows). You can set a passphrase optionally.
```

### 2. Add SSH Key to SSH Agent
#### Linux / macOS
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

#### Windows (Git Bash)
```bash
eval "$(ssh-agent -s)"
ssh-add /c/Users/YourUsername/.ssh/id_rsa
```

#### 3. Copy the Public Key
```bash
cat ~/.ssh/id_rsa.pub   # Linux/macOS
cat /c/Users/YourUsername/.ssh/id_rsa.pub  # Windows
```

Copy the output to your clipboard.

### 4. Add SSH Key to Bitbucket

#### 1. Log in to Bitbucket.
#### 2. Go to Personal Settings → SSH Keys → Add Key.
#### 3. Paste the public key and give it a label.
#### 4. Save.

### 5. Test the SSH Connection
```bash
ssh -T git@bitbucket.org
```

#### Expected response:
```txt
authenticated via ssh key.
You can use git or hg to connect to Bitbucket. Shell access is disabled.
```
#### 6. Configure Git to Use SSH
```bash
# Check remote URL
git remote -v

# Switch remote from HTTPS to SSH
git remote set-url origin git@bitbucket.org:username/repo.git
```

📘 References

- [OpenSSL Documentation](https://www.openssl.org/docs/)
- [SSH Manual (Linux)](https://man.openbsd.org/ssh)
- [Microsoft: S/MIME in Outlook](https://support.microsoft.com/en-us/office/what-is-s-mime-and-how-do-i-turn-it-on-26d83655-4d1f-4473-a9b3-0e08a1e45a72)
- [Bitbucket: Set up an SSH key](https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/)
