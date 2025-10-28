# 🧩 Section 4: PKI Tools

Public Key Infrastructure (PKI) tools are used to **create, manage, and verify digital certificates**, which enable secure communication, authentication, and encryption in networks.  

Common PKI tools include:
- **OpenSSL** (command-line certificate management)  
- **Microsoft ADCS** (Active Directory Certificate Services)  
- **HashiCorp Vault** (secret and certificate management)  
- **EJBCA** (Enterprise Java PKI CA)  
- **Venafi** (enterprise certificate management)

---

## 💻 OpenSSL CA Workflow

OpenSSL can be used to **simulate a Certificate Authority (CA)** and issue certificates for users or servers.

### Step 1: Create a Root CA

```bash
# Generate CA private key
openssl genrsa -out ca.key 2048

# Create a self-signed root certificate
openssl req -x509 -new -key ca.key -out ca.crt -days 3650 -subj "/CN=MyDemoCA"
```
### Step 2: Create a User/Server Certificate

```bash
# Generate user private key
openssl genrsa -out user.key 2048

# Create a certificate signing request (CSR)
openssl req -new -key user.key -out user.csr -subj "/CN=EndUser"

# Sign the CSR with the CA to issue a certificate
openssl x509 -req -in user.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out user.crt -days 365
```
### Step 3: Verify the Certificate
 
```bash
openssl verify -CAfile ca.crt user.crt
```
### ✅ Notes

- ca.key → private key of the CA (keep secure)
- ca.crt → self-signed CA certificate
- user.key → private key for the user or server
- user.csr → certificate signing request
- user.crt → signed certificate issued by CA
- -CAcreateserial → creates a serial number file for CA

### 🏢 ADCS (Active Directory Certificate Services)

- ADCS provides a GUI and wizard-based interface for enterprise certificate management.
- Use the ADCS wizards to create and issue certificates for domain-joined machines or users.
- Ideal for Windows enterprise environments.

### 🗄️ HashiCorp Vault: PKI Secrets Engine

- HashiCorp Vault can manage PKI and issue dynamic certificates.

#### Enable PKI Secrets Engine

```bash
vault secrets enable pki
```
#### Generate a Root CA
```bash
vault write pki/root/generate/internal common_name="demo.com" ttl=8760h
```
#### Issue a Certificate
```bash
vault write pki/issue/demo-dot-com common_name="host.demo.com" ttl=24h
```
#### ✅ Notes

- Vault allows programmatic certificate issuance, ideal for microservices or ephemeral workloads.
- ttl defines certificate lifetime.
- Supports role-based issuance, revocation, and renewal.

#### 🧠 Key Points
| Tool            | Purpose                                         | Notes                                |
| --------------- | ----------------------------------------------- | ------------------------------------ |
| OpenSSL         | CLI certificate management                      | Good for labs, testing, and learning |
| ADCS            | Enterprise CA management                        | GUI-based, Windows-centric           |
| HashiCorp Vault | Dynamic certificate issuance and PKI management | API-driven, ideal for automation     |
| EJBCA           | Enterprise Java CA                              | Full-featured open-source CA         |
| Venafi          | Enterprise certificate management platform      | Commercial, policy enforcement       |

#### 📘 References

- [OpenSSL Documentation](https://www.openssl.org/docs/)
- [Microsoft ADCS Documentation](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/ad-cs/active-directory-certificate-services-overview)
- [HashiCorp Vault PKI Secrets Engine](https://developer.hashicorp.com/vault/docs/secrets/pki)
- [EJBCA Documentation](https://www.ejbca.org/docs/)
- [Venafi Product Overview](https://www.venafi.com/

#### ✅ This file now contains:  
- OpenSSL CA workflow
- ADCS setup notes
- HashiCorp Vault PKI usage
- Key points and references