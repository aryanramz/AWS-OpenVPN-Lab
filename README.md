# AWS-OpenVPN-Lab

## Overview

This project documents the deployment of a self-hosted VPN server on Amazon Web Services using OpenVPN Access Server. The goal of this lab was to understand how cloud-hosted VPN infrastructure works, including EC2 provisioning, AWS Marketplace AMI deployment, SSH key authentication, security group configuration, OpenVPN admin setup, VPN client profiles, NAT routing, and connection testing.

This was completed as a hands-on cloud networking and cybersecurity lab.

## Project Summary

In this lab, I deployed an OpenVPN Access Server instance on AWS EC2 and configured it as a VPN endpoint. After completing the initial server setup, I connected to the VPN from a local device using OpenVPN Connect and verified that my public IP address changed to the AWS-hosted VPN server's IP address.

## Technologies Used

- Amazon Web Services
- Amazon EC2
- AWS Marketplace
- AWS VPC
- AWS Security Groups
- OpenVPN Access Server
- OpenVPN Connect
- SSH
- PowerShell
- Linux server administration

## Lab Architecture

```txt
Local Client Device
OpenVPN Connect
        |
        | Encrypted VPN Tunnel
        |
Internet
        |
AWS EC2 Instance
OpenVPN Access Server
        |
AWS VPC
