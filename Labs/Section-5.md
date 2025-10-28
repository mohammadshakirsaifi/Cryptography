# 🧩 Section 5: KMS Platforms

**Key Management Services (KMS)** are cloud or enterprise platforms for **creating, managing, and securing cryptographic keys**.  
They allow encryption/decryption, key rotation, lifecycle management, and auditing without exposing raw key material.

This section covers basic operations using Key Management Services (KMS) across AWS, Azure, GCP, and Oracle/Thales Common KMS platforms:
- **AWS KMS** (Amazon Web Services)  
- **Oracle Cloud KMS**  
- **GCP KMS** (Google Cloud Platform)  
- **Azure Key Vault**  
- **Thales CipherTrust / HSMs**  

---

## 💻 AWS KMS (CLI)

### 1️⃣ Create a Key:

```bash
aws kms create-key --description "Demo Key"
```

### 2️⃣Encrypt a file:
```bash
aws kms encrypt --key-id <KeyId> --plaintext fileb://plain.txt --output text --query CiphertextBlob | base64 --decode > encrypted.bin
```

### 3️⃣Decrypt a file:
```bash
aws kms decrypt --ciphertext-blob fileb://encrypted.bin --output text --query Plaintext | base64 --decode
```

### 4️⃣Enable key rotation:
```bash
aws kms enable-key-rotation --key-id <KeyId>
```

### 5️⃣Disable a key:
```bash
aws kms disable-key --key-id <KeyId>
```

### 6️⃣Schedule key deletion:
```bash
aws kms schedule-key-deletion --key-id <KeyId> --pending-window-in-days 7
```

## 💻 Azure KMS CLI

### 1️⃣ Create a Key:

```bash
az keyvault key create --vault-name <VaultName> --name <KeyName> --protection software
```

### 2️⃣Encrypt a file:
```bash
az keyvault key encrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value "$(cat plain.txt | base64)"
```

### 3️⃣Decrypt a file:
```bash
az keyvault key decrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value <EncryptedValue>
```

### 4️⃣Enable key rotation (preview feature):
```bash
az keyvault key rotation-policy update --vault-name <VaultName> --name <KeyName> --expiry-time "P90D"
```

### 5️⃣Backup key:
```bash
az keyvault key backup --vault-name <VaultName> --name <KeyName> --file <backup-file>
```

## 💻GCP KMS CLI

### 1️⃣ Create a key ring:

```bash
gcloud kms keyrings create <KeyRingName> --location <Location>
```

### 2️⃣Create a key:
```bash
gcloud kms keys create <KeyName> --location <Location> --keyring <KeyRingName> --purpose encryption
```

### 3️⃣Encrypt a file:
```bash
gcloud kms encrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --plaintext-file plain.txt --ciphertext-file encrypted.bin
```

### 4️⃣Decrypt a file:
```bash
gcloud kms decrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt
```

### 5️⃣Rotate a key:
```bash
gcloud kms keys versions create --location <Location> --keyring <KeyRingName> --key <KeyName>
```

## 💻Oracle KMS CLI (Thales / Oracle Cloud)

#### 1️⃣ Create a key:
```bash
oci kms key create --compartment-id <CompartmentOCID> --display-name <KeyName>
```

### 2️⃣Encrypt a file:
```bash
oci kms crypto encrypt --key-id <KeyId> --plaintext fileb://plain.txt --ciphertext-file encrypted.bin
```

### 3️⃣Decrypt a file:
```bash
oci kms crypto decrypt --key-id <KeyId> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt
```

### 4️⃣Rotate a key:
```bash
oci kms key rotate --key-id <KeyId>
```

### 5️⃣Backup key:
```bash
oci kms key export --key-id <KeyId> --file <backup-file>
```

### Note: Replace placeholders like <KeyId>, <KeyName>, <VaultName>, <Location>, and <CompartmentOCID> with your actual values.

#### ✅ This version:

- Adds **Azure, GCP, and Oracle CLI commands** that were missing.  
- Corrects formatting to **Markdown code blocks**.  
- Standardizes terminology: encrypt/decrypt, rotate, backup.  
- Adds notes about placeholders for clarity.  

## KMS Platforms Command Comparison

| Operation | 1️⃣ AWS CLI | 2️⃣ Azure CLI | 3️⃣ GCP CLI | 4️⃣ Oracle/Thales CLI |
|-----------|---------|-----------|---------|------------------|
| **Create Key** | `aws kms create-key --description "Demo Key"` | `az keyvault key create --vault-name <VaultName> --name <KeyName> --protection software` | `gcloud kms keys create <KeyName> --location <Location> --keyring <KeyRingName> --purpose encryption` | `oci kms key create --compartment-id <CompartmentOCID> --display-name <KeyName>` |
| **Encrypt** | `aws kms encrypt --key-id <KeyId> --plaintext fileb://plain.txt --output text --query CiphertextBlob | base64 --decode > encrypted.bin` | `az keyvault key encrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value "$(cat plain.txt | base64)"` | `gcloud kms encrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --plaintext-file plain.txt --ciphertext-file encrypted.bin` | `oci kms crypto encrypt --key-id <KeyId> --plaintext fileb://plain.txt --ciphertext-file encrypted.bin` |
| **Decrypt** | `aws kms decrypt --ciphertext-blob fileb://encrypted.bin --output text --query Plaintext | base64 --decode` | `az keyvault key decrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value <EncryptedValue>` | `gcloud kms decrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt` | `oci kms crypto decrypt --key-id <KeyId> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt` |
| **Rotate Key** | `aws kms enable-key-rotation --key-id <KeyId>` | `az keyvault key rotation-policy update --vault-name <VaultName> --name <KeyName> --expiry-time "P90D"` | `gcloud kms keys versions create --location <Location> --keyring <KeyRingName> --key <KeyName>` | `oci kms key rotate --key-id <KeyId>` |
| **Backup Key** | `aws kms schedule-key-deletion --key-id <KeyId> --pending-window-in-days 7` (No direct backup, can export manually if using CloudHSM) | `az keyvault key backup --vault-name <VaultName> --name <KeyName> --file <backup-file>` | `gcloud kms keys versions backup/export is limited; consider key version export` | `oci kms key export --key-id <KeyId> --file <backup-file>` |

## KMS Platforms Quick Reference

| #️⃣ | Operation | 1️⃣ AWS CLI | 2️⃣ Azure CLI | 3️⃣ GCP CLI | 4️⃣ Oracle/Thales CLI |
|----|-----------|------------|---------------|------------|----------------------|
| 1️⃣ | Create Key | `aws kms create-key --description "Demo Key"` | `az keyvault key create --vault-name <VaultName> --name <KeyName> --protection software` | `gcloud kms keys create <KeyName> --location <Location> --keyring <KeyRingName> --purpose encryption` | `oci kms key create --compartment-id <CompartmentOCID> --display-name <KeyName>` |
| 2️⃣ | Encrypt File | `aws kms encrypt --key-id <KeyId> --plaintext fileb://plain.txt --output text --query CiphertextBlob | base64 --decode > encrypted.bin` | `az keyvault key encrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value "$(cat plain.txt | base64)"` | `gcloud kms encrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --plaintext-file plain.txt --ciphertext-file encrypted.bin` | `oci kms crypto encrypt --key-id <KeyId> --plaintext fileb://plain.txt --ciphertext-file encrypted.bin` |
| 3️⃣ | Decrypt File | `aws kms decrypt --ciphertext-blob fileb://encrypted.bin --output text --query Plaintext | base64 --decode` | `az keyvault key decrypt --vault-name <VaultName> --name <KeyName> --algorithm RSA-OAEP --value <EncryptedValue>` | `gcloud kms decrypt --location <Location> --keyring <KeyRingName> --key <KeyName> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt` | `oci kms crypto decrypt --key-id <KeyId> --ciphertext-file encrypted.bin --plaintext-file decrypted.txt` |
| 4️⃣ | Enable Key Rotation | `aws kms enable-key-rotation --key-id <KeyId>` | `az keyvault key rotation-policy update --vault-name <VaultName> --name <KeyName> --expiry-time "P90D"` | `gcloud kms keys versions create --location <Location> --keyring <KeyRingName> --key <KeyName>` | `oci kms key rotate --key-id <KeyId>` |
| 5️⃣ | Disable Key | `aws kms disable-key --key-id <KeyId>` | N/A | N/A | N/A |
| 6️⃣ | Schedule Key Deletion | `aws kms schedule-key-deletion --key-id <KeyId> --pending-window-in-days 7` | N/A | N/A | N/A |

## References

### AWS KMS
- [AWS Key Management Service (KMS) Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
- [AWS CLI KMS Commands](https://docs.aws.amazon.com/cli/latest/reference/kms/index.html)

### Azure Key Vault
- [Azure Key Vault Documentation](https://learn.microsoft.com/en-us/azure/key-vault/)
- [Azure CLI Key Vault Commands](https://learn.microsoft.com/en-us/cli/azure/keyvault?view=azure-cli-latest)

### Google Cloud KMS
- [Google Cloud Key Management Service Documentation](https://cloud.google.com/kms/docs)
- [gcloud KMS Commands](https://cloud.google.com/kms/docs/reference/tools)

### Oracle Cloud KMS (Thales)
- [Oracle Cloud Key Management Documentation](https://docs.oracle.com/en-us/iaas/Content/KMS/Concepts/kmsoverview.htm)
- [OCI CLI Key Management Commands](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cliconcepts.htm#kms)



