# 🐧 The Complete Linux Command Line Handbook

[![Linux](https://img.shields.io/badge/Linux-SCC?style=for-the-badge&logo=linux&logoColor=black)](https://www.kernel.org/)
[![Terminal](https://img.shields.io/badge/Shell-Bash%20%7C%20Zsh-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

---

## 📌 Table of Contents
* [1. Core Navigation & File Management](#1-core-navigation--file-management)
* [2. File Inspection & Editing](#2-file-inspection--editing)
* [3. Users, Groups & Permissions](#3-users-groups--permissions)
* [4. Process & System Monitoring](#4-process--system-monitoring)
* [5. Disk Space & Storage](#5-disk-space--storage)
* [6. Package Management](#6-package-management)
* [7. Advanced Networking & Firewalls](#7-advanced-networking--firewalls)
* [8. Text Processing & Regex Mastery](#8-text-processing--regex-mastery)
* [9. Systemd Services & Logs](#9-systemd-services--logs)
* [10. Compression, SSH & File Sync](#10-compression-ssh--file-sync)
* [11. Shell Operators & Redirection](#11-shell-operators--redirection)
* [12. Environment Variables & Shell Config](#12-environment-variables--shell-config)
* [13. DevOps Docker & Kubernetes Essentials](#13-devops-docker--kubernetes-essentials)

---

## 1. Core Navigation & File Management

| Command | Description | Example Syntax |
| :--- | :--- | :--- |
| `pwd` | Print current working directory | `pwd` |
| `ls` | List directory contents | `ls -la` *(All files + detailed attributes)* |
| `cd` | Change current directory | `cd /var/www` or `cd ~` *(Home)* |
| `mkdir` | Create new directories | `mkdir -p project/src/components` |
| `rmdir` | Remove empty directory | `rmdir old_folder` |
| `touch` | Create an empty file or update timestamp | `touch server.js` |
| `cp` | Copy files or directories | `cp -r src/ dist/` |
| `mv` | Move or rename files/directories | `mv old_name.txt new_name.txt` |
| `rm` | Remove files or directories permanently | `rm -rf target_folder/` |
| `find` | Search files by name, type, size, date | `find . -name "*.log" -mtime -1` |
| `locate` | Fast file search using indexed database | `locate nginx.conf` |
| `ln` | Create hard or symbolic links | `ln -s /opt/app/current app` |
| `tree` | Display directory structure visually | `tree -L 2` |
| `clear` | Clear the terminal display | `clear` or press `Ctrl + L` |

> 💡 **Pro-Tip:** Always double-check path references when running `rm -rf` to prevent accidental deletion of critical system files!

---

## 2. File Inspection & Editing

```bash
# View file contents directly in terminal
cat application.log

# View large files page-by-page (Q to exit)
less /var/log/syslog

# Inspect the top 15 lines of a file
head -n 15 dataset.csv

# Monitor continuous logs in real-time
tail -f -n 50 /var/log/nginx/access.log

# Compare two files line by line
diff config.old.yml config.new.yml

# Count lines, words, and bytes in a file
wc -l access.log

# Check the type of a file
file archive.tar.gz

# Command-line text editors
nano config.env        # Beginner friendly
vim /etc/nginx.conf    # Advanced modal editor
```

---

## 3. Users, Groups & Permissions

### Permission Matrix Cheat Sheet
* r (Read) = **4**
* w (Write) = **2**
* x (Execute) = **1**

### Commands

| Command | Action | Example |
| :--- | :--- | :--- |
| `chmod` | Change file/folder permissions | `chmod 755 script.sh` *(rwxr-xr-x)* |
| `chown` | Change file owner and group | `sudo chown -R www-data:www-data /var/www` |
| `sudo` | Execute command with superuser privileges | `sudo systemctl restart nginx` |
| `useradd` | Add a new user account | `sudo useradd -m -s /bin/bash devuser` |
| `passwd` | Set or update a user password | `sudo passwd devuser` |
| `usermod` | Modify user attributes / add to groups | `sudo usermod -aG docker devuser` |
| `groupadd` | Create a new group | `sudo groupadd deployers` |
| `whoami` | Print active logged-in user | `whoami` |
| `id` | Show UID, GID, and group memberships | `id devuser` |
| `su` | Switch to another user session | `su - devuser` |

---

## 4. Process & System Monitoring

```bash
# Display active running processes filtered by name
ps aux | grep node

# Interactive real-time process monitoring
top

# Enhanced interactive process viewer (Requires installation)
htop

# Terminate a process by Process ID (PID)
kill -9 10243

# Terminate all processes matching a string pattern
pkill -f "node server.js"

# Run a process in background detached from terminal session
nohup python3 script.py > output.log 2>&1 &

# List background/foreground jobs in current shell
jobs

# Bring a background job to the foreground
fg %1

# Manage automated cron jobs
crontab -e

# Show system uptime and load average
uptime

# Display kernel, OS, and hardware info
uname -a
```

---

## 5. Disk Space & Storage

```bash
# Display filesystem disk space usage (Human readable format)
df -h

# Summarize directory space consumption
du -sh /var/log/*

# Check RAM and Swap usage in gigabytes
free -h -g

# List block devices (disks, partitions)
lsblk

# Mount and unmount a filesystem
sudo mount /dev/sdb1 /mnt/data
sudo umount /mnt/data

# Check disk read/write activity in real time
iostat -x 2
```

---

## 6. Package Management

```bash
# Debian / Ubuntu (APT)
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
sudo apt remove nginx --purge
apt list --installed

# RHEL / CentOS / Fedora (YUM / DNF)
sudo dnf install nginx -y
sudo dnf remove nginx
sudo dnf update -y

# Arch Linux (Pacman)
sudo pacman -Syu
sudo pacman -S docker

# Universal package formats
sudo snap install code --classic
flatpak install flathub org.videolan.VLC
```

---

## 7. Advanced Networking & Firewalls

```bash
# Show IP addresses and network interfaces
ip a

# Show routing table
ip route

# Test connectivity to a host
ping -c 4 google.com

# Trace the network path to a destination
traceroute github.com

# List open ports and listening services
sudo netstat -tulnp
sudo ss -tulnp

# Check DNS resolution
nslookup example.com
dig example.com +short

# Download a file from a URL
wget https://example.com/file.tar.gz
curl -O https://example.com/file.tar.gz

# UFW firewall (Ubuntu)
sudo ufw allow 22/tcp
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status verbose

# firewalld (RHEL/CentOS)
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload

# iptables (low-level, legacy)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

---

## 8. Text Processing & Regex Mastery

```bash
# Search for a pattern inside files (recursive, line numbers)
grep -rn "ERROR" /var/log/app/

# Search with extended regex, case-insensitive
grep -Ei "fail(ed|ure)" server.log

# Stream editor: find and replace in-place
sed -i 's/localhost/127.0.0.1/g' config.yml

# Print specific columns from structured text
awk -F',' '{print $1, $3}' data.csv

# Sort and remove duplicate lines
sort names.txt | uniq -c | sort -nr

# Count occurrences of a word
grep -o "error" app.log | wc -l

# Extract text between patterns using sed
sed -n '/START/,/END/p' file.txt

# Cut specific fields by delimiter
cut -d':' -f1 /etc/passwd
```

---

## 9. Systemd Services & Logs

```bash
# Start, stop, restart a service
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx

# Enable a service to launch on boot
sudo systemctl enable nginx

# Check current service status
systemctl status nginx

# View real-time logs for a specific service
journalctl -u nginx -f

# View logs since a specific time
journalctl --since "1 hour ago"

# List all active systemd units
systemctl list-units --type=service

# Reload systemd manager configuration
sudo systemctl daemon-reload
```

---

## 10. Compression, SSH & File Sync

```bash
# Create a compressed tar archive
tar -czvf backup.tar.gz /var/www/project

# Extract a tar.gz archive
tar -xzvf backup.tar.gz -C /destination/

# Zip and unzip files
zip -r project.zip project/
unzip project.zip

# Generate an SSH key pair
ssh-keygen -t ed25519 -C "you@example.com"

# Copy your public key to a remote server
ssh-copy-id user@192.168.1.10

# Connect to a remote server via SSH
ssh user@192.168.1.10 -p 22

# Securely copy files to/from a remote server
scp -r ./dist user@server:/var/www/app

# Sync directories efficiently (local or remote)
rsync -avz --progress ./project/ user@server:/var/www/project/
```

---

## 11. Shell Operators & Redirection

```bash
# Redirect stdout to a file (overwrite)
echo "Deploy complete" > status.txt

# Redirect stdout to a file (append)
echo "New log entry" >> status.txt

# Redirect stderr to a file
command 2> error.log

# Redirect both stdout and stderr to the same file
command > all_output.log 2>&1

# Pipe output of one command into another
ps aux | grep nginx | awk '{print $2}'

# Run commands sequentially regardless of success
command1 ; command2

# Run second command only if first succeeds
command1 && command2

# Run second command only if first fails
command1 || command2

# Run a command in the background
long_task.sh &

# Command substitution
current_date=$(date +%F)
```

---

## 12. Environment Variables & Shell Config

```bash
# Print all environment variables
printenv

# Print a specific variable
echo $PATH

# Set a temporary variable (current session only)
export API_KEY="your_key_here"

# Persist a variable across sessions
echo 'export API_KEY="your_key_here"' >> ~/.bashrc
source ~/.bashrc

# Create a command alias
alias ll='ls -la'
echo "alias ll='ls -la'" >> ~/.bashrc

# Check which shell you're using
echo $SHELL
```

---

## 13. DevOps Docker & Kubernetes Essentials

```bash
# --- Docker ---
docker build -t myapp:latest .
docker run -d -p 8080:80 --name myapp myapp:latest
docker ps -a
docker logs -f myapp
docker exec -it myapp /bin/bash
docker stop myapp && docker rm myapp
docker images
docker system prune -af

# Docker Compose
docker compose up -d
docker compose down
docker compose logs -f

# --- Kubernetes (kubectl) ---
kubectl get pods -A
kubectl describe pod mypod
kubectl logs -f mypod
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
kubectl exec -it mypod -- /bin/sh
kubectl get svc
kubectl scale deployment myapp --replicas=3
kubectl rollout restart deployment myapp
```

---

## 📄 License
This handbook is released under the [MIT License](https://opensource.org/licenses/MIT) — free to use, copy, and share.

