# Troubleshooting

## SSH Connection Failed

Possible causes:

- Wrong `.pem` key file
- Wrong EC2 public DNS
- Security group does not allow SSH
- Instance is not fully running
- Wrong Linux username

Fixes:

- Confirm the instance is running
- Confirm EC2 status checks passed
- Use the correct key pair
- Use the correct public DNS
- Confirm TCP port 22 is allowed from your IP

## OpenVPN Admin Page Did Not Load

Possible causes:

- Port 943 is blocked
- Instance is still initializing
- OpenVPN service is not running
- Browser is blocking the self-signed certificate page

Fixes:

- Confirm TCP port 943 is allowed in the security group
- Wait for the instance to finish setup
- Use `https://<public-ip>:943/admin`
- Accept the browser warning for the self-signed certificate

## Login Failed in Admin UI

Possible causes:

- Password was not set correctly
- Wrong username used
- Confusion between Linux user and OpenVPN admin user

Fix:

- SSH into the server as `openvpnas`
- Run:

```bash
sudo passwd openvpn
```

- Log into the Admin UI with:

```txt
Username: openvpn
```

## VPN Connected but IP Did Not Change

Possible causes:

- Internet traffic routing was not enabled
- NAT routing was not selected
- Running server was not updated after changing settings

Fix:

- Go to:

```txt
Configuration -> VPN Settings
```

- Set:

```txt
Should client Internet traffic be routed through the VPN? -> Yes, using NAT
```

- Click:

```txt
Update Running Server
```

## AWS Charges

Possible causes:

- EC2 instance left running
- Elastic IP allocated but unused
- Storage volume left behind

Fix:

- Stop or terminate the EC2 instance if no longer needed
- Release unused Elastic IP addresses
- Check AWS Billing dashboard
