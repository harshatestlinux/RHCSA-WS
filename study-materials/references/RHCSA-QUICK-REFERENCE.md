# 🚀 RHCSA Quick Reference Guide
*Comprehensive command reference extracted from AG Guide + Red Hat Official Guide*

## 📋 **Essential System Commands**

### **System Information**
```bash
# System details
uname -a                    # All system information
hostnamectl                 # Hostname and system info
uptime                      # System uptime and load
who                         # Logged in users
id                          # Current user ID and groups
whoami                      # Current username
```

### **File Operations**
```bash
# Navigation and listing
ls -la                      # Detailed file listing
pwd                         # Current directory
cd /path/to/dir            # Change directory
find /path -name "file"     # Find files
locate filename             # Locate files (updatedb first)

# File manipulation
cp source dest              # Copy files
mv source dest              # Move/rename files
rm -rf /path                # Remove files/directories
mkdir -p /path/to/dir       # Create directories
touch filename              # Create empty file
```

### **File Permissions**
```bash
# Permission management
chmod 755 file              # Set permissions (octal)
chmod u+x,g-w,o-r file     # Set permissions (symbolic)
chown user:group file       # Change ownership
chgrp group file           # Change group ownership
umask 022                  # Set default permissions

# Special permissions
chmod u+s file             # Set SUID
chmod g+s dir              # Set SGID
chmod +t dir               # Set sticky bit
```

---

## 👥 **User and Group Management**

### **User Management**
```bash
# User operations
useradd -m -s /bin/bash user    # Create user with home
usermod -aG group user          # Add user to group
userdel -r user                 # Delete user and home
passwd user                     # Set user password
chage -l user                   # Show password aging
chage -E 2024-12-31 user       # Set account expiry

# User information
id user                         # User ID and groups
getent passwd user              # User account info
last                           # Login history
```

### **Group Management**
```bash
# Group operations
groupadd groupname              # Create group
groupmod -n newname oldname     # Rename group
groupdel groupname              # Delete group
gpasswd -a user group          # Add user to group
gpasswd -d user group          # Remove user from group
```

### **sudo Configuration**
```bash
# sudo management
visudo                         # Edit sudoers file
usermod -aG wheel user         # Add user to wheel group
sudo -l                        # List sudo privileges
sudo -u user command           # Run as different user
```

---

## 🐚 **Bash Shell**

### **Environment and Variables**
```bash
# Environment
env                            # Show environment variables
export VAR=value               # Set environment variable
unset VAR                      # Remove variable
echo $PATH                     # Show PATH variable
which command                  # Show command location
type command                   # Show command type
```

### **Command History**
```bash
# History management
history                        # Show command history
!!                            # Repeat last command
!n                            # Repeat command number n
!string                       # Repeat last command starting with string
Ctrl+R                        # Search command history
```

### **Aliases and Functions**
```bash
# Aliases
alias ll='ls -la'             # Create alias
unalias ll                    # Remove alias
alias                         # Show all aliases

# I/O Redirection
command > file                # Redirect stdout
command >> file               # Append stdout
command 2> file               # Redirect stderr
command &> file               # Redirect both
command | command2            # Pipe output
```

---

## ⚙️ **Process Management**

### **Process Control**
```bash
# Process information
ps aux                        # Show all processes
ps -ef                        # Show all processes (different format)
pgrep process_name            # Find process by name
pidof process_name            # Get PID of process
top                          # Real-time process monitor
htop                         # Enhanced process monitor

# Process control
kill PID                     # Terminate process
kill -9 PID                  # Force kill process
killall process_name         # Kill all processes by name
pkill process_name           # Kill processes by name
nohup command &              # Run command immune to hangups
```

### **Job Control**
```bash
# Job management
jobs                         # Show active jobs
bg %1                        # Put job 1 in background
fg %1                        # Bring job 1 to foreground
Ctrl+Z                       # Suspend current job
command &                    # Run command in background
```

### **Scheduling**
```bash
# Cron jobs
crontab -e                   # Edit user crontab
crontab -l                   # List user crontab
crontab -r                   # Remove user crontab
# Format: min hour day month weekday command
# Example: 0 2 * * * /backup.sh

# At jobs
at 14:30                     # Schedule job for 2:30 PM
at now + 1 hour             # Schedule job for 1 hour from now
atq                         # List scheduled jobs
atrm job_number             # Remove scheduled job
```

---

## 📦 **Package Management**

### **DNF (YUM)**
```bash
# Package operations
dnf search package           # Search for package
dnf info package            # Show package information
dnf install package         # Install package
dnf remove package          # Remove package
dnf update                  # Update all packages
dnf update package          # Update specific package
dnf list installed          # List installed packages
dnf list available          # List available packages

# Repository management
dnf repolist                # List repositories
dnf config-manager --add-repo URL  # Add repository
dnf config-manager --disable repo  # Disable repository
dnf clean all               # Clean package cache
```

### **RPM**
```bash
# RPM operations
rpm -qa                     # List all installed packages
rpm -qi package            # Show package information
rpm -ql package            # List package files
rpm -qf /path/to/file      # Find package owning file
rpm -ivh package.rpm       # Install RPM package
rpm -Uvh package.rpm       # Upgrade RPM package
rpm -e package             # Remove package
rpm -V package             # Verify package
```

---

## 🔧 **System Services (systemd)**

### **Service Management**
```bash
# Service control
systemctl start service     # Start service
systemctl stop service      # Stop service
systemctl restart service   # Restart service
systemctl reload service    # Reload service config
systemctl enable service    # Enable service at boot
systemctl disable service   # Disable service at boot
systemctl status service    # Show service status
systemctl is-active service # Check if service is active
systemctl is-enabled service # Check if service is enabled

# System targets
systemctl get-default       # Show default target
systemctl set-default multi-user.target  # Set default target
systemctl isolate rescue.target  # Switch to rescue mode
systemctl list-units --type=service  # List all services
```

### **Logging**
```bash
# Journal logs
journalctl                  # Show all logs
journalctl -u service       # Show logs for service
journalctl -f               # Follow logs (tail -f style)
journalctl --since "1 hour ago"  # Show recent logs
journalctl -p err           # Show error logs only
journalctl --disk-usage     # Show disk usage
journalctl --vacuum-time=1w # Clean logs older than 1 week
```

---

## 💾 **Storage Management**

### **Disk Information**
```bash
# Disk usage
df -h                       # Show filesystem usage
du -sh /path                # Show directory size
lsblk                       # List block devices
fdisk -l                    # List disk partitions
blkid                       # Show block device UUIDs
```

### **Partitioning**
```bash
# Partition management
fdisk /dev/sda              # Partition disk (MBR)
gdisk /dev/sda              # Partition disk (GPT)
parted /dev/sda             # Partition disk (parted)
partprobe                   # Update kernel partition table
```

### **LVM**
```bash
# Physical volumes
pvcreate /dev/sdb1          # Create physical volume
pvdisplay                   # Show physical volumes
pvs                         # Show PV summary

# Volume groups
vgcreate vg_name /dev/sdb1  # Create volume group
vgextend vg_name /dev/sdc1  # Extend volume group
vgdisplay                   # Show volume groups
vgs                         # Show VG summary

# Logical volumes
lvcreate -L 1G -n lv_name vg_name  # Create logical volume
lvextend -L +500M /dev/vg/lv        # Extend logical volume
lvdisplay                           # Show logical volumes
lvs                                 # Show LV summary
```

### **File Systems**
```bash
# Create filesystems
mkfs.ext4 /dev/sdb1         # Create ext4 filesystem
mkfs.xfs /dev/sdb1          # Create XFS filesystem
mkfs.vfat /dev/sdb1         # Create VFAT filesystem

# Mount operations
mount /dev/sdb1 /mnt        # Mount filesystem
umount /mnt                 # Unmount filesystem
mount -a                    # Mount all fstab entries
findmnt                     # Show mounted filesystems

# Filesystem tools
fsck /dev/sdb1              # Check filesystem
tune2fs -l /dev/sdb1        # Show ext filesystem info
xfs_info /dev/sdb1          # Show XFS filesystem info
resize2fs /dev/sdb1         # Resize ext filesystem
xfs_growfs /mnt             # Resize XFS filesystem
```

### **Swap**
```bash
# Swap management
mkswap /dev/sdb1            # Create swap space
swapon /dev/sdb1            # Enable swap
swapoff /dev/sdb1           # Disable swap
swapon --show               # Show swap usage
free -h                     # Show memory and swap usage
```

---

## 🌐 **Networking**

### **Network Configuration**
```bash
# IP configuration
ip addr show                # Show IP addresses
ip addr add 192.168.1.100/24 dev eth0  # Add IP address
ip route show               # Show routing table
ip route add default via 192.168.1.1   # Add default route
ip link show                # Show network interfaces
ip link set eth0 up         # Enable interface
```

### **NetworkManager**
```bash
# Connection management
nmcli con show              # Show connections
nmcli dev status            # Show device status
nmcli con add type ethernet con-name mycon ifname eth0  # Add connection
nmcli con mod mycon ipv4.addresses 192.168.1.100/24    # Set IP
nmcli con mod mycon ipv4.gateway 192.168.1.1           # Set gateway
nmcli con mod mycon ipv4.dns 8.8.8.8                   # Set DNS
nmcli con mod mycon ipv4.method manual                  # Set manual IP
nmcli con up mycon          # Activate connection
nmcli con down mycon        # Deactivate connection
```

### **Network Testing**
```bash
# Connectivity testing
ping -c 4 host              # Test connectivity
traceroute host             # Trace route to host
nslookup domain             # DNS lookup
dig domain                  # DNS lookup (detailed)
ss -tuln                    # Show listening ports
netstat -tuln               # Show listening ports (legacy)
```

### **Hostname**
```bash
# Hostname management
hostnamectl                 # Show hostname info
hostnamectl set-hostname name  # Set hostname
hostname                    # Show current hostname
```

---

## 🔒 **Security**

### **Firewall (firewalld)**
```bash
# Firewall management
firewall-cmd --state        # Check firewall status
firewall-cmd --list-all     # Show all rules
firewall-cmd --get-zones    # List zones
firewall-cmd --get-default-zone  # Show default zone
firewall-cmd --add-service=http  # Add service
firewall-cmd --add-port=8080/tcp  # Add port
firewall-cmd --reload       # Reload firewall
firewall-cmd --permanent    # Make changes permanent
```

### **SELinux**
```bash
# SELinux management
getenforce                  # Show SELinux status
setenforce 0                # Set permissive mode
setenforce 1                # Set enforcing mode
sestatus                    # Show detailed SELinux status
ls -Z                       # Show SELinux contexts
chcon -t type file          # Change SELinux context
restorecon -R /path         # Restore default contexts
setsebool -P boolean on     # Set SELinux boolean
getsebool -a                # Show all booleans
```

### **SSH**
```bash
# SSH operations
ssh user@host               # Connect to remote host
ssh-keygen -t rsa           # Generate SSH key pair
ssh-copy-id user@host       # Copy public key to remote host
scp file user@host:/path    # Copy file to remote host
sftp user@host              # Secure file transfer
```

---

## 📁 **File Systems and NFS**

### **NFS Client**
```bash
# NFS operations
showmount -e nfs-server     # Show NFS exports
mount -t nfs server:/path /mnt  # Mount NFS share
umount /mnt                 # Unmount NFS share
```

### **AutoFS**
```bash
# AutoFS configuration
systemctl enable --now autofs  # Enable AutoFS
# Edit /etc/auto.master and /etc/auto.indirect
systemctl reload autofs     # Reload AutoFS configuration
```

---

## ⏰ **Time Services**

### **Chrony (NTP)**
```bash
# Time synchronization
chronyc sources             # Show time sources
chronyc tracking            # Show sync status
timedatectl                 # Show time/date status
timedatectl set-timezone America/New_York  # Set timezone
timedatectl set-ntp true    # Enable NTP sync
```

---

## 📦 **Containers (Podman)**

### **Container Management**
```bash
# Basic operations
podman pull image           # Pull container image
podman run -d --name container image  # Run container
podman ps                   # List running containers
podman ps -a                # List all containers
podman stop container       # Stop container
podman start container      # Start container
podman rm container         # Remove container
podman images               # List images
podman rmi image            # Remove image

# Advanced operations
podman exec -it container /bin/bash  # Execute command in container
podman logs container       # Show container logs
podman inspect container    # Show container details
podman generate systemd --new --name container  # Generate systemd service
```

---

## 🔧 **Boot Process and GRUB**

### **GRUB2**
```bash
# GRUB management
grub2-editenv list          # Show GRUB environment
grub2-mkconfig -o /boot/grub2/grub.cfg  # Regenerate GRUB config
grub2-set-default 0         # Set default boot entry
grubby --default-kernel     # Show default kernel
grubby --info=ALL           # Show all kernel entries
```

### **Kernel Parameters**
```bash
# Common kernel parameters for troubleshooting:
# systemd.unit=rescue.target    - Boot to rescue mode
# systemd.unit=emergency.target - Boot to emergency mode
# rd.break                      - Break into initramfs
# init=/bin/bash               - Boot to bash shell
# selinux=0                    - Disable SELinux
```

---

## 📝 **Text Processing**

### **Essential Text Tools**
```bash
# Text manipulation
grep pattern file           # Search for pattern
grep -r pattern /path       # Recursive search
sed 's/old/new/g' file      # Replace text
awk '{print $1}' file       # Print first column
cut -d: -f1 /etc/passwd     # Cut fields
sort file                   # Sort lines
uniq file                   # Remove duplicates
wc -l file                  # Count lines
head -n 10 file             # Show first 10 lines
tail -n 10 file             # Show last 10 lines
tail -f file                # Follow file changes
```

---

## 🎯 **Exam Day Essentials**

### **Quick System Check**
```bash
# System status check
systemctl status            # Overall system status
df -h                       # Disk space
free -h                     # Memory usage
uptime                      # System load
journalctl -p err --since today  # Today's errors
```

### **Emergency Recovery**
```bash
# Password recovery steps:
# 1. Boot and interrupt GRUB
# 2. Add 'rd.break' to kernel line
# 3. mount -o remount,rw /sysroot
# 4. chroot /sysroot
# 5. passwd root
# 6. touch /.autorelabel (if SELinux enabled)
# 7. exit and reboot
```

---

*This quick reference covers all essential commands from both AG Guide (22 chapters) and Red Hat Official Guide (28 chapters). Perfect for exam day review and daily practice!*
