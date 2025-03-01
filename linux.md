# Linux Complete Guide

## Basic Linux Commands

### 1. Navigating the Filesystem
```sh
pwd  # Show current directory

ls  # List directory contents

cd <directory>  # Change directory

cd ..  # Move up one directory level

cd /  # Go to the root directory

find / -name file.txt  # Search for a file
```

### 2. File and Directory Management
```sh
touch file.txt  # Create an empty file

mkdir mydir  # Create a new directory

rm file.txt  # Remove a file

rm -r mydir  # Remove a directory and its contents

cp source.txt destination.txt  # Copy a file

mv oldname.txt newname.txt  # Rename or move a file

stat file.txt  # Show file details

df -h  # Check disk usage
```

### 3. Viewing and Editing Files
```sh
cat file.txt  # View file contents

less file.txt  # View large files page by page

nano file.txt  # Edit a file using Nano editor

vim file.txt  # Edit a file using Vim editor

head -n 10 file.txt  # View the first 10 lines

tail -n 10 file.txt  # View the last 10 lines

grep 'text' file.txt  # Search for text inside a file
```

### 4. User and Group Management
```sh
whoami  # Show the current user

who  # Show logged-in users

id  # Show user ID (UID) and group ID (GID)

adduser username  # Create a new user

passwd username  # Change a user’s password

deluser username  # Delete a user

usermod -aG groupname username  # Add user to a group

groups username  # Show groups a user belongs to
```

### 5. Process Management
```sh
ps aux  # Show running processes

top  # Monitor system resource usage

kill <PID>  # Terminate a process

pkill firefox  # Kill a process by name

htop  # Interactive process monitoring (requires installation)

nice -n 10 command  # Run a process with lower priority
```

### 6. Package Management

#### Debian/Ubuntu (APT)
```sh
sudo apt update  # Update package list

sudo apt upgrade  # Upgrade installed packages

sudo apt install package-name  # Install a package

sudo apt remove package-name  # Remove a package
```

### 7. Networking Commands
```sh
ifconfig  # Show network interfaces

ip a  # Show IP addresses

ping google.com  # Test connectivity

netstat -tulnp  # Show open ports

curl -I example.com  # Fetch HTTP headers

wget url  # Download a file from a URL

scp file.txt user@remote:/path/  # Securely copy files to another server

ssh user@remote  # Connect to a remote system

dig example.com  # Get DNS information for a domain
```

### 8. File Permissions and Ownership
```sh
ls -l  # View file permissions

chmod 644 file.txt  # Change file permissions

chown user:group file.txt  # Change file owner

umask 022  # Set default permissions
```

#### Understanding File Permissions
Each file has three permission categories:
- **Owner (User)** – The creator of the file
- **Group** – Users belonging to a specific group
- **Others** – All other users

Permissions are represented as:
- **r (Read)** – View file contents
- **w (Write)** – Modify file contents
- **x (Execute)** – Run a file as a program

Example:
```
-rw-r--r-- 1 user group 1234 Feb 28 10:00 file.txt
```

### 9. System Monitoring and Logs
```sh
uptime  # Show system uptime

free -h  # Check RAM usage

df -h  # Show disk space usage

journalctl -xe  # View system logs

tail -f /var/log/syslog  # View live logs

history  # Show command history
```

### 10. Archiving and Compression
```sh
tar -cvf archive.tar folder/  # Create a tar archive

tar -xvf archive.tar  # Extract a tar archive

tar -czvf archive.tar.gz folder/  # Compress with gzip

tar -xvzf archive.tar.gz  # Extract a gzip archive

zip -r archive.zip folder/  # Create a zip file

unzip archive.zip  # Extract a zip file
```

---

## Advanced Linux Topics

### 1. Shell Scripting
```sh
#!/bin/bash
echo "Hello, World!"
```
Run the script:
```sh
chmod +x script.sh
./script.sh
```

### 2. SSH Key Management

SSH Key Management is essential for secure remote access to Linux systems. Instead of using passwords, SSH keys provide a more secure and automated way to authenticate users.

The process involves generating a key pair (public and private keys), adding the public key to a remote server, and then using the private key to log in without a password. This is commonly used for remote administration, Git authentication, and secure automated deployments.

#### Generate an SSH key pair:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

#### Copy the SSH key to a remote server:
```
ssh-copy-id user@remote_host
```

#### Manually add an SSH key to `~/.ssh/authorized_keys` on the remote server.

#### Connect to a remote server using SSH:
```
ssh user@remote_host
```

### 3. Firewall Management (UFW and iptables)
```sh
sudo ufw enable  # Enable firewall
sudo ufw allow 22/tcp  # Allow SSH
sudo ufw deny 80/tcp  # Block HTTP
sudo ufw status  # Check firewall rules
```
For iptables:
```sh
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### 4. Systemd and Service Management
```sh
systemctl start service-name  # Start a service

systemctl stop service-name  # Stop a service

systemctl restart service-name  # Restart a service

systemctl enable service-name  # Enable service at boot

systemctl status service-name  # Check service status
```

### 5. Disk Partitioning and Mounting
```sh
fdisk -l  # List partitions
mkfs.ext4 /dev/sdb1  # Format a partition
mount /dev/sdb1 /mnt/data  # Mount a partition
umount /mnt/data  # Unmount a partition
```

---
