# Linux Server Setup Guide

This guide walks you through the steps to set up a Linux machine for server use. It provides recommendations for distributions, security practices, and additional tools to streamline your server management.

## Table of Contents
1. Choosing a Distribution
2. Initial Setup
3. User Management
4. SSH Configuration
5. Optional: SFTP Setup
6. Notes

## Choosing a Distribution
For server use, I recommend:
- **Ubuntu Server LTS**
- **Debian 12**

Avoid installing a Desktop Environment (GUI) as it can significantly impact server performance. For creating bootable USB drives, use:
- [Balena Etcher](https://www.balena.io/etcher)
- [Rufus](https://rufus.ie/)

If using a hypervisor like **Proxmox** or **ESXi**, upload ISO or TAR.ZST files directly into your hypervisor's storage.

## Initial Setup
1. **Set a Secure Root Password**  
   Follow [NIST 800-63b](https://pages.nist.gov/800-63-3/sp800-63b.html) guidelines for strong passwords:
   - At least 8 characters long.
   - Include ASCII characters.
2. **Configure Static IP**  
   Reserve an IP address during setup or in your router's settings.
3. **Update and Upgrade**  
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```
4. **Install `sudo` (if using Debian)**  
   ```bash
   apt install sudo
   ```

## User Management
1. **Create a New User**  
   Replace `example` with your desired username:
   ```bash
   sudo useradd example
   sudo passwd example
   mkhomedir_helper example
   sudo adduser example sudo
   ```
2. **Switch to the New User**  
   ```bash
   su example
   bash
   whoami
   ```

## SSH Configuration
1. **Install OpenSSH Server**  
   ```bash
   sudo apt install openssh-server
   ```
2. **Backup SSH Configuration File**  
   ```bash
   sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original
   sudo chmod a-w /etc/ssh/sshd_config.original
   ```
3. **Edit SSH Configuration**  
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   - Change the port to `2222`.
   - Set `PermitRootLogin` to `no`.
4. **Restart SSH Service**  
   ```bash
   sudo systemctl restart sshd.service
   ```
5. **Generate SSH Keys**  
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
Use [PuTTY](https://www.putty.org/) or the following Linux command to connect:
```bash
ssh remote_username@remote_host
```

## Optional: SFTP Setup
1. **Edit SSH Configuration**  
   Add the following to `/etc/ssh/sshd_config`:
   ```plaintext
   Match Group sftpgroup
       ChrootDirectory %h
       PasswordAuthentication yes
       AllowTcpForwarding no
       X11Forwarding no
       ForceCommand internal-sftp
   ```
2. **Restart SSH Service**  
   ```bash
   sudo systemctl restart sshd
   ```
3. **Create SFTP Group and Add User**  
   ```bash
   sudo addgroup sftpgroup
   sudo usermod -aG sftpgroup example
   sudo service ssh restart
   ```

 Feedback Welcome
 
# Wanted to make this because I never document what I do, this will make my life easier when doing doing my setups again
