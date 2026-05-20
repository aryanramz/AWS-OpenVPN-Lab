# Security Considerations

## Sensitive Files

The following files should never be uploaded to GitHub:

- `.pem` private key files
- `.ovpn` client profiles
- OpenVPN configuration files containing secrets
- Passwords
- Admin URLs
- AWS account information

## SSH Key Security

The EC2 instance was accessed using SSH key-based authentication. This is stronger than password-based SSH access, but the private key must be protected.

If the `.pem` file is exposed, an attacker may be able to access the server.

## OpenVPN Admin UI Exposure

The OpenVPN Admin UI runs on:

```txt
TCP 943
```

This interface is sensitive because it controls the VPN server.

In a production-style setup, access to the Admin UI should be restricted to a trusted IP address.

## Security Group Hardening

A stronger security group configuration would:

- Restrict SSH to a trusted IP address
- Restrict Admin UI access to a trusted IP address
- Allow VPN tunnel traffic only as needed
- Remove unused inbound rules

## Client Profile Risk

OpenVPN client profiles should be treated like credentials. If someone obtains a working profile and login credentials, they may be able to connect to the VPN.

Client profiles should not be committed to GitHub.

## Public IP Redaction

Screenshots should be redacted before being uploaded.

Redact:

- EC2 public IP
- Public DNS
- Private IP
- Instance ID
- AWS account ID
- Key pair name if sensitive
- OpenVPN profile details

## Lab vs. Production

This was a learning lab, not a production VPN deployment.

A production-ready VPN would require:

- MFA
- Logging and monitoring
- Patch management
- Strong access controls
- Alerting
- Backup and recovery planning
- Least privilege firewall rules
- Regular credential rotation
