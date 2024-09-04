# Raspberry Pi 5 SSH Server

This project sets up an SSH server on a Raspberry Pi 5, configured with custom port forwarding to enhance security by reducing bot attacks on the default SSH port (22). The setup leverages knowledge from the Google Cybersecurity Certificate to manage DNS, routers, port forwarding rules, and SSH key configurations.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Security Enhancements](#security-enhancements)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This SSH server is hosted on a Raspberry Pi 5, designed for secure remote access from various devices. The server is set up with port forwarding rules to use a non-standard port, enhancing security by minimizing exposure to common brute-force attacks targeting port 22. Additionally, SSH key-based authentication is configured to replace password-based logins.

## Features

- **Custom SSH Port:** The server operates on a custom port rather than the default port 22, significantly reducing automated attack attempts.
- **SSH Key Authentication:** Ensures secure and password-less logins.
- **DNS Management:** Configured to resolve your domain name, making connections easier and more professional.
- **Router and Port Forwarding Management:** Port forwarding rules are set up to route traffic securely to the Raspberry Pi.
  
## Requirements

- **Raspberry Pi 5** with a suitable power supply.
- **MicroSD Card** with at least 16GB storage.
- **Internet Connection** for remote access.
- **Domain Name** (optional) configured to resolve to your home network's public IP address.
- **Router Access** to configure port forwarding rules.

## Installation

1. **Prepare the Raspberry Pi:**
   - Install the latest version of Ubuntu Server 24.04 LTS for Raspberry Pi.
   - Update the package list:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Install and Configure SSH:**
   - Ensure SSH is installed and enabled:
     ```bash
     sudo apt install openssh-server
     sudo systemctl enable ssh
     sudo systemctl start ssh
     ```

3. **Set Up Port Forwarding:**
   - Access your router's settings (usually accessible via `192.168.1.1`).
   - Configure port forwarding rules to forward traffic from a custom external port (e.g., 2222) to the Raspberry Pi's internal IP address on port 22.

4. **Configure SSH Keys:**
   - Generate SSH keys on your client machine:
     ```bash
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```
   - Copy the public key to the Raspberry Pi:
     ```bash
     ssh-copy-id -p 2222 pi@your_domain_or_ip
     ```

## Configuration

- **SSH Configuration:**
  Edit the SSH configuration file to update the port and disable password authentication:
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
  Change the following lines:
  ```plaintext
  Port 2222
  PasswordAuthentication no
  ```
  Restart SSH:
  ```bash
  sudo systemctl restart ssh
  ```

- **DNS Configuration:**
  Point your domain name to your home network's public IP address using your domain registrar's DNS management tools.

## Security Enhancements

- **Use a non-standard SSH port:** Reduces the risk of automated attacks on the default port 22.
- **SSH Key Authentication:** Provides a secure alternative to password-based logins.
- **Firewall Configuration:** Ensure only necessary ports are open using `ufw`:
  ```bash
  sudo ufw allow 2222/tcp
  sudo ufw enable
  ```

## Troubleshooting

- **SSH Connection Issues:** Ensure that the correct port is specified when connecting (e.g., `ssh -p 2222 pi@your_domain_or_ip`).
- **Port Forwarding Problems:** Double-check your router settings to ensure the forwarding rules are correctly configured.
- **DNS Resolution Errors:** Verify your domain name correctly resolves to your network's public IP address.
