# RHCSA Quick Reference - Essential Commands

## File Operations
```bash
# Navigation
pwd                     # Print working directory
ls -la                  # List files with details
cd /path/to/dir        # Change directory
find /path -name "*.txt" # Find files

# File Management
cp source dest          # Copy files
mv source dest          # Move/rename files
rm -rf directory       # Remove directory recursively
chmod 755 file         # Change permissions
chown user:group file  # Change ownership
```

## User Management
```bash
# User Operations
useradd -m username     # Create user with home directory
usermod -aG group user  # Add user to group
passwd username         # Set password
userdel -r username     # Delete user and home directory

# Group Operations
groupadd groupname      # Create group
gpasswd -a user group   # Add user to group
id username             # Show user info
```

## System Services
```bash
# Service Management
systemctl start service     # Start service
systemctl stop service      # Stop service
systemctl enable service    # Enable at boot
systemctl status service    # Check status
systemctl list-units       # List all units

# Journal Logs
journalctl -u service       # Show service logs
journalctl -f              # Follow logs
journalctl --since "1 hour ago" # Recent logs
```

## Networking
```bash
# Network Configuration
ip addr show               # Show IP addresses
nmcli connection show      # Show connections
nmcli con add type ethernet con-name static ifname eth0
nmcli con mod static ipv4.addresses 192.168.1.100/24
nmcli con up static        # Activate connection

# Firewall
firewall-cmd --list-all    # Show firewall rules
firewall-cmd --add-service=http --permanent
firewall-cmd --reload      # Reload rules
```

## Storage Management
```bash
# Partitioning
fdisk -l                   # List partitions
parted /dev/sdb mklabel gpt # Create GPT table
lsblk                      # Show block devices

# LVM
pvcreate /dev/sdb          # Create physical volume
vgcreate vg_name /dev/sdb  # Create volume group
lvcreate -L 1G -n lv_name vg_name # Create logical volume
lvextend -L +1G /dev/vg/lv # Extend logical volume
```

## File Systems
```bash
# File System Operations
mkfs.ext4 /dev/sdb1        # Create ext4 filesystem
mkfs.xfs /dev/sdb1         # Create XFS filesystem
mount /dev/sdb1 /mnt       # Mount filesystem
umount /mnt                # Unmount filesystem
df -h                      # Show disk usage
```

## Package Management
```bash
# DNF/YUM Operations
dnf search package         # Search packages
dnf install package        # Install package
dnf remove package         # Remove package
dnf update                 # Update all packages
dnf group install "Group Name" # Install package group
```

## SELinux
```bash
# SELinux Management
getenforce                 # Show SELinux mode
setenforce 1               # Set enforcing mode
ls -Z                      # Show SELinux contexts
chcon -t type file         # Change context
restorecon file            # Restore default context
setsebool -P boolean on    # Set SELinux boolean
```

## Process Management
```bash
# Process Operations
ps aux                     # List all processes
top                        # Show running processes
kill -9 PID               # Kill process by PID
killall process_name       # Kill by process name
jobs                       # Show background jobs
bg %1                      # Send job to background
```

## Scheduling
```bash
# Cron Jobs
crontab -e                 # Edit cron jobs
crontab -l                 # List cron jobs
# Format: min hour day month weekday command
0 2 * * * /path/to/script  # Daily at 2 AM

# At Jobs
at 15:30                   # Schedule for 3:30 PM
atq                        # List at jobs
atrm job_number           # Remove at job
```

## Boot Process
```bash
# GRUB and Boot
grub2-mkconfig -o /boot/grub2/grub.cfg # Regenerate GRUB
systemctl get-default      # Show default target
systemctl set-default multi-user.target # Set default target
systemctl isolate rescue.target # Switch to rescue mode
```

## Container Management (Podman)
```bash
# Container Operations
podman pull image          # Pull image
podman run -d --name container image # Run container
podman ps                  # List running containers
podman stop container      # Stop container
podman rm container        # Remove container
podman images              # List images
```

## Archive and Compression
```bash
# Archive Operations
tar -czf archive.tar.gz /path # Create compressed archive
tar -xzf archive.tar.gz    # Extract compressed archive
gzip file                  # Compress file
gunzip file.gz             # Decompress file
```

## Text Processing
```bash
# Text Operations
grep pattern file          # Search for pattern
sed 's/old/new/g' file    # Replace text
awk '{print $1}' file     # Print first column
sort file                  # Sort lines
uniq file                  # Remove duplicates
```

## System Information
```bash
# System Info
uname -a                   # System information
free -h                    # Memory usage
df -h                      # Disk usage
lscpu                      # CPU information
uptime                     # System uptime
```

## SSH and Remote Access
```bash
# SSH Operations
ssh user@host              # Connect to remote host
ssh-keygen -t rsa          # Generate SSH key
ssh-copy-id user@host      # Copy public key
scp file user@host:/path   # Copy file over SSH
```

## Emergency Commands
```bash
# System Recovery
mount -o remount,rw /      # Remount root as read-write
chroot /sysroot            # Change root (in rescue mode)
passwd root                # Change root password
touch /.autorelabel        # Force SELinux relabel on boot
```

## Exam Day Reminders
- Read all tasks before starting
- Verify configurations after each task
- Test that services start at boot
- Check firewall and SELinux settings
- Make sure changes are persistent
- Use man pages for syntax help
- Manage your time effectively

## Common Exam Mistakes to Avoid
- Forgetting to enable services at boot
- Not making firewall changes permanent
- SELinux blocking configured services
- Incorrect file permissions
- Not testing configurations
- Typos in configuration files
- Not reading requirements carefully
