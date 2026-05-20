# Future Improvements

## 1. Restrict Admin Access

Restrict SSH and OpenVPN Admin UI access to a trusted IP address instead of allowing broad internet access.

## 2. Add a Private EC2 Instance

A stronger lab would include a second EC2 instance in a private subnet with no public IP address.

The VPN server would be used to access that private instance.

This would better simulate enterprise remote access infrastructure.

## 3. Automate with Terraform

The current deployment was manual. A future version could use Terraform to automate:

- EC2 instance creation
- Security group rules
- Key pair configuration
- VPC settings
- Subnet setup

## 4. Add Monitoring

AWS CloudWatch could be used to monitor:

- EC2 status
- CPU usage
- Network traffic
- Failed login attempts
- VPN server availability

## 5. Compare OpenVPN and WireGuard

A future version could compare OpenVPN with WireGuard across:

- Setup complexity
- Performance
- Client support
- Security model
- Configuration simplicity

## 6. Add Cost Breakdown

A cost breakdown would help show cloud cost awareness.

Potential cost areas:

- EC2 instance runtime
- EBS storage
- Data transfer
- Elastic IP usage
- Marketplace software costs

## 7. Harden the Server

Future hardening steps:

- Disable password-based SSH
- Restrict inbound firewall rules
- Enable automatic security updates
- Review OpenVPN logs
- Rotate credentials
- Add MFA if supported
