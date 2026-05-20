# Screenshot Checklist

This folder should contain redacted screenshots that prove the lab was completed without exposing sensitive information.

## Recommended Screenshots

| File Name | What It Should Show | What To Redact |
|---|---|---|
| `marketplace-ami-redacted.png` | OpenVPN Access Server AMI selection | AWS account info |
| `ec2-instance-redacted.png` | EC2 instance running | instance ID, public IP, private IP, public DNS |
| `security-group-redacted.png` | inbound rules and ports | home IP address, security group ID if desired |
| `ssh-terminal-redacted.png` | SSH connection or setup wizard | public DNS, IP address, key path if sensitive |
| `openvpn-admin-login-redacted.png` | Admin UI login page | public IP, URL |
| `vpn-settings-redacted.png` | VPN routing setting | public IP, URL |
| `openvpn-client-connected-redacted.png` | OpenVPN Connect showing connected status | server IP, profile details |
| `ip-verification-redacted.png` | public IP changed after connecting | actual IP address |

## Do Not Upload

Do not upload screenshots that show:

- AWS account ID
- EC2 public IP address
- EC2 public DNS
- private key file contents
- `.ovpn` profile contents
- OpenVPN passwords
- unredacted OpenVPN profile names if sensitive
- billing details

## Minimum Evidence Needed

At minimum, add these screenshots:

1. EC2 instance running
2. Security group inbound rules
3. OpenVPN Admin UI or VPN Settings page
4. OpenVPN Connect connected status
5. IP verification before/after VPN connection
