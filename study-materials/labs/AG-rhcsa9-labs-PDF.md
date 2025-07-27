# 📖 RHCSA 9 Labs - AG Guide (PDF)
*Based on "RHCSA 9 Training and Exam Preparation Guide (EX200) Third Edition"*

## 🎯 **Source Information**
- **PDF:** RHCSA 9 Training and Exam Preparation Guide (EX200) Third Edition
- **Author:** [Content extracted from comprehensive guide]
- **Pages:** 1546 pages (comprehensive coverage)
- **Chapters:** 22 chapters + 4 sample exams (Appendices A-D)
- **Focus:** Performance-based RHCSA exam preparation with extensive hands-on exercises
- **Target:** RHCSA EX200 certification exam
- **Approach:** Theoretical concepts + practical implementation + real-world scenarios

## 📋 **Complete Chapter Structure**

**Part I: Foundation**
1. Building a Lab Environment
2. Initial Interaction with the System  
3. File and Directory Operations
4. File Permissions
5. User Account Management
6. Group Account Management and sudo

**Part II: System Operations**
7. The Bash Shell
8. Processes and Job Scheduling
9. Basic Package Management
10. Advanced Package Management
11. Boot Process, GRUB2, and the Kernel
12. System Initialization, Message Logging, and System Tuning

**Part III: Storage and File Systems**
13. Storage Management
14. Local File Systems and Swap

**Part IV: Networking and Services**
15. Networking, Network Devices, and Network Connections
16. Network File Systems
17. Time Services
18. The Secure Shell Service

**Part V: Security and Advanced Topics**
19. The Linux Firewall
20. Security Enhanced Linux
21. Shell Scripting
22. Containers

**Appendices:**
- Appendix A: Sample RHCSA Exam 1
- Appendix B: Sample RHCSA Exam 2  
- Appendix C: Sample RHCSA Exam 3
- Appendix D: Sample RHCSA Exam 4

## 🎯 **Study Approach**
- **Time Duration:** 3 hours per exam simulation
- **Passing Score:** 70% (210 out of 300 points)
- **Format:** Performance-based tasks on RHEL 9 virtual machines
- **Practice:** Hands-on exercises in each chapter
- **Verification:** Command validation and result checking

---

## 📚 **Chapter 1: Building a Lab Environment**

### **Lab 1.1: VirtualBox and RHEL 9 Setup**
**Objective:** Set up lab environment for RHCSA practice
**Time:** 60 minutes

**Tasks:**
1. Download and install VirtualBox Manager
2. Create Red Hat Developer account
3. Download RHEL 9 ISO image
4. Create virtual machines (server1, server2)
5. Install RHEL 9 on both VMs
6. Configure network settings
7. Set up SSH access between systems

**Key Commands:**
```bash
# Network configuration
nmcli con show
nmcli con mod "System eth0" ipv4.addresses 192.168.1.10/24
nmcli con up "System eth0"

# SSH setup
systemctl enable --now sshd
ssh-keygen -t rsa
ssh-copy-id user@server2
```

---

## 📚 **Chapter 2: Initial Interaction with the System**

### **Lab 2.1: Essential System Commands**
**Objective:** Master basic Linux commands and navigation
**Time:** 45 minutes

**Tasks:**
1. Log in to system using different methods
2. Navigate directory structure
3. Use man pages and documentation
4. Practice basic file operations
5. Understand command syntax and options

**Key Commands:**
```bash
# Basic navigation
pwd
ls -la
cd /etc
cd ~
cd -

# Documentation
man ls
info coreutils
ls --help
whatis ls
apropos "list directory"

# File operations
touch file1.txt
cp file1.txt file2.txt
mv file2.txt /tmp/
rm /tmp/file2.txt
```

---

## 📚 **Chapter 3: Essential File Management Tools**

### **Lab 3.1: File and Directory Operations**
**Objective:** Master file management, compression, and archiving
**Time:** 60 minutes

**Tasks:**
1. Create and manage files and directories
2. Use file compression tools (gzip, bzip2)
3. Create and extract tar archives
4. Work with hard and soft links
5. Use text editors (vi/vim, nano)

**Key Commands:**
```bash
# File creation and management
mkdir -p /tmp/testdir/{subdir1,subdir2}
touch /tmp/testdir/file{1..5}.txt
cp -r /tmp/testdir /tmp/backup

# Compression
gzip file1.txt
gunzip file1.txt.gz
bzip2 file2.txt
bunzip2 file2.txt.bz2

# Archiving
tar -czf backup.tar.gz /tmp/testdir
tar -xzf backup.tar.gz
tar -tf backup.tar.gz

# Links
ln file1.txt hardlink.txt
ln -s file1.txt softlink.txt
ls -li
```

---

## 📚 **Chapter 4: File Permissions**

### **Lab 4.1: Standard and Special Permissions**
**Objective:** Configure file permissions and access controls
**Time:** 45 minutes

**Tasks:**
1. Understand and modify standard permissions (ugo/rwx)
2. Work with numeric permission notation
3. Configure special permissions (setuid, setgid, sticky bit)
4. Use umask to set default permissions
5. Find files with specific permissions

**Key Commands:**
```bash
# Standard permissions
chmod 755 script.sh
chmod u+x,g+r,o-w file.txt
chmod a+r file.txt

# Special permissions
chmod u+s /usr/bin/passwd  # setuid
chmod g+s /tmp/shared      # setgid
chmod +t /tmp              # sticky bit
chmod 4755 file            # setuid + 755
chmod 2755 dir             # setgid + 755
chmod 1755 dir             # sticky + 755

# Default permissions
umask 022
umask -S

# Finding files
find /usr -perm 4755 -type f  # setuid files
find /tmp -perm 1777 -type d  # sticky directories
```

---

## 📚 **Chapter 5: User Account Management**

### **Lab 5.1: User Creation and Management (Enhanced)**
**Objective:** Create and manage user accounts with comprehensive understanding
**Time:** 60 minutes (extended for detailed practice)

**Learning Goals:**
- Understand user account structure and components
- Master user creation with various options
- Learn account modification and security settings
- Practice password policies and account expiration
- Understand the difference between system and regular users

**Background Knowledge:**
- **User ID (UID):** Unique numeric identifier for each user
- **Group ID (GID):** Primary group identifier for the user
- **Home Directory:** User's personal directory (usually /home/username)
- **Shell:** Command interpreter assigned to the user
- **Account Files:** /etc/passwd, /etc/shadow, /etc/group, /etc/gshadow

**Tasks with Detailed Explanations:**

**1. Create user accounts with useradd**
```bash
# Basic user creation
useradd -m -s /bin/bash john
# What happens:
# - Creates user 'john' with next available UID (usually 1000+)
# - -m creates home directory /home/john
# - -s sets login shell to /bin/bash
# - Adds entry to /etc/passwd and /etc/shadow
# - Creates private group 'john' with same GID as UID
# - Copies skeleton files from /etc/skel to home directory

# User with full details
useradd -m -c "Jane Doe" -e 2024-12-31 -s /bin/bash jane
# What happens:
# - -c "Jane Doe" sets the comment/full name field
# - -e 2024-12-31 sets account expiration date
# - Account will be automatically disabled after this date
# - Useful for temporary accounts or contractors

# User with multiple groups
useradd -m -G wheel,users -s /bin/bash mike
# What happens:
# - -G wheel,users adds user to supplementary groups
# - 'wheel' group typically has sudo privileges
# - 'users' is a common group for regular users
# - User still gets a private primary group

# System user (for services)
useradd -r -s /sbin/nologin serviceuser
# What happens:
# - -r creates system user with UID < 1000
# - -s /sbin/nologin prevents interactive login
# - No home directory created by default
# - Used for service accounts and daemons
```

**2. Modify user accounts with usermod**
```bash
# Change user comment/full name
usermod -c "John Smith" john
# What happens:
# - Updates the GECOS field in /etc/passwd
# - Changes the full name displayed by finger, who, etc.
# - Does not affect login name or authentication

# Modify user groups (CAREFUL: replaces all supplementary groups)
usermod -G developers,users john
# What happens:
# - Replaces ALL supplementary groups with new list
# - Primary group remains unchanged
# - User loses membership in previous supplementary groups
# - Use -a -G to append instead of replace

# Append to groups (safer method)
usermod -a -G developers john
# What happens:
# - -a (append) adds to existing groups without removing others
# - Safer than -G alone which replaces all groups
# - Preserves existing group memberships

# Change login shell
usermod -s /bin/zsh jane
# What happens:
# - Changes default shell in /etc/passwd
# - User will use zsh for future logins
# - Current sessions continue with old shell
# - Shell must exist in /etc/shells

# Lock user account
usermod -L john
# What happens:
# - Adds '!' to password hash in /etc/shadow
# - Prevents password authentication
# - SSH key authentication may still work
# - Account is not deleted, just disabled

# Unlock user account
usermod -U john
# What happens:
# - Removes '!' from password hash
# - Restores password authentication
# - User can log in normally again

# Change home directory
usermod -d /opt/john -m john
# What happens:
# - -d sets new home directory path
# - -m moves contents from old to new location
# - Updates /etc/passwd with new path
# - Creates new directory if it doesn't exist
```

**3. Password management and security**
```bash
# Set user password
passwd john
# What happens:
# - Prompts for new password (twice for confirmation)
# - Hashes password and stores in /etc/shadow
# - Enforces password complexity rules from /etc/security/pwquality.conf
# - Updates password change date

# View password aging information
chage -l john
# Shows:
# - Last password change date
# - Password expiration date
# - Account expiration date
# - Minimum/maximum password age
# - Warning period before expiration

# Set password aging policies
chage -M 90 -W 7 -I 14 john
# What happens:
# - -M 90: Password expires after 90 days
# - -W 7: Warning starts 7 days before expiration
# - -I 14: Account locked 14 days after password expires
# - Forces regular password changes for security

# Force password change on next login
chage -d 0 john
# What happens:
# - Sets last password change to epoch (1970-01-01)
# - Forces user to change password on next login
# - Useful for new accounts or security incidents
```

**4. User deletion**
```bash
# Delete user and home directory
userdel -r mike
# What happens:
# - Removes user from /etc/passwd and /etc/shadow
# - -r removes home directory and mail spool
# - Removes user from all groups
# - Files owned by user elsewhere remain (become orphaned)

# Delete user but keep home directory
userdel mike
# What happens:
# - Removes user account but preserves /home/mike
# - Useful when you need to preserve user data
# - Home directory ownership shows numeric UID
```

**5. Verification and troubleshooting**
```bash
# Check user information
id john                    # Show UID, GID, and groups
getent passwd john         # Show /etc/passwd entry
getent shadow john         # Show /etc/shadow entry (root only)
groups john               # Show user's groups

# List all users
getent passwd | cut -d: -f1  # All usernames
awk -F: '$3 >= 1000 {print $1}' /etc/passwd  # Regular users only

# Check account status
passwd -S john            # Show password status
lastlog -u john           # Show last login
who                       # Show currently logged in users
```

**Practice Scenarios:**
1. Create a developer user with appropriate groups and shell
2. Set up a temporary contractor account with expiration
3. Create a service account for a web application
4. Implement password aging policy for security compliance
5. Troubleshoot login issues for existing users

---

## 📚 **Chapter 6: Group Management and sudo**

### **Lab 6.1: Group Management and sudo Configuration**
**Objective:** Manage groups and configure sudo access
**Time:** 45 minutes

**Tasks:**
1. Create and manage groups
2. Add users to groups
3. Configure sudo access
4. Test sudo functionality
5. Configure password aging

**Key Commands:**
```bash
# Group management
groupadd developers
groupadd -g 1500 managers
groupmod -n dev developers
gpasswd -a john developers
gpasswd -d john developers
groupdel managers

# sudo configuration
visudo
# Add: john ALL=(ALL) ALL
# Add: %wheel ALL=(ALL) NOPASSWD: ALL

# Testing sudo
sudo -l
sudo systemctl status sshd
sudo su - root

# Password aging
chage -M 90 -m 7 -W 7 -I 14 john
chage -l john
```

---

## 📚 **Chapter 7: The Bash Shell**

### **Lab 7.1: Shell Features and Customization**
**Objective:** Master bash shell features and customization
**Time:** 60 minutes

**Tasks:**
1. Customize command prompt
2. Use input/output redirection
3. Work with pipes and filters
4. Configure aliases and functions
5. Use history and command substitution

**Key Commands:**
```bash
# Prompt customization
export PS1='\u@\h:\w\$ '
export PS1='\[\e[32m\]\u@\h:\w\$\[\e[0m\] '

# Redirection
ls -l > output.txt
ls -l >> output.txt
ls nonexistent 2> error.txt
ls -l > output.txt 2>&1
cat < input.txt

# Pipes and filters
ps aux | grep ssh
cat /etc/passwd | cut -d: -f1 | sort
dmesg | tail -20
history | grep sudo

# Aliases and functions
alias ll='ls -la'
alias grep='grep --color=auto'
function mkcd() { mkdir -p "$1" && cd "$1"; }

# Command substitution
echo "Today is $(date)"
echo "Current directory: $(pwd)"
files=$(ls *.txt)
```

---

## 📚 **Chapter 8: Linux Processes and Job Scheduling**

### **Lab 8.1: Process Management and Scheduling**
**Objective:** Monitor and control processes, schedule jobs
**Time:** 60 minutes

**Tasks:**
1. Monitor processes with ps and top
2. Control process priorities with nice/renice
3. Send signals to processes
4. Schedule jobs with at and cron
5. Manage job scheduling access

**Key Commands:**
```bash
# Process monitoring
ps aux
ps -ef
top
htop
pgrep sshd
pidof sshd

# Process control
nice -n 10 sleep 100 &
renice 5 1234
kill -TERM 1234
kill -KILL 1234
killall sleep

# Job scheduling - at
at now + 5 minutes
at> echo "Hello" > /tmp/message
at> <Ctrl+D>
atq
atrm 1

# Job scheduling - cron
crontab -e
# Add: 0 2 * * * /usr/bin/backup.sh
# Add: */15 * * * * /usr/bin/check_disk.sh
crontab -l
crontab -r

# Access control
echo "john" >> /etc/at.allow
echo "jane" >> /etc/cron.deny
```

---

## 📚 **Chapter 9: Basic Package Management**

### **Lab 9.1: RPM Package Management**
**Objective:** Manage packages using rpm command
**Time:** 45 minutes

**Tasks:**
1. Query package information
2. Install and remove packages
3. Verify package integrity
4. Extract files from packages
5. Work with GPG keys

**Key Commands:**
```bash
# Package queries
rpm -qa | grep ssh
rpm -qi openssh-server
rpm -ql openssh-server
rpm -qf /usr/sbin/sshd
rpm -qd openssh-server

# Package installation/removal
rpm -ivh package.rpm
rpm -Uvh package.rpm
rpm -e package-name
rpm -e --nodeps package-name

# Package verification
rpm -V openssh-server
rpm -Va
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

# Extract files
rpm2cpio package.rpm | cpio -idmv
```

---

## 📚 **Chapter 10: Advanced Package Management**

### **Lab 10.1: DNF Package Management**
**Objective:** Manage packages and repositories with dnf
**Time:** 60 minutes

**Tasks:**
1. Configure DNF repositories
2. Install, update, and remove packages
3. Manage package groups
4. Search and query packages
5. Handle dependencies

**Key Commands:**
```bash
# Repository management
dnf repolist
dnf repolist --enabled
dnf config-manager --add-repo http://example.com/repo
dnf config-manager --disable repo-name

# Package management
dnf install httpd
dnf update
dnf update httpd
dnf remove httpd
dnf autoremove

# Package information
dnf search web server
dnf info httpd
dnf list installed
dnf list available
dnf provides /usr/sbin/httpd

# Package groups
dnf group list
dnf group install "Development Tools"
dnf group remove "Development Tools"
dnf group info "Development Tools"

# History
dnf history
dnf history undo 5
```

---

## 📚 **Chapter 11: Boot Process, GRUB2, and the Linux Kernel**

### **Lab 11.1: Boot Process and GRUB2 Management**
**Objective:** Understand boot process and manage GRUB2 bootloader
**Time:** 60 minutes

**Tasks:**
1. Modify GRUB2 configuration
2. Change default boot timeout
3. Boot into different targets
4. Reset root password
5. Install and manage kernels

**Key Commands:**
```bash
# GRUB2 configuration
grub2-editenv list
grub2-mkconfig -o /boot/grub2/grub.cfg
grubby --default-kernel
grubby --info=ALL

# Boot timeout
sed -i 's/GRUB_TIMEOUT=5/GRUB_TIMEOUT=10/' /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg

# Target management
systemctl get-default
systemctl set-default multi-user.target
systemctl isolate rescue.target

# Kernel management
uname -r
rpm -qa kernel
dnf install kernel
dnf remove kernel-old-version
```

---

## 📚 **Chapter 12: System Initialization, Message Logging, and System Tuning**

### **Lab 12.1: systemd and Logging Management**
**Objective:** Manage services, logging, and system tuning
**Time:** 60 minutes

**Tasks:**
1. Manage systemd services and targets
2. Configure system logging
3. Work with systemd journal
4. Apply tuning profiles
5. Configure log rotation

**Key Commands:**
```bash
# Service management
systemctl status httpd
systemctl start httpd
systemctl enable httpd
systemctl disable httpd
systemctl mask httpd
systemctl unmask httpd

# Target management
systemctl list-units --type=target
systemctl isolate multi-user.target
systemctl set-default graphical.target

# Journal management
journalctl -u httpd
journalctl -f
journalctl --since "2023-01-01" --until "2023-01-02"
journalctl -p err
mkdir /var/log/journal
systemctl restart systemd-journald

# System tuning
tuned-adm list
tuned-adm active
tuned-adm profile throughput-performance
tuned-adm recommend
```

---

## 📚 **Chapter 13: Storage Management**

### **Lab 13.1: Disk Partitioning and LVM**
**Objective:** Manage disk partitions and logical volumes
**Time:** 90 minutes

**Tasks:**
1. Create MBR and GPT partitions
2. Set up LVM (PV, VG, LV)
3. Extend and reduce logical volumes
4. Configure VDO volumes
5. Remove storage components

**Key Commands:**
```bash
# Disk information
lsblk
fdisk -l
parted -l

# MBR partitioning
parted /dev/sdb mklabel msdos
parted /dev/sdb mkpart primary 1MiB 1GiB
parted /dev/sdb set 1 lvm on

# GPT partitioning
gdisk /dev/sdc
# n (new), 1 (partition number), default start, +1G, 8e00 (LVM)

# LVM setup
pvcreate /dev/sdb1 /dev/sdc1
vgcreate vg01 /dev/sdb1
lvcreate -L 500M -n lv01 vg01
lvcreate -l 100%FREE -n lv02 vg01

# LVM extension
vgextend vg01 /dev/sdc1
lvextend -L +200M /dev/vg01/lv01
lvextend -l +100%FREE /dev/vg01/lv02

# VDO setup
vdo create --name=vdo1 --device=/dev/sdd --vdoLogicalSize=10G
mkfs.xfs /dev/mapper/vdo1
```

---

## 📚 **Chapter 14: Local File Systems and Swap**

### **Lab 14.1: File System Management**
**Objective:** Create and manage file systems and swap
**Time:** 75 minutes

**Tasks:**
1. Create ext4, xfs, and vfat file systems
2. Mount file systems persistently
3. Resize file systems
4. Configure swap spaces
5. Monitor file system usage

**Key Commands:**
```bash
# File system creation
mkfs.ext4 /dev/vg01/lv01
mkfs.xfs /dev/vg01/lv02
mkfs.vfat /dev/sdb2

# File system mounting
mount /dev/vg01/lv01 /mnt/ext4
mount /dev/vg01/lv02 /mnt/xfs

# Persistent mounting
blkid /dev/vg01/lv01
echo "UUID=xxx /mnt/ext4 ext4 defaults 0 2" >> /etc/fstab
mount -a

# File system resizing
resize2fs /dev/vg01/lv01  # ext4
xfs_growfs /mnt/xfs       # xfs

# Swap management
mkswap /dev/sdb3
swapon /dev/sdb3
echo "/dev/sdb3 swap swap defaults 0 0" >> /etc/fstab
swapon -a
free -h

# Usage monitoring
df -h
du -sh /var/log
```

---

## 📚 **Chapter 15: Networking, Network Devices, and Network Connections**

### **Lab 15.1: Network Configuration**
**Objective:** Configure network interfaces and connections
**Time:** 60 minutes

**Tasks:**
1. Configure static IP addresses
2. Manage network connections with nmcli
3. Configure IPv6 addresses
4. Set up network bonding
5. Troubleshoot network issues

**Key Commands:**
```bash
# Network information
ip addr show
ip route show
nmcli con show
nmcli dev status

# Static IP configuration
nmcli con add type ethernet con-name static-eth0 ifname eth0
nmcli con mod static-eth0 ipv4.addresses 192.168.1.100/24
nmcli con mod static-eth0 ipv4.gateway 192.168.1.1
nmcli con mod static-eth0 ipv4.dns 8.8.8.8
nmcli con mod static-eth0 ipv4.method manual
nmcli con up static-eth0

# IPv6 configuration
nmcli con mod static-eth0 ipv6.addresses 2001:db8::100/64
nmcli con mod static-eth0 ipv6.method manual

# Network bonding
nmcli con add type bond con-name bond0 ifname bond0 mode active-backup
nmcli con add type ethernet slave-type bond con-name bond0-slave1 ifname eth1 master bond0
nmcli con add type ethernet slave-type bond con-name bond0-slave2 ifname eth2 master bond0

# Troubleshooting
ping -c 4 8.8.8.8
traceroute google.com
ss -tuln
netstat -rn
```

---

## 📚 **Chapter 16: Network File Systems**

### **Lab 16.1: NFS and AutoFS Configuration**
**Objective:** Configure NFS client and AutoFS
**Time:** 60 minutes

**Tasks:**
1. Mount NFS shares manually
2. Configure persistent NFS mounts
3. Set up AutoFS for automatic mounting
4. Configure direct and indirect maps
5. Test AutoFS functionality

**Key Commands:**
```bash
# NFS client setup
dnf install nfs-utils autofs
systemctl enable --now nfs-client.target

# Manual NFS mounting
showmount -e nfs-server.example.com
mount -t nfs nfs-server.example.com:/shared /mnt/nfs

# Persistent NFS mounting
echo "nfs-server.example.com:/shared /mnt/nfs nfs defaults 0 0" >> /etc/fstab
mount -a

# AutoFS configuration
systemctl enable --now autofs

# Direct map
echo "/mnt/direct /etc/auto.direct" >> /etc/auto.master
echo "/mnt/direct -rw nfs-server.example.com:/shared" > /etc/auto.direct

# Indirect map
echo "/mnt/indirect /etc/auto.indirect" >> /etc/auto.master
echo "shared -rw nfs-server.example.com:/shared" > /etc/auto.indirect

systemctl reload autofs

# Testing
ls /mnt/indirect/shared
df -h | grep nfs
```

---

## 📚 **Chapter 17: Time Services**

### **Lab 17.1: Chrony Time Synchronization**
**Objective:** Configure time synchronization with chrony
**Time:** 45 minutes

**Tasks:**
1. Configure chrony client
2. Add time servers
3. Monitor time synchronization
4. Set system date and time
5. Configure timezone

**Key Commands:**
```bash
# Chrony configuration
dnf install chrony
systemctl enable --now chronyd

# Configure time servers
echo "server 0.pool.ntp.org iburst" >> /etc/chrony.conf
echo "server 1.pool.ntp.org iburst" >> /etc/chrony.conf
systemctl restart chronyd

# Monitor synchronization
chronyc sources
chronyc sourcestats
chronyc tracking

# Manual time setting
timedatectl status
timedatectl set-time "2023-12-25 10:30:00"
timedatectl set-timezone America/New_York
timedatectl list-timezones

# Hardware clock
hwclock --show
hwclock --systohc
```

---

## 📚 **Chapter 18: The Secure Shell Service**

### **Lab 18.1: SSH Configuration and Key-Based Authentication**
**Objective:** Configure SSH service and key-based authentication
**Time:** 60 minutes

**Tasks:**
1. Configure SSH server
2. Generate and distribute SSH keys
3. Configure key-based authentication
4. Disable password authentication
5. Test remote access and file transfer

**Key Commands:**
```bash
# SSH server configuration
systemctl enable --now sshd
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Key generation
ssh-keygen -t rsa -b 4096
ssh-keygen -t ed25519

# Key distribution
ssh-copy-id user@remote-server
scp ~/.ssh/id_rsa.pub user@remote-server:~/.ssh/authorized_keys

# SSH configuration
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
systemctl restart sshd

# Testing
ssh user@remote-server
sftp user@remote-server
scp file.txt user@remote-server:/tmp/
rsync -av /local/dir/ user@remote-server:/remote/dir/
```

---

## 📚 **Chapter 19: The Linux Firewall**

### **Lab 19.1: Firewalld Configuration**
**Objective:** Configure firewall rules and zones
**Time:** 60 minutes

**Tasks:**
1. Manage firewall zones
2. Add and remove services
3. Configure port access
4. Create custom services
5. Test firewall rules

**Key Commands:**
```bash
# Firewall status
systemctl enable --now firewalld
firewall-cmd --state
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones

# Zone management
firewall-cmd --list-all
firewall-cmd --list-all-zones
firewall-cmd --set-default-zone=public
firewall-cmd --zone=dmz --change-interface=eth1

# Service management
firewall-cmd --list-services
firewall-cmd --add-service=http
firewall-cmd --add-service=https --permanent
firewall-cmd --remove-service=ssh
firewall-cmd --reload

# Port management
firewall-cmd --add-port=8080/tcp
firewall-cmd --add-port=1000-2000/tcp --permanent
firewall-cmd --remove-port=8080/tcp

# Custom service
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/myapp.xml
# Edit myapp.xml
firewall-cmd --reload
firewall-cmd --add-service=myapp

# Testing
ss -tuln
nmap -p 22,80,443 localhost
```

---

## 📚 **Chapter 20: Security Enhanced Linux**

### **Lab 20.1: SELinux Configuration and Management**
**Objective:** Configure and manage SELinux policies
**Time:** 75 minutes

**Tasks:**
1. Manage SELinux modes
2. Work with file contexts
3. Configure port labels
4. Manage SELinux booleans
5. Troubleshoot SELinux violations

**Key Commands:**
```bash
# SELinux status
getenforce
sestatus
selinux-config-enforcing-mode

# Mode management
setenforce 0  # permissive
setenforce 1  # enforcing
sed -i 's/SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config

# File contexts
ls -Z /var/www/html/
semanage fcontext -l | grep httpd
semanage fcontext -a -t httpd_exec_t "/opt/myapp(/.*)?"  
restorecon -Rv /opt/myapp

# Port labels
semanage port -l | grep http
semanage port -a -t http_port_t -p tcp 8080
semanage port -d -t http_port_t -p tcp 8080

# Booleans
getsebool -a | grep httpd
setsebool httpd_can_network_connect on
setsebool -P httpd_can_network_connect on

# Troubleshooting
ausearch -m AVC -ts recent
sealert -a /var/log/audit/audit.log
tail -f /var/log/audit/audit.log
```

---

## 📚 **Chapter 21: Shell Scripting**

### **Lab 21.1: Bash Script Development**
**Objective:** Create functional bash scripts with logic constructs
**Time:** 90 minutes

**Tasks:**
1. Create basic scripts with variables
2. Use command substitution
3. Implement conditional logic (if-then-else)
4. Create loops (for, while)
5. Handle script parameters and exit codes

**Key Commands:**
```bash
# Basic script structure
#!/bin/bash
# Script: system_info.sh

echo "System Information Report"
echo "========================"
echo "Hostname: $(hostname)"
echo "Date: $(date)"
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h

# Variables and parameters
#!/bin/bash
USER_NAME=$1
GROUP_NAME=${2:-users}
HOME_DIR="/home/$USER_NAME"

if [ $# -eq 0 ]; then
    echo "Usage: $0 <username> [group]"
    exit 1
fi

# Conditional logic
if [ -d "$HOME_DIR" ]; then
    echo "User directory exists"
else
    echo "Creating user directory"
    mkdir -p "$HOME_DIR"
fi

# Loops
for user in john jane mike; do
    echo "Processing user: $user"
    id $user 2>/dev/null || echo "User $user not found"
done

# While loop
counter=1
while [ $counter -le 5 ]; do
    echo "Count: $counter"
    counter=$((counter + 1))
done

# Functions
function backup_file() {
    local file=$1
    if [ -f "$file" ]; then
        cp "$file" "${file}.backup"
        echo "Backed up $file"
    fi
}

backup_file "/etc/passwd"
```

---

## 📚 **Chapter 22: Containers**

### **Lab 22.1: Container Management with Podman**
**Objective:** Manage containers using podman and systemd
**Time:** 90 minutes

**Tasks:**
1. Install container tools
2. Search and download images
3. Run and manage containers
4. Configure persistent storage
5. Set up containers as systemd services

**Key Commands:**
```bash
# Container tools installation
dnf install podman skopeo buildah

# Image management
podman search httpd
podman pull registry.redhat.io/ubi8/httpd-24
podman images
podman inspect httpd-24
skopeo inspect docker://registry.redhat.io/ubi8/httpd-24

# Container operations
podman run -d --name web-server -p 8080:8080 httpd-24
podman ps
podman ps -a
podman logs web-server
podman exec -it web-server /bin/bash

# Persistent storage
mkdir /opt/web-content
echo "<h1>Hello World</h1>" > /opt/web-content/index.html
podman run -d --name web-persistent \
  -p 8080:8080 \
  -v /opt/web-content:/var/www/html:Z \
  httpd-24

# Environment variables
podman run -d --name web-env \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=myapp \
  mysql:8.0

# Systemd integration (rootless)
mkdir -p ~/.config/systemd/user
podman generate systemd --new --name web-server > ~/.config/systemd/user/web-server.service
systemctl --user daemon-reload
systemctl --user enable --now web-server.service
systemctl --user status web-server.service

# Systemd integration (rootful)
sudo podman generate systemd --new --name web-server > /etc/systemd/system/web-server.service
sudo systemctl daemon-reload
sudo systemctl enable --now web-server.service

# Containerfile
cat > Containerfile << EOF
FROM registry.redhat.io/ubi8/ubi:latest
RUN dnf install -y httpd && dnf clean all
COPY index.html /var/www/html/
EXPOSE 80
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
EOF

podman build -t my-httpd .
podman run -d --name custom-web -p 8080:80 my-httpd
```

---

## 🎯 **Summary**

This comprehensive lab guide covers all 22 chapters from AG's RHCSA 9 Training and Exam Preparation Guide. Each lab includes:

- **Detailed objectives** aligned with RHCSA exam requirements
- **Step-by-step tasks** with practical scenarios
- **Essential commands** with explanations
- **Time estimates** for completion
- **Real-world applications** for each topic

**Total Study Time:** Approximately 25-30 hours of hands-on practice

**Next Steps:**
1. Complete each lab in sequence
2. Practice the DIY challenge labs from the PDF
3. Attempt the 4 sample exams in the appendices
4. Review and reinforce weak areas
5. Schedule your RHCSA exam

**Success Tips:**
- Practice in a real RHEL 9 environment
- Time yourself during lab exercises
- Focus on understanding concepts, not memorizing commands
- Test all configurations survive reboots
- Keep detailed notes of troubleshooting steps
