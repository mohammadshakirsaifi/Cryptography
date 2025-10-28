# Section 6: Managing HSMs

This section covers managing Hardware Security Modules (HSMs) using **Thales Luna HSM** and **AWS CloudHSM**.
# HSM Workflow Diagram
---
## Thales Luna HSM

Client
  |
  v
Partition (login via `vtl login -n <partition_name>`)
  |
  v
Key Operations
  ├── Create Key (`vtl createKey`)
  ├── List Keys (`vtl listKeys`)
  └── Delete Key (`vtl deleteKey`)

---

## AWS CloudHSM

Client (within VPC/network)
  |
  v
HSM Cluster (login via `cloudhsm_mgmt_util loginHSM`)
  |
  v
Key Operations
  ├── Generate Key (`cloudhsm_mgmt_util genKey`)
  └── Other management operations
---
```mermaid
graph TD
  Client --> Partition[Partition (login via vtl login -n <partition_name>)]
  Partition --> KeyOps[Key Operations]
  KeyOps --> Create[Create Key (vtl createKey)]
  KeyOps --> List[List Keys (vtl listKeys)]
  KeyOps --> Delete[Delete Key (vtl deleteKey)]
```
---

## Thales Luna HSM

Thales Luna HSM can be managed using the `vtl` CLI.

### Common Commands

```bash
# Login to a partition
vtl login -n <partition_name>
# Example Output:

Partition <partition_name> login successful.

# Create an AES key (256-bit) with a specific label
vtl createKey -type AES -size 256 -label "DemoAESKey"

# Example Output:

Key "DemoAESKey" created successfully.

# List all keys in the partition
vtl listKeys

# Example Output:

Label           Type    Size
---------------------------------
DemoAESKey      AES     256

# Delete a key by label
vtl deleteKey -label "DemoAESKey"

# Example Output:

Key "DemoAESKey" deleted successfully.
```
#### Notes:

- Ensure you have network access to the Luna HSM appliance.
- Keys are created within partitions; you must log in to the correct partition first.



#### AWS CloudHSM

AWS CloudHSM can be managed using the cloudhsm_mgmt_util CLI.

Common Commands
```bash
# Login to the HSM
cloudhsm_mgmt_util loginHSM <user> <password>

# Example Output:
Login successful.

# Generate an RSA key (2048-bit) with a specific label
cloudhsm_mgmt_util genKey -keyType rsa:2048 -keyLabel "MyKey"
# Example Output:

RSA key "MyKey" (2048-bit) generated successfully.
```

#### Notes:

- CloudHSM requires VPC network access to the HSM cluster.
- Keys are created per HSM instance; ensure proper login before performing operations.

## Troubleshooting
### Thales Luna HSM

- **Login fails:**
Ensure the partition name is correct and the user has permission. Verify network connectivity to the HSM.

- **Key creation fails:**
Check that the partition has enough quota for new keys and that the key label is unique.

- **Key not listed:**
Confirm you are logged into the correct partition. Use vtl listPartitions to verify.

### AWS CloudHSM

- **Login fails:**
Ensure your IP has access to the HSM cluster’s security group. Check username/password correctness.

- **Key generation fails:**
Verify that the HSM has sufficient capacity and that the key label is unique.

- **CLI commands hang:**
Check network connectivity to the HSM endpoints and ensure the HSM cluster is in an ACTIVE state.

## Best Practices
### Key Management

- Use descriptive and unique labels for each key to avoid confusion.
- Regularly rotate keys according to your organization’s security policy.
- Delete unused keys promptly to reduce risk.

### Partition Management (Thales Luna)

- Assign separate partitions for different applications or teams for isolation.
- Monitor partition quotas to prevent failures when creating new keys.

### Security

- Always use strong passwords for HSM users.
- Limit network access to HSMs to trusted IPs or VPCs.
- Enable logging and monitoring for all HSM operations.

### Backup and Recovery

- Regularly backup HSM configurations and key metadata.
- Test recovery procedures periodically to ensure continuity in case of failures.


Summary Table
| HSM Type        | Deployment                 | CLI Tool                 |
| --------------- | -------------------------- | ------------------------ |
| Thales Luna HSM | On-premise / Network-based | `vtl` CLI                |
| AWS CloudHSM    | AWS Only                   | `cloudhsm_mgmt_util` CLI |

References

- [Thales Luna HSM CLI Guide](https://cpl.thalesgroup.com/software/management-tools/luna-remote-client)
- [AWS CloudHSM Documentation](https://docs.aws.amazon.com/cloudhsm/latest/userguide/)


✅ **Changes / Additions I made:**
1. Added proper Markdown formatting for headings, code blocks, tables, and lists.  
2. Clarified network requirements and usage notes for both HSM types.  
3. Added a references section with official documentation links.  
4. Fixed minor command formatting for readability.  

