# Setting Up a Linux Server

When starting the setup for a Linux machine, you have many options for distros. However, for server use, I recommend using a **Command Line Interface (CLI)** with either **Ubuntu Server LTS** or **Debian 12**. If given the option, do not install a Desktop Environment (GUI), as it can significantly impact server performance. However, this is optional.

## Creating Bootable Media
- For creating bootable USB drives, I recommend:
  - [Balena Etcher](https://www.balena.io/etcher)
  - [Rufus](https://rufus.ie) (if Etcher doesn’t work).
- If using a Type-1 or bare-metal hypervisor (e.g., Proxmox or ESXi), download the ISO/TAR.ZST files and upload them to your hypervisor's local storage.

---

## Initial Setup Steps
1. **Secure Root Password**
   - Follow [NIST 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) guidelines: 
     - Passwords should be at least 8 characters long.
     - Include ASCII characters for additional complexity.

2. **Set Static IP**
   - Configure during setup or reserve the IP address in your router to ensure consistent network access.

3. **Log In**
   - Once setup is complete, log in using the system name:
     ```plaintext
     NameOfSystem login:
     ```
     - Use `root` or the username you created during setup.
   - After logging in, update the system:
     ```bash
     sudo apt-get update && sudo apt-get upgrade
     ```
   - If on Debian, install `sudo`:
     ```bash
     apt install sudo
     ```

---

## Creating a User with Least Privilege
1. **Add a New User**
   ```bash
   sudo useradd example
Verify user creation:
bash
Copy code
sudo id example
Set a password for the new user:
bash
Copy code
sudo passwd example
Create a home directory:
bash
Copy code
mkhomedir_helper example
Add the user to the sudo group:
bash
Copy code
sudo adduser example sudo
Switch to the New User
bash
Copy code
su example
bash
Confirm with:
bash
Copy code
whoami
Setting Up SSH
Install OpenSSH

bash
Copy code
sudo apt install openssh-server
Verify installation and edit the configuration:
bash
Copy code
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original
sudo chmod a-w /etc/ssh/sshd_config.original
nano /etc/ssh/sshd_config
Recommended changes:
Change port to 2222.
Set PermitRootLogin to no.
Restart SSH Service

bash
Copy code
sudo systemctl restart sshd.service
Generate SSH Keys

bash
Copy code
ssh-keygen -t rsa -b 4096
Connecting to the Server

Use tools like:
PuTTY
Linux terminal:
bash
Copy code
ssh remote_username@remote_host
Optional: Setting Up SFTP
SFTP allows easy file transfers via a GUI instead of using wget in the terminal.

Configure SFTP in SSH

Add the following to the bottom of /etc/ssh/sshd_config:
plaintext
Copy code
Match Group sftpgroup
    ChrootDirectory %h
    PasswordAuthentication yes
    AllowTcpForwarding no
    X11Forwarding no
    ForceCommand internal-sftp
Restart the SSH service:
bash
Copy code
sudo systemctl restart sshd
Create SFTP Group and Add User

bash
Copy code
sudo addgroup sftpgroup
sudo usermod -aG sftpgroup example
sudo service ssh restart
Restart the System
 lastly you want to  type (sudo service ssh restart again) then restart the system now now your done

 Feedback Welcome
 
# Wanted to make this because I never document what I do, this will make my life easier when doing doing my setups again
 #Also isn't Spell Checked and order is a little chanky but with will work for now
