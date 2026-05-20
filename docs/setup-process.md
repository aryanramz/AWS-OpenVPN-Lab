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

The first SSH attempt used the `root` user:

```bash
ssh -i "<key-pair>.pem" root@<ec2-public-dns>
```

The server rejected direct root login and instructed that the instance should be accessed as the `openvpnas` user instead.

The correct SSH command followed this structure:

```bash
ssh -i "<key-pair>.pem" openvpnas@<ec2-public-dns>
```

The actual key file name, public DNS, and public IP address should not be published in the repository.

## 9. Accept OpenVPN License Agreement

After connecting as `openvpnas`, the OpenVPN Access Server Initial Configuration Tool displayed the End User License Agreement.

The agreement was accepted by typing:

```txt
yes
```

## 10. Complete Initial OpenVPN Setup

The OpenVPN Access Server Initial Configuration Tool asked several setup questions. The lab used mostly default settings, except for enabling VPN client internet traffic routing.

The main setup choices were:

| Setting | Selection Used |
|---|---|
| Primary Access Server node | Default: yes |
| Admin Web UI network interface | Default: all interfaces, `0.0.0.0` |
| OpenVPN CA algorithm | Default: `secp384r1` |
| Self-signed web certificate algorithm | Default: `secp384r1` |
| Admin Web UI port | Default: `943` |
| OpenVPN daemon TCP port | Default: `443` |
| Route client traffic through VPN | `yes` |
| Route client DNS traffic through VPN | Default: no |
| Allow private subnets to be accessible to clients | EC2 default: yes |
| Admin authentication method | Local |
| Admin UI username | `openvpn` |
| Activation key | Left blank to specify later |

During this setup process, the password for the `openvpn` Admin UI account was created directly inside the configuration wizard.

The password had to meet OpenVPN's complexity requirements:

```txt
At least 8 characters, including a digit, an uppercase letter, and a symbol.
```

This means the admin password was not created with `sudo passwd openvpn` in this lab. It was created during the Initial Configuration Tool flow.

## 11. Finish OpenVPN Initialization

After the setup choices were submitted, OpenVPN initialized the server.

The setup process performed actions such as:

- Initializing OpenVPN Access Server
- Writing the configuration file
- Creating the default profile
- Adding the `openvpn` admin user
- Setting the admin password in the OpenVPN database
- Preparing web certificates
- Enabling and starting the OpenVPN Access Server service

After initialization completed, the setup tool displayed the Admin UI and Client UI URLs.

The URLs followed this general structure:

```txt
Admin UI:  https://<public-ip>:943/admin
Client UI: https://<public-ip>:943/
```

The actual public IP address should not be published in the repository.

## 12. Log Into the Admin UI

The Admin UI was accessed in the browser using:

```txt
https://<public-ip>:943/admin
```

The login used:

```txt
Username: openvpn
Password: password created during the Initial Configuration Tool setup
```

## 13. Confirm VPN Routing Settings

Client internet traffic routing was enabled during the Initial Configuration Tool setup by answering `yes` to:

```txt
Should client traffic be routed by default through the VPN?
```

This allows client internet traffic to route through the AWS-hosted VPN server.

If this setting needs to be checked or changed later, it can be reviewed in the Admin UI under:

```txt
Configuration -> VPN Settings
```

## 14. Log Into the Client Portal

The OpenVPN user portal was accessed at:

```txt
https://<public-ip>:943/
```

The same `openvpn` user credentials were used.

## 15. Connect Using OpenVPN Connect

OpenVPN Connect was used as the VPN client.

The profile was added/imported, and the client connected to the AWS-hosted OpenVPN server.

## 16. Verify the VPN

The VPN connection was verified by searching:

```txt
what's my IP
```

After connecting, the displayed public IP matched the AWS EC2 public IP, confirming that traffic was routing through the VPN.
