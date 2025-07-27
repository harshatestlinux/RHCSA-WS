# RHCSA Lab Answers and Solutions
*Answer Key for Red Hat RHCSA 9 Cert Guide Practice Labs*

## 📚 Reference Materials
- **[Red Hat RHCSA 9 Cert Guide PDF](../resources/pdfs/preview-9780138096151_A46998912.pdf)**
- **[Practice Labs](./red-hat-cert-guide-labs.md)**

---

## Chapter 1 Lab Solutions: Installing Red Hat Enterprise Linux

### Expected Outcomes:
```bash
# System verification commands and expected outputs:

# Check RHEL version
cat /etc/redhat-release
# Command breakdown:
# - cat: Concatenate and display file contents
# - /etc/redhat-release: System file containing OS version information
# Expected: Red Hat Enterprise Linux release 9.x (Plow)

# Verify kernel version
uname -a
# Command breakdown:
# - uname: Display system information
# - -a: Show all available system information (kernel name, hostname, kernel release, etc.)
# Expected: Linux hostname 5.14.0-xxx.el9.x86_64 #1 SMP ... x86_64 x86_64 x86_64 GNU/Linux

# Check disk partitions
df -h
# Command breakdown:
# - df: Display filesystem disk space usage
# - -h: Human-readable format (shows sizes in KB, MB, GB instead of blocks)
# Expected output showing separate partitions:
# /dev/sda1        1.0G  200M  800M  20% /boot
# /dev/sda2         20G  2.0G   18G  10% /
# /dev/sda3        5.0G  100M  4.9G   2% /home
# /dev/sda4        3.0G  100M  2.9G   4% /var
# /dev/sda5        2.0G   50M  1.9G   3% /tmp

# Check swap
swapon --show
# Command breakdown:
# - swapon: Enable/disable swap areas
# - --show: Display summary of swap areas currently in use
# Expected: /dev/sda6 partition 2G 0B -2
```

### Troubleshooting Common Issues:
- **Boot issues:** Check GRUB configuration in `/boot/grub2/grub.cfg`
- **Partition problems:** Use `lsblk` to verify partition layout
- **Network issues:** Verify with `nmcli device status`

---

## Chapter 2 Lab Solutions: Using Essential Tools

### Shell Skills Verification:
```bash
# Command history verification
history | tail -20
# Command breakdown:
# - history: Display command history list with line numbers
# - |: Pipe operator - sends output of first command to second command
# - tail -20: Display last 20 lines of input
# Expected: Shows last 20 commands with line numbers

# Command substitution verification
echo "Today is $(date)"
# Command breakdown:
# - echo: Display text to standard output
# - "Today is $(date)": String with command substitution
# - $(date): Command substitution - executes 'date' command and substitutes result
# - date: Display current date and time
# Expected: Today is Mon Jan 15 10:30:15 EST 2024

echo "Current user: $(whoami)"
# Command breakdown:
# - whoami: Display current username
# - $(whoami): Command substitution executing whoami command
# Expected: Current user: [your_username]

# Environment variables
echo $SHELL
# Command breakdown:
# - echo: Display text to standard output
# - $SHELL: Environment variable containing path to current shell
# - $: Variable expansion operator
# Expected: /bin/bash

env | grep PATH
# Command breakdown:
# - env: Display all environment variables
# - |: Pipe operator
# - grep PATH: Search for lines containing "PATH"
# Expected: PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
```

### I/O Redirection Solutions:
```bash
# File creation verification
cat testfile.txt
# Command breakdown:
# - cat: Concatenate and display file contents
# - testfile.txt: File created in previous lab exercises
# Expected output:
# Line 1
# Line 2
# Line 3

# Redirection verification
ls -la > directory_listing.txt && head -5 directory_listing.txt
# Command breakdown:
# - ls: List directory contents
# - -la: Long format (-l) showing all files (-a) including hidden ones
# - >: Output redirection operator (redirects stdout to file, overwrites existing)
# - directory_listing.txt: Target file for redirection
# - &&: Logical AND operator (execute second command only if first succeeds)
# - head -5: Display first 5 lines of input
# Expected: First 5 lines of directory listing

# Error redirection test
ls -la /nonexistent 2> error.log && cat error.log
# Command breakdown:
# - ls -la /nonexistent: Attempt to list non-existent directory (will generate error)
# - 2>: Error redirection operator (redirects stderr to file)
# - error.log: File to store error messages
# - &&: Execute cat only if ls command completes (even with errors)
# - cat error.log: Display contents of error log file
# Expected: ls: cannot access '/nonexistent': No such file or directory

# Pipe verification
cat /etc/passwd | grep root
# Command breakdown:
# - cat /etc/passwd: Display contents of password file
# - |: Pipe operator (sends stdout of first command to stdin of second)
# - grep root: Search for lines containing "root"
# - /etc/passwd: System file containing user account information
# Expected: root:x:0:0:root:/root:/bin/bash
```

### Help System Solutions:
```bash
# Man page verification
man ls | head -10
# Command breakdown:
# - man: Display manual pages for commands
# - ls: Command to get manual page for
# - |: Pipe operator
# - head -10: Display first 10 lines of manual page
# Expected: LS(1) manual page header

# Apropos verification
apropos "copy files"
# Command breakdown:
# - apropos: Search manual page names and descriptions
# - "copy files": Search term (quotes preserve the space)
# - Searches through manual page descriptions for matching keywords
# Expected: cp (1) - copy files or directories
#          rsync (1) - a fast, versatile, remote (and local) file-copying tool

# Documentation check
ls /usr/share/doc/ | wc -l
# Command breakdown:
# - ls: List directory contents
# - /usr/share/doc/: Standard directory containing package documentation
# - |: Pipe operator
# - wc -l: Word count with -l flag (count lines only)
# Expected: Number > 100 (showing installed package documentation)
```

---

## Chapter 3 Lab Solutions: Essential File Management Tools

### File System Navigation:
```bash
# Directory structure verification
tree ~/lab3/
# Command breakdown:
# - tree: Display directory structure in tree format
# - ~/lab3/: Target directory (~ represents home directory)
# - Shows hierarchical view of directories and files
# Expected output:
# /home/user/lab3/
# ├── backup/
# ├── docs/
# │   ├── file1.txt
# │   ├── file2.txt
# │   ├── file3.txt
# │   ├── file4.txt
# │   └── file5.txt
# ├── scripts/
# │   ├── scriptA.sh
# │   ├── scriptB.sh
# │   └── scriptC.sh
# └── temp/

# Wildcard verification
ls docs/file*.txt | wc -l
# Command breakdown:
# - ls: List directory contents
# - docs/file*.txt: Path with wildcard (* matches any characters)
# - |: Pipe operator
# - wc -l: Count lines (number of files matched)
# Expected: 5

ls scripts/script[A-C].sh
# Command breakdown:
# - ls: List directory contents
# - scripts/script[A-C].sh: Path with character class wildcard
# - [A-C]: Character range matching A, B, or C
# - .sh: File extension for shell scripts
# Expected: scripts/scriptA.sh scripts/scriptB.sh scripts/scriptC.sh
```

### Links Solutions:
```bash
# Link verification
ls -li ~/lab3/ | grep -E "(original|hardlink|symlink)"
# Command breakdown:
# - ls: List directory contents
# - -l: Long format (detailed information)
# - -i: Show inode numbers (unique file identifiers)
# - ~/lab3/: Target directory
# - |: Pipe operator
# - grep -E: Extended regular expressions search
# - "(original|hardlink|symlink)": Pattern matching any of these words
# Expected output showing:
# - Same inode number for original.txt and hardlink.txt
# - Different inode for symlink.txt with -> pointing to original.txt

stat ~/lab3/original.txt
# Command breakdown:
# - stat: Display detailed file/filesystem status
# - ~/lab3/original.txt: Target file path
# - Shows file size, permissions, timestamps, inode, links count
# Expected: Shows Links: 2 (original + hardlink)

stat ~/lab3/symlink.txt
# Command breakdown:
# - stat: Display detailed file/filesystem status
# - ~/lab3/symlink.txt: Target symbolic link
# - For symlinks, shows both link and target file information
# Expected: Shows it's a symbolic link pointing to original.txt

# Content verification after modification
cat ~/lab3/original.txt
# Command breakdown:
# - cat: Concatenate and display file contents
# - ~/lab3/original.txt: Source file that was modified via hardlink
# - Hard links share the same data blocks, so changes appear in both
# Expected: 
# Original content for linking practice
# Additional content

cat ~/lab3/symlink.txt
# Command breakdown:
# - cat: Concatenate and display file contents
# - ~/lab3/symlink.txt: Symbolic link file
# - Symlinks point to the original file, so content is identical
# Expected: Same content as original.txt
```

### Archive Solutions:
```bash
# Archive verification
tar -tzf ~/lab3/backup/docs_backup.tar.gz
# Command breakdown:
# - tar: Archive manipulation tool
# - -t: List contents of archive (test/list mode)
# - -z: Handle gzip compression (.gz files)
# - -f: Specify archive filename
# - ~/lab3/backup/docs_backup.tar.gz: Path to compressed archive
# Expected: List of files in docs/ directory with full paths

tar -tjf ~/lab3/backup/scripts_backup.tar.bz2
# Command breakdown:
# - tar: Archive manipulation tool
# - -t: List contents of archive
# - -j: Handle bzip2 compression (.bz2 files)
# - -f: Specify archive filename
# - ~/lab3/backup/scripts_backup.tar.bz2: Path to bzip2 compressed archive
# Expected: List of script files

# Extraction verification
ls -la ~/lab3/restore_test/
# Command breakdown:
# - ls: List directory contents
# - -l: Long format (detailed information)
# - -a: Show all files including hidden ones
# - ~/lab3/restore_test/: Directory where archive was extracted
# - Verifies that extraction preserved directory structure and permissions
# Expected: Restored directory structure matching original
```

---

## Chapter 4 Lab Solutions: Working with Text Files

### Text Analysis Solutions:
```bash
# Sample log analysis
head -3 ~/lab3/sample.log
# Expected:
# 2024-01-15 10:30:15 INFO User alice logged in
# 2024-01-15 10:31:22 ERROR Failed login attempt for user bob
# 2024-01-15 10:32:45 INFO User charlie logged in

wc -l ~/lab3/sample.log
# Expected: 6 ~/lab3/sample.log

cut -d' ' -f1,2 ~/lab3/sample.log
# Expected: Date and time columns only
# 2024-01-15 10:30:15
# 2024-01-15 10:31:22
# etc.
```

### Regular Expression Solutions:
```bash
# Basic grep results
grep "ERROR" ~/lab3/sample.log
# Expected:
# 2024-01-15 10:31:22 ERROR Failed login attempt for user bob
# 2024-01-15 10:34:55 ERROR Database connection failed

grep "^2024" ~/lab3/sample.log | wc -l
# Expected: 6 (all lines start with 2024)

grep -E "(ERROR|WARNING)" ~/lab3/sample.log
# Expected: 3 lines (2 ERROR + 1 WARNING)

grep -c "INFO" ~/lab3/sample.log
# Expected: 3
```

---

## Chapter 5 Lab Solutions: SSH and Remote Access

### SSH Key Authentication:
```bash
# Key generation verification
ls -la ~/.ssh/rhcsa_lab_key*
# Expected:
# -rw------- 1 user user 3243 Jan 15 10:30 /home/user/.ssh/rhcsa_lab_key
# -rw-r--r-- 1 user user  743 Jan 15 10:30 /home/user/.ssh/rhcsa_lab_key.pub

# Key-based login test
ssh -i ~/.ssh/rhcsa_lab_key user@server2 'hostname; date'
# Expected: 
# server2.example.com
# Mon Jan 15 10:35:22 EST 2024
```

### File Transfer Solutions:
```bash
# SCP verification
ssh -i ~/.ssh/rhcsa_lab_key user@server2 'ls -la /tmp/sample.log'
# Expected: File exists on remote server with correct size

# Rsync verification
ssh -i ~/.ssh/rhcsa_lab_key user@server2 'ls -la /tmp/lab3_sync/'
# Expected: Complete directory structure synchronized
```

---

## Chapter 6 Lab Solutions: User and Group Management

### User Creation Verification:
```bash
# User account verification
id dbadmin
# Command breakdown:
# - id: Display user and group IDs
# - dbadmin: Username to query
# - Shows UID (user ID), GID (primary group ID), and all group memberships
# Expected: uid=1001(dbadmin) gid=1001(dbadmin) groups=1001(dbadmin),10(wheel)

getent passwd webdev
# Command breakdown:
# - getent: Get entries from administrative databases
# - passwd: Query the passwd database (/etc/passwd)
# - webdev: Username to look up
# - Returns passwd entry: username:password:UID:GID:comment:home:shell
# Expected: webdev:x:1002:1002:Web Developer:/opt/webdev:/bin/bash

# Home directory verification
ls -la /opt/webdev/
# Command breakdown:
# - ls: List directory contents
# - -l: Long format showing permissions, ownership, size, date
# - -a: Show all files including hidden ones (starting with .)
# - /opt/webdev/: Custom home directory path for webdev user
# Expected: Home directory exists with proper ownership

# Password aging verification
sudo chage -l dbadmin
# Command breakdown:
# - sudo: Execute command with elevated privileges
# - chage: Change user password aging information
# - -l: List password aging information for user
# - dbadmin: Username to query
# - Shows password expiration, aging policies, and account status
# Expected:
# Last password change: Jan 15, 2024
# Password expires: Apr 15, 2024
# Password inactive: never
# Account expires: never
# Minimum number of days between password change: 7
# Maximum number of days between password change: 90
# Number of days of warning before password expires: 14
```

### Group Management Solutions:
```bash
# Group verification
getent group developers
# Expected: developers:x:2000:dbadmin,webdev

groups dbadmin
# Expected: dbadmin wheel developers dbusers

id webdev
# Expected: Shows membership in developers and webadmins groups
```

---

## Chapter 7 Lab Solutions: Permissions Management

### Permission Verification:
```bash
# Directory permissions check
ls -la /shared/
# Expected output showing:
# drwxrwxr-t  2 root developers  4096 Jan 15 10:30 group/  (SGID + sticky bit)
# drwxr-xr-x  2 root root       4096 Jan 15 10:30 public/
# drwx------  2 root root       4096 Jan 15 10:30 private/

# Special permissions verification
ls -la /shared/ | grep -E "(suid|sgid)"
# Expected: Files showing 's' in permission bits

# ACL verification
getfacl /shared/private
# Expected:
# # file: shared/private
# # owner: root
# # group: root
# user::rwx
# user:webdev:rwx
# group::---
# mask::rwx
# other::---
```

### File Attributes Solutions:
```bash
# Immutable attribute test
lsattr /shared/immutable_file
# Expected: ----i--------e-- /shared/immutable_file

# Test immutability (should fail)
sudo rm /shared/immutable_file
# Expected: rm: cannot remove '/shared/immutable_file': Operation not permitted
```

---

## Chapter 8 Lab Solutions: Network Configuration

### Network Interface Verification:
```bash
# Connection verification
nmcli connection show static-lab
# Expected: Detailed connection information showing static IP configuration

ip addr show eth0
# Expected: Interface showing 192.168.1.100/24 address

# DNS verification
cat /etc/resolv.conf
# Expected: nameserver 8.8.8.8, nameserver 8.8.4.4

# Connectivity test
ping -c 2 8.8.8.8
# Expected: 2 packets transmitted, 2 received, 0% packet loss
```

### Hostname Solutions:
```bash
# Hostname verification
hostnamectl
# Expected:
#    Static hostname: rhcsa-lab.example.com
#          Icon name: computer-vm
#            Chassis: vm
#         Machine ID: [unique-id]
#            Boot ID: [unique-id]
#     Virtualization: vmware
#   Operating System: Red Hat Enterprise Linux 9.x
```

---

## Chapter 9 Lab Solutions: Software Management

### Package Management Verification:
```bash
# Repository verification
sudo dnf repolist | grep epel
# Expected: epel/x86_64 Extra Packages for Enterprise Linux 9

# Package installation verification
rpm -q httpd
# Expected: httpd-2.4.xx-x.el9.x86_64

systemctl status httpd
# Expected: Service status (may be inactive, but installed)

# Package search verification
dnf search httpd | head -5
# Expected: List of httpd-related packages
```

### DNF History Solutions:
```bash
# History verification
dnf history | head -5
# Expected: Transaction history with ID, Command Line, Date and time, Action(s), Altered

# Group installation verification
dnf group list installed | grep "Development Tools"
# Expected: Development Tools group listed as installed
```

---

## Chapter 10 Lab Solutions: Process Management

### Process Control Verification:
```bash
# Background jobs verification
jobs
# Expected: List of background jobs with job numbers

# Process monitoring
ps aux | grep sleep | grep -v grep
# Expected: sleep processes running with different PIDs

# Process priority verification
ps -eo pid,ni,comm | grep sleep
# Expected: sleep processes showing different nice values (10, -5, 0)
```

---

## Advanced Lab Solutions

### Chapter 11: systemd Solutions
```bash
# Custom service verification
sudo systemctl status myapp.service
# Expected:
# ● myapp.service - My Custom Application
#    Loaded: loaded (/etc/systemd/system/myapp.service; enabled; vendor preset: disabled)
#    Active: active (running) since Mon 2024-01-15 10:30:15 EST; 5min ago

# Service accessibility test
curl http://localhost:8080
# Expected: Directory listing or default Python HTTP server response
```

### Chapter 12: Scheduling Solutions
```bash
# Cron verification
crontab -l
# Command breakdown:
# - crontab: Manage user cron jobs
# - -l: List current user's cron jobs
# Expected: 0 2 * * * /usr/bin/find /tmp -type f -mtime +7 -delete

# System cron verification
grep "yum update" /etc/crontab
# Command breakdown:
# - grep: Search for text patterns
# - "yum update": Search pattern
# - /etc/crontab: System-wide cron configuration file
# Expected: 30 3 * * 0 root /usr/bin/yum update -y

# Systemd timer verification
sudo systemctl list-timers | grep backup
# Command breakdown:
# - systemctl list-timers: List all systemd timers and their status
# - |: Pipe operator
# - grep backup: Filter for backup-related timers
# Expected: backup.timer listed with next run time
```

### Chapter 13: Logging Solutions
```bash
# Rsyslog configuration verification
sudo systemctl status rsyslog
# Command breakdown:
# - systemctl status: Check service status
# - rsyslog: System logging daemon
# Expected: active (running) status

ls -la /etc/rsyslog.d/myapp.conf
# Command breakdown:
# - ls -la: List files with detailed information
# - /etc/rsyslog.d/myapp.conf: Custom rsyslog configuration file
# Expected: Configuration file exists with proper permissions

# Test custom logging
logger -p local0.info "Test message" && tail -1 /var/log/myapp.log
# Command breakdown:
# - logger: Send messages to system log
# - -p local0.info: Specify facility (local0) and priority (info)
# - &&: Execute second command only if first succeeds
# - tail -1: Show last line of file
# Expected: Test message appears in custom log file

# Journal verification
journalctl --disk-usage
# Command breakdown:
# - journalctl: Query systemd journal
# - --disk-usage: Show disk space used by journal files
# Expected: Journal disk usage information

journalctl -u rsyslog.service --no-pager
# Command breakdown:
# - -u rsyslog.service: Show logs for specific systemd unit
# - --no-pager: Output directly without pager
# Expected: rsyslog service log entries
```

### Chapter 16: Kernel Management Solutions
```bash
# Module verification
lsmod | grep dummy
# Command breakdown:
# - lsmod: List loaded kernel modules
# - |: Pipe operator
# - grep dummy: Search for dummy module
# Expected: dummy module listed if loaded

modinfo dummy | head -10
# Command breakdown:
# - modinfo: Display kernel module information
# - dummy: Module name to query
# - |: Pipe operator
# - head -10: Show first 10 lines
# Expected: Module description, author, license, parameters

# Kernel parameter verification
sysctl vm.swappiness
# Command breakdown:
# - sysctl: Configure kernel parameters at runtime
# - vm.swappiness: Virtual memory swappiness parameter
# Expected: Current swappiness value (default 60, modified to 10)

grep "vm.swappiness" /etc/sysctl.conf
# Command breakdown:
# - grep: Search for text patterns
# - "vm.swappiness": Search pattern
# - /etc/sysctl.conf: Kernel parameter configuration file
# Expected: vm.swappiness = 10
```

### Chapter 17: Boot Procedure Solutions
```bash
# GRUB verification
grub2-editenv list
# Command breakdown:
# - grub2-editenv: Edit GRUB environment variables
# - list: Show current environment variables
# Expected: saved_entry=0 (or current default)

grep "GRUB_CMDLINE_LINUX" /etc/default/grub
# Command breakdown:
# - grep: Search for text patterns
# - "GRUB_CMDLINE_LINUX": GRUB kernel command line parameter
# - /etc/default/grub: GRUB configuration file
# Expected: GRUB_CMDLINE_LINUX="rhgb quiet"

# Boot target verification
systemctl get-default
# Command breakdown:
# - systemctl: Control systemd services and targets
# - get-default: Show default boot target
# Expected: multi-user.target (or graphical.target)

systemctl list-units --type=target
# Command breakdown:
# - systemctl list-units: List systemd units
# - --type=target: Filter for target units only
# Expected: List of available systemd targets
```

### Chapter 18: Troubleshooting Solutions
```bash
# System status verification
systemctl --failed
# Command breakdown:
# - systemctl: Control systemd services
# - --failed: Show only failed units
# Expected: List of failed services (should be empty in healthy system)

journalctl -p err --since today
# Command breakdown:
# - journalctl: Query systemd journal
# - -p err: Show only error priority messages
# - --since today: Filter messages from today
# Expected: Error messages from today (if any)

# Network troubleshooting verification
ping -c 2 8.8.8.8
# Command breakdown:
# - ping: Send ICMP echo requests
# - -c 2: Send only 2 packets
# - 8.8.8.8: Google's public DNS server
# Expected: 2 packets transmitted, 2 received, 0% packet loss

ss -tuln | grep :22
# Command breakdown:
# - ss: Display socket statistics
# - -t: Show TCP sockets
# - -u: Show UDP sockets
# - -l: Show listening sockets
# - -n: Show numerical addresses
# - |: Pipe operator
# - grep :22: Filter for SSH port
# Expected: SSH daemon listening on port 22
```

### Chapter 14-15: Storage Solutions
```bash
# Partition verification
lsblk | grep sdb
# Expected:
# sdb      8:16   0   10G  0 disk 
# ├─sdb1   8:17   0    1G  0 part 
# ├─sdb2   8:18   0    1G  0 part 
# └─sdb3   8:19   0    8G  0 part

# LVM verification
pvdisplay
# Command breakdown:
# - pvdisplay: Display physical volume information
# - Shows PV name, VG name, PV size, allocatable space, PE size, total PE, free PE
# - Physical volumes are the foundation of LVM storage
# Expected: Physical volume information for /dev/sdb3

vgdisplay vg_data
# Command breakdown:
# - vgdisplay: Display volume group information
# - vg_data: Specific volume group name to query
# - Shows VG name, system ID, format, metadata areas, VG size, PE size, total PE, alloc PE, free PE
# - Volume groups combine physical volumes into pools of storage
# Expected: Volume group details showing total/free space

lvdisplay /dev/vg_data/lv_web
# Command breakdown:
# - lvdisplay: Display logical volume information
# - /dev/vg_data/lv_web: Full path to logical volume
# - Shows LV path, LV name, VG name, LV UUID, LV write access, LV creation host, LV status, LV size, current LE, segments, allocation
# - Logical volumes are the usable storage units created from volume groups
# Expected: Logical volume details

# Mount verification
df -h | grep -E "(web|database)"
# Expected:
# /dev/mapper/vg_data-lv_web  485M   2.3M  458M   1% /mnt/web
# /dev/mapper/vg_data-lv_db   295M    21M  275M   8% /mnt/database

# Fstab verification
grep vg_data /etc/fstab
# Expected: Entries for both logical volumes
```

### Chapter 22: SELinux Solutions
```bash
# SELinux status verification
getenforce
# Expected: Enforcing

sestatus
# Expected: SELinux status: enabled, Current mode: enforcing

# Boolean verification
getsebool httpd_can_network_connect
# Expected: httpd_can_network_connect --> on

# Context verification
ls -Z /web/content/
# Expected: Files showing httpd_exec_t context

# Audit log verification (after generating denial)
sudo ausearch -m AVC -ts recent | grep httpd
# Expected: AVC denial messages if any policy violations occurred
```

### Chapter 23: Firewall Solutions
```bash
# Firewall status verification
sudo firewall-cmd --state
# Expected: running

# Zone configuration verification
sudo firewall-cmd --list-all
# Expected: Services: dhcpv6-client http https ssh, Ports: 8080/tcp

# Custom zone verification
sudo firewall-cmd --zone=webserver --list-all
# Expected: webserver zone with http and https services
```

### Chapter 26: Container Solutions
```bash
# Container verification
podman ps
# Expected: Running web-server container on port 8080

# Container accessibility test
curl http://localhost:8080
# Expected: Nginx welcome page or custom content

# Systemd service verification
systemctl --user status web-server.service
# Expected: Container service running under user systemd
```

---

## Common Troubleshooting Solutions

### Network Issues:
```bash
# If network doesn't work:
sudo nmcli connection show  # Check connections
sudo nmcli device status   # Check device status
ip route show              # Check routing table
sudo systemctl restart NetworkManager  # Restart network service
```

### Storage Issues:
```bash
# If mounts fail:
sudo mount -a              # Test all fstab entries
sudo systemctl daemon-reload  # Reload systemd
lsblk                      # Verify block devices
sudo vgscan                # Scan for volume groups
```

### Service Issues:
```bash
# If services don't start:
sudo systemctl daemon-reload  # Reload systemd configuration
sudo journalctl -u service_name  # Check service logs
sudo systemctl status service_name  # Check service status
sudo setenforce 0          # Temporarily disable SELinux for testing
```

### Permission Issues:
```bash
# If permission denied:
ls -la file_or_directory   # Check permissions
getfacl file_or_directory  # Check ACLs
sudo restorecon -Rv path   # Restore SELinux contexts
sudo setsebool -P boolean_name on  # Set SELinux booleans
```

### User Issues:
```bash
# If user can't login:
sudo passwd username       # Reset password
sudo chage -l username     # Check password aging
sudo usermod -U username   # Unlock account
grep username /etc/passwd  # Verify user exists
```

---

## Exam Day Verification Checklist

### Before Submitting Each Task:
1. **Test the configuration:**
   ```bash
   # For services:
   sudo systemctl status service_name
   sudo systemctl is-enabled service_name
   
   # For network:
   ping -c 2 target_ip
   curl http://localhost:port
   
   # For users:
   su - username
   ssh username@hostname
   
   # For storage:
   df -h
   mount | grep mount_point
   ```

2. **Verify persistence:**
   ```bash
   # Check configuration files:
   cat /etc/fstab           # Storage mounts
   cat /etc/systemd/system/service.service  # Custom services
   crontab -l               # User cron jobs
   sudo crontab -l          # System cron jobs
   ```

3. **Test after reboot (if time permits):**
   ```bash
   sudo reboot
   # After reboot, verify all services and mounts are working
   ```

### Expected Exam Environment:
- **Time:** 2.5 hours (150 minutes)
- **Tasks:** 15-20 practical tasks
- **Passing Score:** 210/300 (70%)
- **Environment:** 2 RHEL 9 systems
- **Access:** SSH and console access

### Time Allocation Strategy:
- **Quick tasks (5-10 min each):** User management, basic permissions
- **Medium tasks (10-15 min each):** Network config, service setup
- **Complex tasks (15-25 min each):** Storage/LVM, SELinux troubleshooting
- **Review time:** 30 minutes at the end

---

*This answer key provides expected outputs and solutions for all practice labs. Use it to verify your work and understand the correct approaches for RHCSA exam success!*
