# Setup Process

## 1. Open AWS EC2

The lab started in the AWS console by opening the EC2 service. EC2 was used to host the VPN server as a cloud-based virtual machine.

## 2. Launch a New Instance

A new EC2 instance was launched from the EC2 dashboard.

## 3. Select OpenVPN Access Server from AWS Marketplace

Instead of using a standard Linux image, the lab used the OpenVPN Access Server AMI from AWS Marketplace.

The selected AMI was:

```txt
OpenVPN Access Server
```

The Marketplace page showed the BYOL option, which allows free usage for two connected VPN devices.

## 4. Choose Instance Type

The instance type used for this lab was:

```txt
t3.small
```

This instance type was selected because it provides enough resources for a personal OpenVPN lab without being intended for production-scale VPN usage.

## 5. Configure Network Settings and Security Group Rules

During the EC2 launch process, the network settings section was used to configure the instance's cloud network and firewall behavior.

The network settings included:

- Selecting the AWS VPC for the instance
- Using a default subnet selection
- Enabling auto-assignment of a public IP address
- Creating a new security group for the VPN server
- Configuring inbound traffic rules for SSH, HTTPS, the OpenVPN web portal, and OpenVPN tunnel traffic

Auto-assigning a public IP was required so the VPN server could be reached from the internet.

The security group acted as the instance-level firewall. The inbound rules configured for this lab were:

| Purpose | Type | Protocol | Port | Source |
|---|---|---|---:|---|
| SSH administration | SSH | TCP | 22 | My IP only |
| HTTPS access | HTTPS | TCP | 443 | Anywhere |
| OpenVPN web/admin portal | Custom TCP | TCP | 943 | My IP only |
| OpenVPN tunnel traffic | Custom UDP | UDP | 1194 | My IP only |

The source IP for SSH, the OpenVPN admin portal, and OpenVPN UDP traffic was restricted to `My IP` instead of being left fully open to the internet. This reduced unnecessary exposure while still allowing the local machine to administer and connect to the server.

Security note:

```txt
Do not publish the actual home IP address, public DNS name, or AWS account details in GitHub screenshots or documentation.
```

## 6. Create SSH Key Pair

An SSH key pair was created and downloaded as a `.pem` file.

The private key file is required to SSH into the EC2 instance.

Security note:

```txt
The .pem file should never be uploaded to GitHub.
```

## 7. Launch the EC2 Instance

After selecting the AMI, instance type, network settings, security group rules, and key pair, the EC2 instance was launched.

Once the instance passed its status checks, the public DNS address was used for SSH access.

## 8. SSH Into the Server

PowerShell was used to connect to the EC2 instance with SSH.

The command followed this general structure:

```bash
ssh -i "vpnserver.pem" root@<ec2-public-dns>
```

The actual public DNS should not be published in the repository.

## 9. Accept OpenVPN License Agreement

After connecting to the instance, OpenVPN Access Server displayed its license agreement.

The agreement was accepted by typing:

```txt
yes
```

## 10. Complete Initial OpenVPN Setup

The OpenVPN Access Server setup wizard asked a series of configuration questions. The lab used mostly default settings.

The setup initialized the OpenVPN Access Server service and generated the Admin UI and Client UI URLs.

## 11. Reconnect as `openvpnas`

After initial setup, the system instructed the user to log in as:

```txt
openvpnas
```

This is the Linux user used to administer the OpenVPN Access Server instance.

## 12. Set the OpenVPN Admin Password

After reconnecting as `openvpnas`, the OpenVPN admin password was set using:

```bash
sudo passwd openvpn
```

This sets the password for the OpenVPN Admin UI user:

```txt
openvpn
```

## 13. Log Into the Admin UI

The Admin UI was accessed in the browser using:

```txt
https://<public-ip>:943/admin
```

The login used:

```txt
Username: openvpn
Password: password created with sudo passwd openvpn
```

## 14. Configure VPN Routing

Inside the Admin UI, the lab went to:

```txt
Configuration -> VPN Settings
```

The setting changed was:

```txt
Should client Internet traffic be routed through the VPN?
```

The selected option was:

```txt
Yes, using NAT
```

This allows client internet traffic to route through the VPN server.

## 15. Update the Running Server

After saving the VPN routing setting, the OpenVPN Admin UI required the running server to be updated.

The lab clicked:

```txt
Update Running Server
```

## 16. Log Into the Client Portal

The OpenVPN user portal was accessed at:

```txt
https://<public-ip>:943/
```

The same `openvpn` user credentials were used.

## 17. Connect Using OpenVPN Connect

OpenVPN Connect was used as the VPN client.

The profile was added/imported, and the client connected to the AWS-hosted OpenVPN server.

## 18. Verify the VPN

The VPN connection was verified by searching:

```txt
what's my IP
```

After connecting, the displayed public IP matched the AWS EC2 public IP, confirming that traffic was routing through the VPN.
