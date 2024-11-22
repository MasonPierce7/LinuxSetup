# LinuxSetup
When Starting the setup for a linux machine you have many options for distros but when setting up linux for server use I use a (CLI)command line interface and the distros of choice are always ubuntu server LTS or debian 12 and if given the option do not install a Desktop Environment (GUI)
Desktop Environments will tank performace on the server you want to run but It is optional. If using a USB I suggest (https://www.balena.io/etcher) but if that isnt working you can use (https://rufus.ie/) or if you are running on a Type-1 or a bare-metal hypervisor like proxmox or esxi you can download the isos/tar.zst files and input them into your local storage on the hypervisor

When going through the starting setup you want to make sure your root password is as secure as it can be per NIST 800-63b the password should be least 8 characters long and having ASCII (American Standard Code for Information Interchange) characters in your password
then you will want to setup a static IP address or reserve the IP in your router this can be done during setup or after
moving on when you are finished you should see a screen (NameofSystem login:) here you type in root or if it let you create a user you can put that instead then put in the password then you will see root@NameofSystem or user@NameofSystem
Then you type in (sudo apt-get update and sudo apt-get upgrade) or (sudo apt update -y && sudo apt upgrade -y) then if on debian type (apt install sudo)
here comes the part if you already made a user you can skip but we will be making a user account because we want Least-Privilege so that the application you are running has the minimum permissions to do its tasks this will also help against any protenial malicious code or application that might get a hand on the system'
ok you want to type (sudo useradd example) this will put it in /etc/default/useradd then to check if it worked you want to type type sudo id example) and you should get something like (uid=1000(example) gid=1000(example) groups=1000(example)
then go ahead and make a password to do that type (sudo passwd example) then when its done you will get (passwd: password updated successfully) then you will want to create a home directory (mkhomedir_helper example) then to test that it worked type (ls -la /home/example) then type (sudo adduser example sudo)
now we will go into the account you just made type (su example) then put in your password and then you will type (bash) then to test it type (whoami)
now that we are in the user you created will will now setup ssh so that you can remotely access the server to install OpenSSH you will type (sudo apt install openssh-server) It should already be installed but it sometimes isnt 
(https://ubuntu.com/server/docs/openssh-server) you will want to make a copy before editing (sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original) (sudo chmod a-w /etc/ssh/sshd_config.original) then we are going to edit the file type (nano /etc/ssh/sshd_config)
on the ones I talk about make sure to remove the #, now change the port to 2222 then scroll down to (PermitRootLogin) and change that to (no)
then restart the service (sudo systemctl restart sshd.service) now we will generate the ssh private and public key (ssh-keygen -t rsa -b 4096) then you can type the phrase you want to use 
the programs I use to connect to the OpenSSH server is (Putty https://www.putty.org/) and then in linux it should look something like this (ssh remote_username@remote_host)

Optional 
Now I listed this as optional because its not needed but I like it because I dont have to use the terminal as much when editing files
SFTP is nice when wanting to upload files to the server and not having to wget something to get the file and you can do it from a GUI
at the bottom of (sudo nano /etc/ssh/sshd_config) add
Match Group sftpgroup
    ChrootDirectory %h
 PasswordAuthentication yes
 AllowTcpForwarding no
 X11Forwarding no
 ForceCommand internal-sftp
 
 then type (sudo systemctl restart sshd) then add the group (sudo addgroup sftpgroup)
 then to add the user to the group type (sudo usermod -aG sftpgroup example)
 lastly you want to  type (sudo service ssh restart again) then restart the system now now your done
 
# Wanted to make this because I never document what I do, this will make my life easier when doing doing my setups again
 #Also isn't Spell Checked and order is a little chanky but with will work for now
