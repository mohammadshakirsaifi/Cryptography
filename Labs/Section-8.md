# Section 8: Scripting for Automation

This section covers how to automate certificate generation and management using **Python**, **PowerShell**, and **Bash**.

---
## Certificate Automation Workflow

```mermaid
graph TD
    A[Python Script] --> B[Generate RSA Key]
    B --> C[Create CSR]
    C --> D[Send to CA]
    D --> E[Receive Signed Certificate]
```
### Workflow Explanation:

**1.Python Script** – Runs the automation commands.
**2. Generate RSA Key** – Produces user.key using OpenSSL.
**3. Create CSR** – Generates user.csr from the private key.
**4. Send to CA** – Submit user.csr to your Certificate Authority (ca.crt and ca.key) for signing.
**5. Receive Signed Certificate** – Receive user.crt from the CA, completing the automation workflow.
```txt
💡 Tip: Filenames like user.key, user.csr, ca.crt, and user.crt are referenced in the explanation, but avoid using parentheses or special characters in Mermaid nodes to ensure GitHub renders the diagram correctly.
```

## Python Automation

You can use Python's `subprocess` module to automate OpenSSL commands.

```python
import subprocess

# Generate a 2048-bit RSA private key
subprocess.run(['openssl', 'genrsa', '-out', 'user.key', '2048'])

# Generate a Certificate Signing Request (CSR) using the private key
subprocess.run([
    'openssl', 'req', '-new', '-key', 'user.key', 
    '-out', 'user.csr', '-subj', '/CN=EndUser'
])

# Generate a self-signed certificate or sign with a CA
subprocess.run([
    'openssl', 'x509', '-req', '-in', 'user.csr', 
    '-CA', 'ca.crt', '-CAkey', 'ca.key', '-CAcreateserial', 
    '-out', 'user.crt', '-days', '365'
])
```
```txt
Note: Replace ca.crt and ca.key with your Certificate Authority files if signing with a CA.
```
### PowerShell Automation
PowerShell can be used to list certificates and export them:
```txt
# List certificates in the current user's personal store
Get-ChildItem -Path Cert:\CurrentUser\My

# Export a specific certificate by its thumbprint
$thumb = "<Enter_Thumbprint_Here>"
Export-Certificate -Cert (Get-ChildItem Cert:\CurrentUser\My\$thumb) -FilePath C:\user.crt

# Tip: Replace <Enter_Thumbprint_Here> with the actual certificate thumbprint.
```
### Bash Bulk Operations

For bulk processing of .pem files to extract certificate subjects:
```bash
for file in *.pem
do
  echo "$file:"
  openssl x509 -in "$file" -noout -subject
done

# Explanation: This loops over all .pem files in the current directory and prints their subject information.
```

### Command Reference Table
| Tool       | Command                                                                                                                                                      | Description                                                       |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Python     | `subprocess.run(['openssl', 'genrsa', '-out', 'user.key', '2048'])`                                                                                          | Generate a 2048-bit RSA private key                               |
| Python     | `subprocess.run(['openssl', 'req', '-new', '-key', 'user.key', '-out', 'user.csr', '-subj', '/CN=EndUser'])`                                                 | Create a CSR using the private key                                |
| Python     | `subprocess.run(['openssl', 'x509', '-req', '-in', 'user.csr', '-CA', 'ca.crt', '-CAkey', 'ca.key', '-CAcreateserial', '-out', 'user.crt', '-days', '365'])` | Sign the CSR to generate a certificate valid for 365 days         |
| PowerShell | `Get-ChildItem -Path Cert:\CurrentUser\My`                                                                                                                   | List all certificates in the current user's personal store        |
| PowerShell | `Export-Certificate -Cert (Get-ChildItem Cert:\CurrentUser\My\$thumb) -FilePath C:\user.crt`                                                                 | Export a certificate to a `.crt` file                             |
| Bash       | `for file in *.pem; do openssl x509 -in "$file" -noout -subject; done`                                                                                       | Loop over `.pem` files to display the subject of each certificate |


### Summary

- **Python:** Best for automating multi-step OpenSSL processes.
- **PowerShell:** Ideal for Windows certificate management.
- **Bash:** Perfect for bulk operations on Linux/macOS.
```txt
💡 Tip: Always double-check paths and file names when automating certificate tasks to avoid overwriting files or using the wrong keys.
```
## References

1. **Python `subprocess` Module Documentation**  
   [https://docs.python.org/3/library/subprocess.html](https://docs.python.org/3/library/subprocess.html)  

2. **OpenSSL Documentation**  
   [https://www.openssl.org/docs/man1.1.1/man1/](https://www.openssl.org/docs/man1.1.1/man1/)  

3. **PowerShell Certificate Cmdlets**  
   [https://learn.microsoft.com/en-us/powershell/module/pkiclient/?view=windowsserver2022-ps](https://learn.microsoft.com/en-us/powershell/module/pkiclient/?view=windowsserver2022-ps)  

4. **Bash Scripting Basics**  
   [https://www.gnu.org/software/bash/manual/bash.html](https://www.gnu.org/software/bash/manual/bash.html)  

5. **Mermaid Diagrams for Markdown**  
   [https://mermaid-js.github.io/mermaid/#/](https://mermaid-js.github.io/mermaid/#/)  

6. **Certificate Automation Concepts**  
   - Generating RSA Keys, CSRs, and Signing Certificates:  
     [https://www.ssl.com/how-to/create-a-csr-in-openssl/](https://www.ssl.com/how-to/create-a-csr-in-openssl/)  
