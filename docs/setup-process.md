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

A small EC2 instance type was used for this lab. The tutorial showed a small general-purpose instance suitable for testing.

For a personal lab, a small instance is enough because the goal is not production-scale performance.

## 5. Create SSH Key Pair

An SSH key pair was created and downloaded as a `.pem` file.

The private key file is required to SSH into the EC2 instance.

Security note:

```txt
The .pem file should never be uploaded to GitHub.
```

## 6. Launch the EC2 Instance

After selecting the AMI, instance type, and key pair, the EC2 instance was launched.

Once the instance passed its status checks, the public DNS address was used for SSH access.

## 7. SSH Into the Server

PowerShell was used to connect to the EC2 instance with SSH.

The command followed this general structure:

```bash
ssh -i "vpnserver.pem" root@<ec2-public-dns>
```

The actual public DNS should not be published in the repository.

## 8. Accept OpenVPN License Agreement

After connecting to the instance, OpenVPN Access Server displayed its license agreement.

The agreement was accepted by typing:

```txt
yes
```

## 9. Complete Initial OpenVPN Setup

The OpenVPN Access Server setup wizard asked a series of configuration questions. The lab used mostly default settings.

The setup initialized the OpenVPN Access Server service and generated the Admin UI and Client UI URLs.

## 10. Reconnect as `openvpnas`

After initial setup, the system instructed the user to log in as:

```txt
openvpnas
```

This is the Linux user used to administer the OpenVPN Access Server instance.

## 11. Set the OpenVPN Admin Password

After reconnecting as `openvpnas`, the OpenVPN admin password was set using:

```bash
sudo passwd openvpn
```

This sets the password for the OpenVPN Admin UI user:

```txt
openvpn
```

## 12. Log Into the Admin UI

The Admin UI was accessed in the browser using:

```txt
https://<public-ip>:943/admin
```

The login used:

```txt
Username: openvpn
Password: password created with sudo passwd openvpn
```

## 13. Configure VPN Routing

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

## 14. Update the Running Server

After saving the VPN routing setting, the OpenVPN Admin UI required the running server to be updated.

The lab clicked:

```txt
Update Running Server
```

## 15. Log Into the Client Portal

The OpenVPN user portal was accessed at:

```txt
https://<public-ip>:943/
```

The same `openvpn` user credentials were used.

## 16. Connect Using OpenVPN Connect

OpenVPN Connect was used as the VPN client.

The profile was added/imported, and the client connected to the AWS-hosted OpenVPN server.

## 17. Verify the VPN

The VPN connection was verified by searching:

```txt
what's my IP
```

After connecting, the displayed public IP matched the AWS EC2 public IP, confirming that traffic was routing through the VPN.
