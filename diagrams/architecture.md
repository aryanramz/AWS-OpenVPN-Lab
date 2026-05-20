# Architecture Diagram

```txt
+----------------------+
| Local Device         |
| OpenVPN Connect      |
+----------+-----------+
           |
           | Encrypted VPN Tunnel
           |
+----------v-----------+
| Internet             |
+----------+-----------+
           |
           | TCP 943 / UDP 1194
           |
+----------v----------------------+
| AWS EC2 Instance                 |
| OpenVPN Access Server            |
| Security Group Protected         |
+----------+----------------------+
           |
           | NAT Routing
           |
+----------v-----------+
| Public Internet      |
| Traffic exits via    |
| AWS EC2 public IP    |
+----------------------+
```

## Flow Explanation

1. The local device runs OpenVPN Connect.
2. The client connects to the AWS-hosted OpenVPN Access Server.
3. The VPN tunnel encrypts traffic between the client and the EC2 instance.
4. The EC2 security group controls which ports are reachable.
5. OpenVPN uses NAT routing so client internet traffic exits through the AWS EC2 public IP address.
