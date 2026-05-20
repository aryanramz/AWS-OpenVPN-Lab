# Technical Notes

## What This Lab Demonstrates

This lab demonstrates how to deploy a cloud-hosted VPN server using AWS EC2 and OpenVPN Access Server.

The core technical concept is that a local client creates an encrypted tunnel to a remote server. In this lab, the remote server is an EC2 instance hosted in AWS.

## EC2 Role

The EC2 instance acts as the VPN server. It provides:

- Compute resources
- A public IP address
- Network connectivity
- A Linux server environment
- A host for OpenVPN Access Server

## OpenVPN Access Server Role

OpenVPN Access Server provides:

- VPN server software
- Admin web interface
- Client web portal
- VPN user management
- VPN client profiles
- Routing and NAT settings

## Linux Users vs. OpenVPN Users

This lab involved two different user concepts.

### Linux server user

```txt
openvpnas
```

This user is used to administer the OpenVPN Access Server instance through SSH.

### OpenVPN web/admin user

```txt
openvpn
```

This user is used to log into the OpenVPN Admin UI and Client UI.

This distinction matters because SSH server administration and VPN web portal authentication are separate.

## VPN Routing

The key Admin UI setting was:

```txt
Should client Internet traffic be routed through the VPN?
```

The selected option was:

```txt
Yes, using NAT
```

NAT allows traffic from VPN clients to be routed through the VPN server to the internet. This is why the client's public IP changes to the VPN server's public IP after connecting.

## Important Ports

| Port | Protocol | Purpose |
|---:|---|---|
| 22 | TCP | SSH access to the EC2 instance |
| 943 | TCP | OpenVPN Admin UI and Client UI |
| 443 | TCP | HTTPS access |
| 1194 | UDP | OpenVPN tunnel traffic |

## Security Group Role

The EC2 security group acts like a cloud firewall. It controls which inbound traffic can reach the VPN server.

For this lab, the security group needed to allow traffic for:

- SSH administration
- OpenVPN web access
- OpenVPN tunnel traffic

In a more secure setup, SSH and admin UI access should be restricted to a trusted IP address.

## VPN Verification

The VPN was tested by checking the public IP address before and after connecting.

Expected result:

```txt
Before VPN: local network public IP
After VPN: AWS EC2 public IP
```

This confirms that client internet traffic is being routed through the AWS VPN server.
