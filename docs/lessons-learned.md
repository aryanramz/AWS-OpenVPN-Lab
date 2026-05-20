# Lessons Learned

## What I Built

For this project, I built a VPN server using AWS resources and OpenVPN Access Server. The goal was not just to connect to a VPN, but to understand what is involved in deploying one in a cloud environment.

The lab used an EC2 instance as the VPN server, AWS security groups as the firewall layer, SSH key-based access for server administration, and OpenVPN Connect as the local VPN client.

## What I Learned About AWS

This project helped me understand how EC2 instances are set up and how different AWS configuration choices affect both functionality and security.

The most important AWS concepts I worked with were:

- EC2 instance deployment
- AWS Marketplace AMI selection
- Instance type selection
- SSH key pairs
- Public IP assignment
- VPC and subnet selection
- Security group inbound rules
- Cloud firewall behavior

The network settings were especially important because the VPN server needed to be reachable from my machine, but it also needed to avoid unnecessary public exposure. This made the security group configuration one of the most important parts of the lab.

## What I Learned About Networking

This lab helped me better understand how traffic moves between a local machine, the public internet, and a cloud-hosted server.

The main networking takeaways were:

- The VPN server needs specific ports open in order to function.
- Security group rules directly affect whether SSH, the OpenVPN web portal, and VPN traffic can reach the instance.
- Client traffic can be routed through the VPN so that external websites see the VPN server's public IP instead of the original local network IP.
- Public IP verification is important because a VPN client showing "connected" is not enough proof by itself.

The project made it clear that networking is not just about making a connection work. It is also about controlling which traffic is allowed and which traffic should be blocked.

## What Confused Me

The main issue I ran into was with the SSH private key file on Windows.

When I tried to SSH into the EC2 instance, Windows gave a warning similar to:

```txt
WARNING: UNPROTECTED PRIVATE KEY FILE
bad permissions
```

My understanding is that this happened because the `.pem` file permissions were too open, meaning the key file could potentially be accessed by more users than it should. SSH rejected the key because private keys are expected to have restricted permissions.

The fix was to reset and restrict the file permissions using PowerShell or Command Prompt.

```powershell
# Reset existing permissions
icacls.exe yourkey.pem /reset

# Grant read-only access to the current user
icacls.exe yourkey.pem /GRANT:R "$($env:USERNAME):(R)"

# Disable inheritance and remove inherited permissions
icacls.exe yourkey.pem /inheritance:r
```

After correcting the permissions, the SSH key could be used properly.

This was useful because it showed that cloud setup problems are not always caused by AWS. Sometimes the issue is local system permissions, SSH behavior, or operating system security rules.

## Security Takeaways

The biggest security takeaway from this project was that the network configuration matters as much as the server configuration.

A VPN server is exposed to the internet by design, so the security group rules need to be configured carefully. If too much traffic is allowed, the server has unnecessary exposure. If too little traffic is allowed, the VPN will not function correctly.

Important security takeaways:

- The `.pem` private key should never be uploaded to GitHub.
- The OpenVPN profile should be treated like a credential.
- Public IP addresses, public DNS names, and AWS resource IDs should be redacted from screenshots.
- SSH access should be restricted to a trusted IP address when possible.
- The OpenVPN admin portal should not be broadly exposed if it can be restricted.
- Security group rules should be reviewed carefully before launching the instance.
- VPN routing should be verified instead of assuming it works.

This project helped reinforce the idea that a working setup is not automatically a secure setup.

## What I Would Improve

If I expanded this lab, the best improvement would be to make it closer to a real remote-access environment.

The next version of the project could include:

- A private EC2 instance with no public IP address
- VPN-based access to that private instance
- Tighter restrictions on the OpenVPN Admin UI
- AWS CloudWatch monitoring for instance health and traffic
- A cost analysis for running the VPN server
- Terraform automation for repeatable deployment
- A comparison between OpenVPN and WireGuard

The strongest improvement would be adding a private subnet and a private EC2 instance. That would make the project more realistic because the VPN would not only hide public traffic, but also provide controlled access to private cloud infrastructure.

## Career Relevance

This project is relevant to cloud security, networking, IT support, and cybersecurity because it involved several practical skills:

- Deploying cloud infrastructure
- Configuring EC2 networking
- Using security groups as firewall controls
- Troubleshooting SSH access
- Handling private key permissions
- Testing VPN connectivity
- Redacting sensitive information before publishing documentation

The main thing I gained from this lab was a better understanding of how cloud networking, access control, and VPN infrastructure work together.
