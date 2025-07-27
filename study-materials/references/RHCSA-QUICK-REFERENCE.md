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

### **DNF (Dandified YUM) - Package Manager**

**What is DNF:** Modern package manager for RPM-based systems, successor to YUM.

**Key Concepts:**
- **Package:** Software bundle with metadata (dependencies, version, etc.)
- **Repository:** Collection of packages available for installation
- **Transaction:** Group of package operations performed together
- **Dependencies:** Other packages required for a package to work

```bash
# Package Discovery and Information
dnf search package           # Search for packages by name/description
# What happens: Searches package names and descriptions across all enabled repos
# Returns: package names with brief descriptions
# Useful when you know what you want but not the exact package name

dnf info package            # Show detailed package information
# Shows: version, size, dependencies, description, repository
# What happens: Queries package metadata from repositories
# Useful for understanding what a package does before installing

dnf provides */filename     # Find which package provides a file
# What happens: Searches all packages for files matching the pattern
# Useful when you need a specific command/file but don't know the package

# Package Installation and Removal
dnf install package         # Install package and dependencies
# What happens: Downloads package and all required dependencies
# Installs packages in correct order to satisfy dependencies
# Updates RPM database and runs post-install scripts

dnf install -y package      # Install without confirmation prompts
# What happens: Automatically answers 'yes' to all prompts
# Useful for scripting and automation

dnf remove package          # Remove package (keeps dependencies)
# What happens: Removes package but leaves dependencies installed
# Other packages might still need those dependencies

dnf autoremove              # Remove orphaned dependencies
# What happens: Removes packages that were installed as dependencies
# but are no longer needed by any installed package

# Package Updates
dnf update                  # Update all installed packages
# What happens: Downloads and installs newer versions of all packages
# Checks all enabled repositories for updates
# Can take significant time and bandwidth

dnf update package          # Update specific package only
# What happens: Updates only the specified package and its dependencies
# Faster and more targeted than full system update

dnf check-update            # Check for available updates (no installation)
# What happens: Lists packages that have updates available
# Useful for seeing what would be updated before running update

# Package Listing and History
dnf list installed          # List all installed packages
# Shows: package name, version, repository it came from
# Useful for inventory and troubleshooting

dnf list available          # List packages available for installation
# Shows: all packages in enabled repositories not currently installed
# Can be very long output

dnf history                 # Show transaction history
# Shows: transaction ID, date, action, packages affected
# Useful for troubleshooting and rollback

dnf history undo ID         # Undo a specific transaction
# What happens: Reverses the specified transaction
# Installs removed packages, removes installed packages

# Repository Management
dnf repolist                # List enabled repositories
# Shows: repository ID, name, status, number of packages
# Useful for verifying which repos are active

dnf repolist --all          # List all repositories (enabled and disabled)
# Shows: all configured repositories regardless of status

dnf config-manager --add-repo URL  # Add new repository
# What happens: Creates new .repo file in /etc/yum.repos.d/
# Repository becomes available for package operations

dnf config-manager --disable repo  # Disable repository
# What happens: Sets enabled=0 in repository configuration
# Packages from this repo won't be available for installation

dnf config-manager --enable repo   # Enable repository
# What happens: Sets enabled=1 in repository configuration

# Cache and Cleanup
dnf clean all               # Clean all cached data
# What happens: Removes downloaded packages, metadata, and cache files
# Frees disk space but next operations will be slower

dnf makecache               # Download and cache metadata
# What happens: Downloads repository metadata for faster searches
# Useful after adding new repositories
```

**Common DNF Workflows:**
```bash
# Install development tools
dnf groupinstall "Development Tools"

# Search and install unknown package
dnf search web server
dnf info httpd
dnf install httpd

# System maintenance
dnf check-update
dnf update
dnf autoremove
dnf clean all
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

### **Systemd Services (System and Service Manager)**

**What is systemd:** Modern init system that manages services, targets, and system state.

**Key Concepts:**
- **Units:** Basic objects that systemd manages (services, targets, mounts, etc.)
- **Targets:** Groups of units that define system states (like runlevels)
- **Dependencies:** Units can depend on other units

```bash
# Service Management
systemctl start service     # Start a service immediately
# What happens: Executes the service's ExecStart command
# Service runs until stopped or fails

systemctl stop service      # Stop a running service
# What happens: Sends SIGTERM, then SIGKILL if needed
# Executes ExecStop command if defined

systemctl restart service   # Stop and start service
# What happens: Combines stop + start operations
# Useful when configuration files have changed

systemctl reload service    # Reload service configuration
# What happens: Sends SIGHUP to service (if supported)
# Service stays running but re-reads config
# Not all services support reload

systemctl enable service    # Enable service to start at boot
# What happens: Creates symlinks in /etc/systemd/system/
# Service will start automatically during boot process

systemctl disable service   # Disable service from starting at boot
# What happens: Removes symlinks from /etc/systemd/system/
# Service won't start automatically but can be started manually

systemctl status service    # Show detailed service status
# Shows: active state, process info, recent log entries
# Very useful for troubleshooting service issues

systemctl is-active service # Check if service is currently running
# Returns: active, inactive, or failed

systemctl is-enabled service # Check if service is enabled for boot
# Returns: enabled, disabled, or static

# System Targets (Runlevels)
systemctl get-default       # Show default boot target
# Common targets:
# - graphical.target (GUI desktop)
# - multi-user.target (command line with networking)
# - rescue.target (single user mode)

systemctl set-default multi-user.target  # Set default boot target
# What happens: Changes /etc/systemd/system/default.target symlink
# System will boot to this target by default

systemctl isolate rescue.target          # Switch to rescue mode immediately
# What happens: Stops all services not required by rescue.target
# Useful for system maintenance

# Advanced Service Operations
systemctl mask service      # Completely disable service (cannot be started)
# What happens: Links service to /dev/null
# Prevents accidental or dependency-based activation

systemctl unmask service    # Remove mask from service
# What happens: Removes /dev/null link, service can be used again

systemctl daemon-reload     # Reload systemd configuration
# What happens: Re-reads all unit files
# Required after creating/modifying unit files
```

**Service States Explained:**
- **active (running):** Service is currently running
- **active (exited):** Service completed successfully (one-time tasks)
- **inactive (dead):** Service is not running
- **failed:** Service failed to start or crashed
- **activating:** Service is in the process of starting
- **deactivating:** Service is in the process of stopping

**Common Systemd Directories:**
```bash
/etc/systemd/system/        # Custom and override unit files
/usr/lib/systemd/system/    # Distribution-provided unit files
/run/systemd/system/        # Runtime unit files
```

**Useful Service Investigation Commands:**
```bash
systemctl list-units --type=service    # List all loaded services
systemctl list-units --failed          # List failed services
systemctl list-unit-files --type=service # List all service files
journalctl -u service                   # View service logs
journalctl -u service --since today     # View today's service logs
```systemctl list-units --type=service  # List all services
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

### **LVM (Logical Volume Management)**

**What is LVM:** A flexible disk management system that allows dynamic resizing of storage.

**LVM Hierarchy:** Physical Volumes → Volume Groups → Logical Volumes

```bash
# Physical Volumes (PVs) - Raw storage devices
pvcreate /dev/sdb1          # Create physical volume
# What happens: Writes LVM metadata to the device, making it usable by LVM
# The device is now ready to be added to a volume group

pvdisplay                   # Show detailed physical volume information
pvs                         # Show PV summary (size, VG assignment, etc.)

# Volume Groups (VGs) - Pool of storage from multiple PVs
vgcreate vg_name /dev/sdb1  # Create volume group from one or more PVs
# What happens: Creates a storage pool that can span multiple physical devices
# You can now create logical volumes from this pool

vgextend vg_name /dev/sdc1  # Add another PV to extend the volume group
# What happens: Increases the total storage pool size
# Existing logical volumes can now be extended with the new space

vgdisplay                   # Show detailed volume group information
vgs                         # Show VG summary (size, free space, PV count)

# Logical Volumes (LVs) - Virtual partitions created from VG space
lvcreate -L 1G -n lv_name vg_name  # Create 1GB logical volume
# What happens: Allocates space from the volume group
# Creates a block device at /dev/vg_name/lv_name
# This device can be formatted and mounted like a regular partition

lvextend -L +500M /dev/vg/lv        # Extend logical volume by 500MB
# What happens: Allocates additional space from the volume group
# The filesystem must be resized separately (resize2fs for ext4, xfs_growfs for XFS)

lvdisplay                           # Show detailed logical volume information
lvs                                 # Show LV summary (size, VG, attributes)
```

**LVM Advantages:**
- **Dynamic resizing:** Grow/shrink volumes without unmounting
- **Spanning devices:** One logical volume can span multiple physical disks
- **Snapshots:** Create point-in-time copies for backups
- **Migration:** Move data between physical devices online

**Common LVM Workflow:**
```bash
# 1. Prepare physical storage
pvcreate /dev/sdb1 /dev/sdc1

# 2. Create volume group
vgcreate my_vg /dev/sdb1 /dev/sdc1

# 3. Create logical volumes
lvcreate -L 2G -n data_lv my_vg
lvcreate -L 1G -n logs_lv my_vg

# 4. Create filesystems
mkfs.ext4 /dev/my_vg/data_lv
mkfs.xfs /dev/my_vg/logs_lv

# 5. Mount and use
mount /dev/my_vg/data_lv /data
mount /dev/my_vg/logs_lv /logs
```

### **File Systems (Storage Layer)**

**What are filesystems:** Data structures that organize and manage files on storage devices.

**Common RHEL Filesystems:**
- **ext4:** Default Linux filesystem, supports journaling, good performance
- **XFS:** High-performance filesystem, excellent for large files, default in RHEL 7+
- **VFAT:** Windows-compatible filesystem for USB drives

```bash
# Creating Filesystems
mkfs.ext4 /dev/sdb1         # Create ext4 filesystem
# What happens: Writes filesystem metadata (superblock, inodes, journal)
# Creates directory structure and prepares for file storage
# Supports files up to 16TB, volumes up to 1EB

mkfs.xfs /dev/sdb1          # Create XFS filesystem
# What happens: Creates XFS metadata structures
# Optimized for large files and high-performance I/O
# Cannot be shrunk (only grown)

mkfs.vfat /dev/sdb1         # Create VFAT (FAT32) filesystem
# What happens: Creates FAT table and root directory
# Compatible with Windows, limited to 4GB file size
# Useful for USB drives and EFI boot partitions

# Mount Operations
mount /dev/sdb1 /mnt        # Mount filesystem to directory
# What happens: Attaches filesystem to directory tree
# Makes files accessible through /mnt path
# Kernel reads superblock to understand filesystem structure

umount /mnt                 # Unmount filesystem
# What happens: Flushes cached data to disk
# Detaches filesystem from directory tree
# Ensures data integrity before removal

mount -a                    # Mount all filesystems in /etc/fstab
# What happens: Reads /etc/fstab and mounts all entries
# Useful during boot or after fstab changes
# Skips already mounted filesystems

findmnt                     # Show all mounted filesystems in tree format
# Shows: mount point, filesystem type, device, mount options
# More detailed than 'mount' command

findmnt /                   # Show specific mount point details
# Useful for checking mount options and filesystem type

# Filesystem Maintenance
fsck /dev/sdb1              # Check and repair filesystem
# What happens: Scans filesystem for errors and inconsistencies
# Can repair minor issues automatically
# NEVER run on mounted filesystems (data corruption risk)

fsck -f /dev/sdb1           # Force check even if filesystem appears clean
# What happens: Performs thorough check regardless of clean flag
# Useful after system crashes or power failures

# Filesystem Information
tune2fs -l /dev/sdb1        # Show ext2/3/4 filesystem information
# Shows: block size, inode count, mount count, last check time
# Useful for monitoring filesystem health

xfs_info /dev/sdb1          # Show XFS filesystem information
# Shows: block size, inode size, log information
# Must be run on mounted XFS filesystem

# Filesystem Resizing
resize2fs /dev/sdb1         # Resize ext2/3/4 filesystem
# What happens: Adjusts filesystem size to match partition size
# Can grow or shrink (shrinking requires unmounting)
# Must resize partition first, then filesystem

xfs_growfs /mnt             # Grow XFS filesystem
# What happens: Expands XFS filesystem to fill available space
# Can only grow (never shrink) XFS filesystems
# Must be run on mounted filesystem
# Partition must be expanded first
```

**Filesystem Resize Workflow:**
```bash
# For ext4 (can grow/shrink):
# 1. Unmount filesystem (for shrinking)
umount /mnt
# 2. Check filesystem
fsck -f /dev/sdb1
# 3. Resize partition (using fdisk/parted)
# 4. Resize filesystem
resize2fs /dev/sdb1
# 5. Remount
mount /dev/sdb1 /mnt

# For XFS (can only grow):
# 1. Resize partition first (using fdisk/parted)
# 2. Grow filesystem (while mounted)
xfs_growfs /mnt
```

**Important /etc/fstab Format:**
```bash
# Device/UUID    Mount Point    FS Type    Options    Dump    Pass
/dev/sdb1       /data          ext4       defaults   0       2
UUID=abc123     /backup        xfs        defaults   0       2
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

### **NetworkManager (Network Configuration)**

**What is NetworkManager:** Modern network management daemon that handles network connections dynamically.

**Key Concepts:**
- **Device:** Physical/virtual network interface (eth0, wlan0, etc.)
- **Connection:** Configuration profile with IP, DNS, routes settings
- **Profile:** Stored connection that can be activated on devices

```bash
# Connection Management
nmcli con show              # Show all connection profiles
# Shows: NAME, UUID, TYPE, DEVICE (device shows if active)
# Active connections display which device they're using

nmcli con show "connection-name"  # Show detailed connection settings
# Shows: IP configuration, DNS servers, routes, security settings
# Useful for troubleshooting network issues

nmcli dev status            # Show all network devices and their state
# Shows: DEVICE, TYPE, STATE, CONNECTION
# States: connected, disconnected, unavailable, unmanaged
# Helps identify which devices are available

# Creating Network Connections
nmcli con add type ethernet con-name mycon ifname eth0  # Add basic ethernet connection
# What happens: Creates new connection profile with DHCP by default
# Connection is stored in /etc/NetworkManager/system-connections/
# Can be activated immediately or later

# Configuring Static IP
nmcli con mod mycon ipv4.addresses 192.168.1.100/24    # Set static IP address
# What happens: Configures IP address with /24 subnet mask
# Format: IP/prefix (e.g., 192.168.1.100/24 = 255.255.255.0)

nmcli con mod mycon ipv4.gateway 192.168.1.1           # Set default gateway
# What happens: Sets route for traffic to other networks
# Gateway must be on same subnet as IP address

nmcli con mod mycon ipv4.dns "8.8.8.8 8.8.4.4"        # Set DNS servers
# What happens: Configures DNS resolution servers
# Multiple DNS servers separated by spaces
# Updates /etc/resolv.conf when connection is active

nmcli con mod mycon ipv4.method manual                  # Set manual IP configuration
# What happens: Disables DHCP, uses static configuration
# Required when setting static IP addresses
# Other methods: auto (DHCP), disabled, link-local

# Connection Activation
nmcli con up mycon          # Activate connection profile
# What happens: Applies connection settings to device
# Brings interface up with configured IP, DNS, routes
# Deactivates any existing connection on the device

nmcli con down mycon        # Deactivate connection
# What happens: Brings down the network interface
# Removes IP address, routes, and DNS settings
# Device becomes available for other connections

# Advanced Connection Management
nmcli con reload            # Reload connection files from disk
# What happens: Re-reads connection files after manual editing
# Useful after modifying files in /etc/NetworkManager/system-connections/

nmcli con delete mycon      # Delete connection profile
# What happens: Removes connection file permanently
# Connection cannot be activated anymore
```

**Complete Static IP Setup Example:**
```bash
# 1. Create connection with static IP
nmcli con add type ethernet con-name static-eth ifname eth0 \
  ipv4.addresses 192.168.1.100/24 \
  ipv4.gateway 192.168.1.1 \
  ipv4.dns "8.8.8.8 8.8.4.4" \
  ipv4.method manual

# 2. Activate the connection
nmcli con up static-eth

# 3. Verify configuration
ip addr show eth0
ip route show
cat /etc/resolv.conf
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

### **SELinux (Security Enhanced Linux)**

**What is SELinux:** Mandatory Access Control (MAC) system that provides fine-grained security policies.

**SELinux Modes:**
- **Enforcing:** SELinux policies are enforced (blocks unauthorized access)
- **Permissive:** SELinux policies are not enforced but violations are logged
- **Disabled:** SELinux is completely turned off

```bash
# SELinux Status and Mode Management
getenforce                  # Show current SELinux mode (Enforcing/Permissive/Disabled)
# Returns: Enforcing, Permissive, or Disabled

setenforce 0                # Set permissive mode (temporarily)
# What happens: Policies are not enforced but violations are logged
# Useful for troubleshooting - if something works in permissive, SELinux is the issue

setenforce 1                # Set enforcing mode (temporarily)
# What happens: All SELinux policies are actively enforced
# This is the secure production mode

sestatus                    # Show detailed SELinux status
# Shows: current mode, mode from config file, policy version, etc.

# SELinux Contexts (Labels)
ls -Z                       # Show SELinux contexts of files
# Format: user:role:type:level (e.g., unconfined_u:object_r:admin_home_t:s0)
# - user: SELinux user identity
# - role: SELinux role
# - type: SELinux type (most important for file access)
# - level: MLS/MCS level (Multi-Level Security)

chcon -t httpd_exec_t /path/script.sh  # Change SELinux type context
# What happens: Temporarily changes the SELinux type of the file
# WARNING: Changes are lost if filesystem is relabeled

restorecon -R /path         # Restore default SELinux contexts
# What happens: Resets SELinux contexts to default values based on policy
# Use this after moving files or when contexts are incorrect
# -R flag applies recursively to directories

# SELinux Booleans (Policy Switches)
getsebool -a                # Show all SELinux booleans and their values
# Booleans are on/off switches that modify SELinux policy behavior

getsebool httpd_can_network_connect  # Check specific boolean
# Returns: on or off

setsebool httpd_can_network_connect on     # Set boolean temporarily
# What happens: Enables the boolean for current session only
# Lost after reboot

setsebool -P httpd_can_network_connect on  # Set boolean permanently
# What happens: Enables the boolean and saves to policy
# -P flag makes the change persistent across reboots
```

**SELinux Context Management:**
```bash
# Permanent context changes (survives relabeling)
semanage fcontext -a -t httpd_exec_t "/opt/myapp(/.*)?"  # Add context rule
restorecon -R /opt/myapp                                # Apply the rule

# Port labeling
semanage port -l | grep http                           # List HTTP port labels
semanage port -a -t http_port_t -p tcp 8080           # Add port label
```

**Common SELinux Troubleshooting:**
```bash
# Check for SELinux denials
ausearch -m AVC -ts recent                             # Search audit log for denials
sealert -a /var/log/audit/audit.log                    # Analyze SELinux alerts

# Generate policy modules (when needed)
audit2allow -a                                         # Generate policy from denials
audit2allow -a -M mypolicy                            # Create policy module
semodule -i mypolicy.pp                               # Install policy module
```

**SELinux Best Practices:**
- **Never disable SELinux** in production
- **Use permissive mode** for troubleshooting only
- **Always use restorecon** after moving files
- **Use semanage** for permanent context changes
- **Check audit logs** when applications fail

**Understanding SELinux Contexts:**
- **httpd_exec_t:** Executable files for Apache
- **httpd_config_t:** Apache configuration files
- **admin_home_t:** Administrator home directories
- **user_home_t:** Regular user home directories
- **tmp_t:** Temporary files

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

### **Emergency Recovery (Password Recovery)**

**When to use:** When you've forgotten the root password or need emergency system access.

**Step-by-Step Process:**

```bash
# Step 1: Boot and interrupt GRUB
# - Reboot the system
# - When GRUB menu appears, press 'e' to edit the boot entry
# - This gives you access to edit kernel parameters before boot

# Step 2: Add 'rd.break' to kernel line
# - Find the line starting with 'linux' (contains kernel path)
# - Navigate to the end of that line
# - Add: rd.break
# - Press Ctrl+X to boot with modified parameters
# - This breaks into the initramfs emergency shell BEFORE the real root filesystem is mounted

# Step 3: Remount /sysroot as read-write
mount -o remount,rw /sysroot
# Why: /sysroot contains the real root filesystem, but it's mounted read-only
# This command remounts it as read-write so we can make changes

# Step 4: Change root to the real filesystem
chroot /sysroot
# Why: This changes our root directory from initramfs to the actual system
# Now we're operating in the real system environment

# Step 5: Change the root password
passwd root
# Enter new password twice when prompted
# This updates /etc/shadow with the new password hash

# Step 6: Create SELinux relabel file (CRITICAL if SELinux is enabled)
touch /.autorelabel
# Why: Changing passwd modified /etc/shadow, which changes SELinux contexts
# This file tells SELinux to relabel all files on next boot
# Without this, SELinux may prevent login even with correct password

# Step 7: Exit and reboot
exit        # Exit chroot environment
exit        # Exit initramfs shell
# System will reboot and relabel files if SELinux is enabled
```

**What happens behind the scenes:**

1. **rd.break parameter:** Interrupts the boot process in initramfs (initial RAM filesystem)
2. **initramfs:** Temporary filesystem loaded into memory during boot, contains essential tools
3. **/sysroot:** Mount point where the real root filesystem is attached during boot
4. **chroot:** Changes the apparent root directory, making /sysroot appear as /
5. **SELinux relabeling:** Ensures file security contexts are correct after password change

**Alternative emergency parameters:**
```bash
# Other useful emergency boot parameters:
init=/bin/bash              # Boot directly to bash shell (bypasses systemd)
systemd.unit=rescue.target  # Boot to rescue mode (single user)
systemd.unit=emergency.target # Boot to emergency mode (minimal system)
selinux=0                   # Disable SELinux for this boot
enforcing=0                 # Set SELinux to permissive mode
```

**Important Notes:**
- **Always use rd.break** for password recovery (safest method)
- **Never skip the .autorelabel step** if SELinux is enabled
- **This works on RHEL/CentOS/Fedora** systems using systemd
- **Physical access required** - this is a security feature, not a vulnerability

---

*This quick reference covers all essential commands from both AG Guide (22 chapters) and Red Hat Official Guide (28 chapters). Perfect for exam day review and daily practice!*
