# Challenges Faced

This document summarizes the main setup issues encountered while building and configuring the AWS OpenVPN lab. These challenges were useful because they exposed real-world problems around local file permissions, VPN routing behavior, and cloud networking setup.

## 1. SSH `.pem` File Permission Error on Windows

### Problem

While connecting to the AWS EC2 instance over SSH, the private key file produced a Windows permissions error. The warning indicated that the `.pem` file had overly permissive access settings.

Common error messages included:

```text
WARNING: UNPROTECTED PRIVATE KEY FILE!
bad permissions
```

This happened because the private key file was accessible by more users/groups than SSH allows. SSH private keys need to be restricted so only the current user can read them. If the key is too open, the SSH client refuses to use it for security reasons.

### Cause

On Windows, downloaded files may inherit broad permissions from the parent folder. In this case, the `.pem` file likely inherited access permissions that allowed other users or groups on the machine to read the file.

### Solution

The issue was fixed by resetting the file permissions, removing inherited permissions, and granting read-only access to the current Windows user.

Run Command Prompt or PowerShell as Administrator, navigate to the folder containing the `.pem` file, and run:

```powershell
icacls.exe your_key.pem /reset
icacls.exe your_key.pem /inheritance:r
icacls.exe your_key.pem /GRANT:R "%USERNAME%:(R)"
```

Replace `your_key.pem` with the actual private key filename.

### Command Breakdown

| Command | Purpose |
|---|---|
| `icacls.exe your_key.pem /reset` | Resets existing explicit permissions on the file. |
| `icacls.exe your_key.pem /inheritance:r` | Disables inherited permissions from the parent folder and removes inherited entries. |
| `icacls.exe your_key.pem /GRANT:R "%USERNAME%:(R)"` | Grants read-only access to the current Windows user. |

### Lesson Learned

SSH key permissions are a security requirement, not just a Windows file setting issue. Even if the key file exists and the SSH command is correct, the connection can fail if the key is readable by too many users.

---

## 2. OpenVPN Client Internet Traffic Routing Setting

### Problem

During OpenVPN Access Server setup, there was a configuration option asking whether client internet traffic should be routed through the VPN. This setting is important because it determines whether VPN clients only access private network resources or whether all internet traffic goes through the VPN tunnel.

The confusing part was that this setting appeared during the command-line setup process, but it was not easy to locate later in the newer OpenVPN web interface.

### Cause

The OpenVPN Access Server setup process includes important routing questions during the initial command-line configuration. However, the newer OpenVPN web UI does not always present these options in an obvious location, especially for a beginner setting up the VPN for the first time.

### Solution

The setting was enabled during the command-line setup process when OpenVPN prompted for it. This avoided needing to manually search through the OpenVPN web interface afterward.

The key configuration decision was to allow client internet traffic to route through the VPN when prompted during setup.

### Why This Mattered

Without enabling this routing behavior, the VPN may connect successfully but not route general internet traffic through the VPN. That would make the setup incomplete if the goal is to use the VPN as a full-tunnel VPN rather than only accessing private AWS resources.

### Lesson Learned

Some critical configuration options are easier to set correctly during the initial setup process than after installation. It is important to pay attention to command-line setup prompts because they may control behavior that is less obvious in the application UI later.

---

## Overall Takeaways

This project helped reinforce several practical cloud and networking lessons:

- AWS infrastructure setup requires both cloud-side and local machine configuration.
- SSH private key security is enforced by the SSH client and must be handled correctly.
- VPN functionality depends heavily on routing decisions, not just whether the client can connect.
- Setup prompts can affect long-term behavior, so they should be read carefully rather than skipped.
- Troubleshooting cloud projects often requires checking multiple layers: local OS permissions, AWS security groups, EC2 access, VPN server configuration, and client routing behavior.
