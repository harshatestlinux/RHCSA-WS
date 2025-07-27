# RHCSA Comprehensive Practice Labs
*Based on Red Hat Certification Study Guide and Official Exam Objectives*

## 📚 Study Resources
- **[Red Hat Certification Study Guide](./preview-9780138096151_A46998912.pdf)** - Primary reference material
- **Lab Environment:** 2 RHEL 9 VMs (4GB RAM, 40GB disk each)

---

## Lab Environment Setup

### VM Configuration
```bash
# VM1 (server1.example.com) - Primary system
# VM2 (server2.example.com) - Secondary for networking/clustering
# Network: 192.168.1.0/24
# server1: 192.168.1.10
# server2: 192.168.1.20
```

### Initial Setup Commands
```bash
# Set hostnames
sudo hostnamectl set-hostname server1.example.com
sudo hostnamectl set-hostname server2.example.com

# Update /etc/hosts
echo "192.168.1.10 server1.example.com server1" | sudo tee -a /etc/hosts
echo "192.168.1.20 server2.example.com server2" | sudo tee -a /etc/hosts
```

---

## Week 1: System Access and File Management

### Lab 1.1: Boot Process and GRUB Configuration
**Objective:** Master system boot, GRUB, and recovery procedures

**Scenario:** You're a system administrator who needs to configure boot parameters and recover from boot failures.

**Tasks:**
1. **Modify GRUB boot parameters**
   ```bash
   # Edit GRUB configuration
   sudo vim /etc/default/grub
   # Add: GRUB_CMDLINE_LINUX="rhgb quiet net.ifnames=0"
   sudo grub2-mkconfig -o /boot/grub2/grub.cfg
   ```

2. **Reset root password using rescue mode**
   ```bash
   # Boot to rescue mode (rd.break)
   # At rescue prompt:
   mount -o remount,rw /sysroot
   chroot /sysroot
   passwd root
   touch /.autorelabel
   exit
   reboot
   ```

3. **Configure systemd targets**
   ```bash
   # Set default target
   sudo systemctl set-default multi-user.target
   sudo systemctl get-default
   
   # Boot to specific target
   sudo systemctl isolate graphical.target
   ```

**Verification:**
- System boots with custom parameters
- Root password successfully reset
- Default target correctly set

---

### Lab 1.2: Advanced File Operations and Permissions
**Objective:** Master file permissions, ACLs, and special attributes

**Scenario:** Configure a shared project directory with complex permission requirements.

**Tasks:**
1. **Create directory structure with proper permissions**
   ```bash
   # Create project directories
   sudo mkdir -p /projects/{web,database,backup}
   sudo mkdir -p /projects/shared/{docs,scripts,logs}
   
   # Set ownership and permissions
   sudo chown -R root:developers /projects
   sudo chmod 2775 /projects/*
   sudo chmod g+s /projects/shared
   ```

2. **Implement Access Control Lists (ACLs)**
   ```bash
   # Create users and groups
   sudo groupadd developers
   sudo groupadd webadmins
   sudo useradd -G developers alice
   sudo useradd -G developers,webadmins bob
   sudo useradd charlie
   
   # Set ACLs
   sudo setfacl -m g:webadmins:rwx /projects/web
   sudo setfacl -m u:charlie:r-- /projects/web
   sudo setfacl -d -m g:developers:rw- /projects/shared
   ```

3. **Configure special file attributes**
   ```bash
   # Create files with special permissions
   sudo touch /projects/shared/critical.conf
   sudo chattr +i /projects/shared/critical.conf  # Immutable
   
   sudo touch /projects/shared/append.log
   sudo chattr +a /projects/shared/append.log     # Append-only
   ```

**Verification:**
```bash
# Check permissions and ACLs
ls -la /projects/
getfacl /projects/web
lsattr /projects/shared/*
```

---

### Lab 1.3: File System Links and Archives
**Objective:** Master hard links, symbolic links, and archive management

**Tasks:**
1. **Create and manage links**
   ```bash
   # Create test files
   echo "Original content" > /tmp/original.txt
   
   # Hard link
   ln /tmp/original.txt /tmp/hardlink.txt
   
   # Symbolic links
   ln -s /tmp/original.txt /tmp/symlink.txt
   ln -s /nonexistent /tmp/broken_link
   
   # Verify link properties
   ls -li /tmp/*link*
   stat /tmp/original.txt /tmp/hardlink.txt
   ```

2. **Archive and compression operations**
   ```bash
   # Create archives
   tar -czf /tmp/projects_backup.tar.gz /projects/
   tar -cjf /tmp/logs_backup.tar.bz2 /var/log/*.log
   
   # Extract and verify
   mkdir /tmp/restore_test
   tar -xzf /tmp/projects_backup.tar.gz -C /tmp/restore_test
   ```

---

## Week 2: User and Group Management

### Lab 2.1: Advanced User Management
**Objective:** Master user creation, modification, and password policies

**Scenario:** Set up user accounts for a development team with specific requirements.

**Tasks:**
1. **Create users with custom settings**
   ```bash
   # Create users with specific requirements
   sudo useradd -m -s /bin/bash -G wheel -c "Senior Developer" john
   sudo useradd -m -s /bin/bash -G developers -e 2024-12-31 temp_dev
   sudo useradd -m -s /bin/bash -d /opt/service_user service_account
   
   # Set passwords
   echo "john:SecurePass123!" | sudo chpasswd
   sudo passwd --expire temp_dev
   ```

2. **Configure password policies**
   ```bash
   # Edit password aging
   sudo chage -M 90 -m 7 -W 14 john
   sudo chage -E 2024-12-31 temp_dev
   
   # View password information
   sudo chage -l john
   ```

3. **Modify user accounts**
   ```bash
   # Change user properties
   sudo usermod -aG webadmins john
   sudo usermod -s /bin/zsh john
   sudo usermod -L temp_dev  # Lock account
   ```

**Verification:**
```bash
id john
groups john
sudo chage -l john
```

---

### Lab 2.2: Group Management and sudo Configuration
**Objective:** Configure groups and sudo access

**Tasks:**
1. **Advanced group operations**
   ```bash
   # Create groups with specific GIDs
   sudo groupadd -g 1500 dbadmins
   sudo groupadd -g 1501 backup_operators
   
   # Add users to multiple groups
   sudo usermod -aG dbadmins,backup_operators alice
   ```

2. **Configure sudo access**
   ```bash
   # Create sudo rules
   echo "john ALL=(ALL) NOPASSWD: /usr/bin/systemctl" | sudo tee /etc/sudoers.d/john
   echo "%dbadmins ALL=(postgres) /usr/bin/psql" | sudo tee /etc/sudoers.d/dbadmins
   
   # Validate sudoers files
   sudo visudo -c
   ```

---

## Week 3: File Systems and Storage

### Lab 3.1: Disk Partitioning and File Systems
**Objective:** Master disk management, partitioning, and file system creation

**Scenario:** Add new storage to the system and configure various file systems.

**Tasks:**
1. **Create and manage partitions**
   ```bash
   # Add a new disk (simulate with loop device)
   sudo dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024
   sudo losetup /dev/loop1 /tmp/disk1.img
   
   # Create partition table and partitions
   sudo fdisk /dev/loop1
   # Create: 500MB ext4, 300MB xfs, 200MB swap
   
   # Alternative with parted
   sudo parted /dev/loop1 mklabel gpt
   sudo parted /dev/loop1 mkpart primary ext4 1MiB 501MiB
   sudo parted /dev/loop1 mkpart primary xfs 501MiB 801MiB
   sudo parted /dev/loop1 mkpart primary linux-swap 801MiB 1001MiB
   ```

2. **Create file systems**
   ```bash
   # Create different file systems
   sudo mkfs.ext4 -L "data_ext4" /dev/loop1p1
   sudo mkfs.xfs -L "data_xfs" /dev/loop1p2
   sudo mkswap -L "swap1" /dev/loop1p3
   
   # Enable swap
   sudo swapon /dev/loop1p3
   ```

3. **Mount file systems**
   ```bash
   # Create mount points
   sudo mkdir -p /mnt/{data_ext4,data_xfs}
   
   # Mount file systems
   sudo mount /dev/loop1p1 /mnt/data_ext4
   sudo mount /dev/loop1p2 /mnt/data_xfs
   
   # Add to fstab
   echo "LABEL=data_ext4 /mnt/data_ext4 ext4 defaults 0 2" | sudo tee -a /etc/fstab
   echo "LABEL=data_xfs /mnt/data_xfs xfs defaults 0 2" | sudo tee -a /etc/fstab
   echo "LABEL=swap1 none swap defaults 0 0" | sudo tee -a /etc/fstab
   ```

**Verification:**
```bash
lsblk
df -h
swapon --show
mount | grep loop1
```

---

### Lab 3.2: LVM Configuration
**Objective:** Master Logical Volume Management

**Tasks:**
1. **Create LVM structure**
   ```bash
   # Create another disk for LVM
   sudo dd if=/dev/zero of=/tmp/disk2.img bs=1M count=2048
   sudo losetup /dev/loop2 /tmp/disk2.img
   
   # Create physical volume
   sudo pvcreate /dev/loop2
   
   # Create volume group
   sudo vgcreate vg_data /dev/loop2
   
   # Create logical volumes
   sudo lvcreate -L 500M -n lv_web vg_data
   sudo lvcreate -L 300M -n lv_db vg_data
   sudo lvcreate -l 100%FREE -n lv_backup vg_data
   ```

2. **Format and mount LVM volumes**
   ```bash
   # Create file systems
   sudo mkfs.ext4 /dev/vg_data/lv_web
   sudo mkfs.xfs /dev/vg_data/lv_db
   sudo mkfs.ext4 /dev/vg_data/lv_backup
   
   # Create mount points and mount
   sudo mkdir -p /srv/{web,database,backup}
   sudo mount /dev/vg_data/lv_web /srv/web
   sudo mount /dev/vg_data/lv_db /srv/database
   sudo mount /dev/vg_data/lv_backup /srv/backup
   ```

3. **Resize LVM volumes**
   ```bash
   # Extend logical volume and file system
   sudo lvextend -L +200M /dev/vg_data/lv_web
   sudo resize2fs /dev/vg_data/lv_web
   
   # For XFS
   sudo lvextend -L +100M /dev/vg_data/lv_db
   sudo xfs_growfs /srv/database
   ```

**Verification:**
```bash
pvdisplay
vgdisplay
lvdisplay
df -h /srv/*
```

---

## Week 4: Network Configuration

### Lab 4.1: Network Interface Configuration
**Objective:** Configure network interfaces using NetworkManager

**Tasks:**
1. **Configure static IP with nmcli**
   ```bash
   # Create new connection
   sudo nmcli con add type ethernet con-name static-eth0 ifname eth0
   sudo nmcli con mod static-eth0 ipv4.addresses 192.168.1.10/24
   sudo nmcli con mod static-eth0 ipv4.gateway 192.168.1.1
   sudo nmcli con mod static-eth0 ipv4.dns "8.8.8.8,8.8.4.4"
   sudo nmcli con mod static-eth0 ipv4.method manual
   
   # Activate connection
   sudo nmcli con up static-eth0
   ```

2. **Configure hostname and DNS**
   ```bash
   # Set hostname
   sudo hostnamectl set-hostname server1.example.com
   
   # Configure DNS
   echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
   echo "search example.com" | sudo tee -a /etc/resolv.conf
   ```

**Verification:**
```bash
ip addr show
nmcli con show
hostnamectl
nslookup google.com
```

---

### Lab 4.2: Firewall Configuration
**Objective:** Configure firewalld for network security

**Tasks:**
1. **Basic firewall operations**
   ```bash
   # Check firewall status
   sudo firewall-cmd --state
   sudo firewall-cmd --get-default-zone
   
   # List current configuration
   sudo firewall-cmd --list-all
   ```

2. **Configure services and ports**
   ```bash
   # Add services
   sudo firewall-cmd --permanent --add-service=http
   sudo firewall-cmd --permanent --add-service=https
   sudo firewall-cmd --permanent --add-service=ssh
   
   # Add custom ports
   sudo firewall-cmd --permanent --add-port=8080/tcp
   sudo firewall-cmd --permanent --add-port=3306/tcp
   
   # Reload configuration
   sudo firewall-cmd --reload
   ```

3. **Create custom zones**
   ```bash
   # Create DMZ zone
   sudo firewall-cmd --permanent --new-zone=dmz
   sudo firewall-cmd --permanent --zone=dmz --add-service=http
   sudo firewall-cmd --permanent --zone=dmz --add-service=https
   sudo firewall-cmd --permanent --zone=dmz --add-interface=eth1
   sudo firewall-cmd --reload
   ```

---

## Week 5: Services and Process Management

### Lab 5.1: systemd Service Management
**Objective:** Master systemd service configuration and management

**Tasks:**
1. **Create custom systemd service**
   ```bash
   # Create a simple service script
   sudo tee /usr/local/bin/myapp.sh << 'EOF'
   #!/bin/bash
   while true; do
       echo "$(date): MyApp is running" >> /var/log/myapp.log
       sleep 30
   done
   EOF
   sudo chmod +x /usr/local/bin/myapp.sh
   
   # Create systemd service file
   sudo tee /etc/systemd/system/myapp.service << 'EOF'
   [Unit]
   Description=My Custom Application
   After=network.target
   
   [Service]
   Type=simple
   User=nobody
   ExecStart=/usr/local/bin/myapp.sh
   Restart=always
   RestartSec=10
   
   [Install]
   WantedBy=multi-user.target
   EOF
   ```

2. **Manage the service**
   ```bash
   # Reload systemd and start service
   sudo systemctl daemon-reload
   sudo systemctl enable myapp.service
   sudo systemctl start myapp.service
   
   # Check service status
   sudo systemctl status myapp.service
   sudo journalctl -u myapp.service -f
   ```

**Verification:**
```bash
sudo systemctl is-active myapp.service
sudo systemctl is-enabled myapp.service
tail -f /var/log/myapp.log
```

---

### Lab 5.2: Process Management and Scheduling
**Objective:** Master process control and job scheduling

**Tasks:**
1. **Process management**
   ```bash
   # Start background processes
   sleep 300 &
   dd if=/dev/zero of=/tmp/bigfile bs=1M count=1000 &
   
   # Monitor processes
   jobs
   ps aux | grep sleep
   pgrep -f "dd if"
   
   # Control processes
   kill -STOP %1  # Suspend first job
   kill -CONT %1  # Resume first job
   killall sleep
   ```

2. **Configure cron jobs**
   ```bash
   # Add system-wide cron job
   echo "0 2 * * * root /usr/bin/find /tmp -type f -mtime +7 -delete" | sudo tee -a /etc/crontab
   
   # Add user cron job
   crontab -e
   # Add: */5 * * * * /usr/bin/date >> /home/$(whoami)/timestamp.log
   
   # View cron jobs
   crontab -l
   sudo crontab -l
   ```

3. **Configure systemd timers**
   ```bash
   # Create timer for log cleanup
   sudo tee /etc/systemd/system/cleanup.service << 'EOF'
   [Unit]
   Description=Cleanup old log files
   
   [Service]
   Type=oneshot
   ExecStart=/usr/bin/find /var/log -name "*.log" -mtime +30 -delete
   EOF
   
   sudo tee /etc/systemd/system/cleanup.timer << 'EOF'
   [Unit]
   Description=Run cleanup daily
   Requires=cleanup.service
   
   [Timer]
   OnCalendar=daily
   Persistent=true
   
   [Install]
   WantedBy=timers.target
   EOF
   
   sudo systemctl enable cleanup.timer
   sudo systemctl start cleanup.timer
   ```

---

## Week 6: Security and SELinux

### Lab 6.1: SELinux Configuration
**Objective:** Master SELinux policies and troubleshooting

**Tasks:**
1. **SELinux basics**
   ```bash
   # Check SELinux status
   getenforce
   sestatus
   
   # View SELinux contexts
   ls -Z /var/www/html/
   ps -eZ | grep httpd
   id -Z
   ```

2. **Manage SELinux policies**
   ```bash
   # Install web server for testing
   sudo dnf install -y httpd
   sudo systemctl enable --now httpd
   
   # Create web content with wrong context
   sudo mkdir /web_content
   echo "<h1>Test Page</h1>" | sudo tee /web_content/index.html
   
   # Fix SELinux context
   sudo semanage fcontext -a -t httpd_exec_t "/web_content(/.*)?"
   sudo restorecon -Rv /web_content/
   
   # Configure httpd to use custom directory
   sudo setsebool -P httpd_enable_homedirs on
   ```

3. **Troubleshoot SELinux denials**
   ```bash
   # Generate SELinux denial (intentionally)
   sudo systemctl start httpd
   curl http://localhost/
   
   # Check for denials
   sudo ausearch -m AVC -ts recent
   sudo sealert -a /var/log/audit/audit.log
   ```

**Verification:**
```bash
getenforce
getsebool httpd_enable_homedirs
ls -Z /web_content/
```

---

### Lab 6.2: SSH and Key-based Authentication
**Objective:** Configure secure SSH access

**Tasks:**
1. **Configure SSH key authentication**
   ```bash
   # Generate SSH key pair
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_lab
   
   # Copy public key to server2
   ssh-copy-id -i ~/.ssh/id_rsa_lab.pub user@server2
   
   # Test key-based login
   ssh -i ~/.ssh/id_rsa_lab user@server2
   ```

2. **Harden SSH configuration**
   ```bash
   # Edit SSH daemon configuration
   sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
   
   # Modify SSH settings
   sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
   sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
   echo "AllowUsers alice bob" | sudo tee -a /etc/ssh/sshd_config
   
   # Restart SSH service
   sudo systemctl restart sshd
   ```

---

## Week 7: Container Management

### Lab 7.1: Podman Container Operations
**Objective:** Master container management with Podman

**Tasks:**
1. **Basic container operations**
   ```bash
   # Pull and run containers
   podman pull registry.redhat.io/ubi8/ubi:latest
   podman run -it --name test-container ubi8/ubi /bin/bash
   
   # List containers and images
   podman ps -a
   podman images
   
   # Container lifecycle
   podman start test-container
   podman stop test-container
   podman rm test-container
   ```

2. **Run containerized services**
   ```bash
   # Run web server container
   podman run -d --name web-server -p 8080:80 \
     -v /home/$(whoami)/web:/usr/share/nginx/html:Z \
     nginx:latest
   
   # Create persistent storage
   mkdir -p ~/web
   echo "<h1>Containerized Web Server</h1>" > ~/web/index.html
   
   # Test web server
   curl http://localhost:8080
   ```

3. **Create systemd services for containers**
   ```bash
   # Generate systemd unit file
   podman generate systemd --new --name web-server > ~/.config/systemd/user/web-server.service
   
   # Enable user service
   systemctl --user daemon-reload
   systemctl --user enable web-server.service
   systemctl --user start web-server.service
   ```

---

## Week 8: Advanced Topics and Exam Simulation

### Lab 8.1: System Monitoring and Logging
**Objective:** Master system monitoring and log analysis

**Tasks:**
1. **Configure rsyslog**
   ```bash
   # Create custom log configuration
   echo "local0.*    /var/log/myapp.log" | sudo tee /etc/rsyslog.d/myapp.conf
   sudo systemctl restart rsyslog
   
   # Test custom logging
   logger -p local0.info "Test message for myapp"
   tail /var/log/myapp.log
   ```

2. **Monitor system performance**
   ```bash
   # Install monitoring tools
   sudo dnf install -y htop iotop
   
   # Monitor system resources
   top
   htop
   iotop
   iostat 1 5
   vmstat 1 5
   
   # Check disk usage
   df -h
   du -sh /var/log/*
   ```

3. **Configure log rotation**
   ```bash
   # Create logrotate configuration
   sudo tee /etc/logrotate.d/myapp << 'EOF'
   /var/log/myapp.log {
       daily
       rotate 7
       compress
       delaycompress
       missingok
       notifempty
       postrotate
           /bin/systemctl reload rsyslog > /dev/null 2>&1 || true
       endscript
   }
   EOF
   
   # Test logrotate
   sudo logrotate -d /etc/logrotate.d/myapp
   ```

---

### Lab 8.2: Exam Simulation Scenarios
**Objective:** Practice real exam-style tasks

**Scenario 1: System Recovery**
```bash
# Simulate broken system and fix:
# 1. Reset root password
# 2. Fix fstab entry
# 3. Restore SELinux contexts
# 4. Fix network configuration
```

**Scenario 2: Complete System Setup**
```bash
# Set up a complete web server environment:
# 1. Configure users and groups
# 2. Set up LVM storage
# 3. Install and configure Apache
# 4. Configure firewall
# 5. Set up SSL certificates
# 6. Configure SELinux policies
```

**Scenario 3: Automation and Monitoring**
```bash
# Create automated backup system:
# 1. Write backup scripts
# 2. Configure cron jobs
# 3. Set up log monitoring
# 4. Create systemd services
# 5. Configure email notifications
```

---

## Final Exam Preparation Checklist

### Essential Commands to Master
- **File Operations:** `ls`, `cp`, `mv`, `rm`, `find`, `locate`, `which`
- **Permissions:** `chmod`, `chown`, `chgrp`, `setfacl`, `getfacl`
- **Text Processing:** `grep`, `sed`, `awk`, `sort`, `uniq`, `cut`
- **Archive:** `tar`, `gzip`, `gunzip`
- **Process Management:** `ps`, `top`, `kill`, `jobs`, `nohup`
- **Network:** `ip`, `nmcli`, `firewall-cmd`, `ss`, `ping`
- **Storage:** `fdisk`, `parted`, `mkfs`, `mount`, `lvm commands`
- **Services:** `systemctl`, `journalctl`
- **Users:** `useradd`, `usermod`, `userdel`, `passwd`, `chage`
- **SELinux:** `getenforce`, `setsebool`, `semanage`, `restorecon`

### Time Management Tips
- **Read all questions first** (5 minutes)
- **Allocate time per question** (~15 minutes each)
- **Verify your work** (30 minutes at end)
- **Use `history` command** to review what you've done

### Common Exam Gotchas
- Always make configurations persistent
- Check SELinux contexts after file operations
- Verify services start automatically after reboot
- Test network connectivity after configuration changes
- Use proper mount options in /etc/fstab

---

## Study Schedule Recommendations

### Daily Practice (2 hours)
- **Week 1-2:** Basic operations and file systems
- **Week 3-4:** Users, groups, and networking
- **Week 5-6:** Services and security
- **Week 7-8:** Advanced topics and exam simulation

### Weekly Review
- Practice all commands without looking at notes
- Complete full lab scenarios under time pressure
- Review and understand error messages
- Practice troubleshooting common issues

**Good luck with your RHCSA exam preparation!** 🚀
