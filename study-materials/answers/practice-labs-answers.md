# RHCSA Practice Labs - Answer Sheet

## Week 1 Labs: Linux Fundamentals - ANSWERS

### Lab 1.1: File System Navigation and Operations - ANSWERS

**Solution Steps:**

1. **Create directory structure:**
```bash
mkdir -p /home/student/projects/{web,db,backup}
```

2. **Create files with different permissions:**
```bash
# Create files in web directory
touch /home/student/projects/web/{index.html,style.css,script.js}
chmod 644 /home/student/projects/web/index.html
chmod 600 /home/student/projects/web/style.css
chmod 755 /home/student/projects/web/script.js

# Create files in db directory
touch /home/student/projects/db/{users.sql,config.conf}
chmod 640 /home/student/projects/db/users.sql
chmod 600 /home/student/projects/db/config.conf

# Create files in backup directory
touch /home/student/projects/backup/{daily.tar.gz,weekly.tar.gz}
chmod 644 /home/student/projects/backup/*.tar.gz
```

3. **Practice file operations:**
```bash
# Copy operations
cp /home/student/projects/web/index.html /tmp/
cp -r /home/student/projects/web /tmp/web_backup

# Move operations
mv /home/student/projects/db/config.conf /home/student/projects/db/database.conf

# Delete operations
rm /tmp/index.html
rm -rf /tmp/web_backup
```

4. **Use find to locate files:**
```bash
# Find HTML files
find /home/student -name "*.html" -type f

# Find files modified in last 24 hours
find /home/student -mtime -1

# Find files with specific permissions
find /home/student -perm 644

# Find directories
find /home/student -type d -name "web"

# Find files larger than 1KB
find /home/student -size +1k
```

5. **Create symbolic and hard links:**
```bash
# Create symbolic link
ln -s /home/student/projects/web/index.html /tmp/index_link

# Create hard link
ln /home/student/projects/web/index.html /tmp/index_hard

# Verify links
ls -la /tmp/index*
stat /home/student/projects/web/index.html
```

**Verification Commands:**
```bash
# Check directory structure
tree /home/student/projects/

# Check file permissions
ls -la /home/student/projects/web/
ls -la /home/student/projects/db/
ls -la /home/student/projects/backup/

# Verify links
ls -li /home/student/projects/web/index.html /tmp/index_hard
readlink /tmp/index_link
```

### Lab 1.2: User and Group Management - ANSWERS

**Solution Steps:**

1. **Create users:**
```bash
# Create users with home directories
useradd -m -c "Web Administrator" webadmin
useradd -m -c "Database Administrator" dbadmin
useradd -m -c "Backup User" backup_user

# Set passwords
echo "WebPass123!" | passwd --stdin webadmin
echo "DbPass123!" | passwd --stdin dbadmin
echo "BackupPass123!" | passwd --stdin backup_user
```

2. **Create groups:**
```bash
groupadd webteam
groupadd dbteam
groupadd backupteam
```

3. **Add users to groups:**
```bash
usermod -aG webteam webadmin
usermod -aG dbteam dbadmin
usermod -aG backupteam backup_user

# Add users to secondary groups
usermod -aG wheel webadmin  # For sudo access
```

4. **Set password policies:**
```bash
# Set password expiration (90 days)
chage -M 90 webadmin
chage -M 90 dbadmin
chage -M 90 backup_user

# Force password change on next login
chage -d 0 webadmin
chage -d 0 dbadmin
chage -d 0 backup_user
```

5. **Configure sudo access:**
```bash
# Edit sudoers file
visudo

# Add these lines:
# webadmin ALL=(ALL) ALL
# %dbteam ALL=(ALL) /usr/bin/systemctl
# backup_user ALL=(ALL) NOPASSWD: /usr/bin/tar, /usr/bin/rsync
```

**Verification Commands:**
```bash
# Check user creation
id webadmin
id dbadmin
id backup_user

# Check group memberships
groups webadmin
groups dbadmin
groups backup_user

# Check password aging
chage -l webadmin

# Test sudo access
sudo -u webadmin whoami
```

### Lab 1.3: File Permissions and ACLs - ANSWERS

**Solution Steps:**

1. **Create shared directories:**
```bash
mkdir -p /shared/{webteam,dbteam,common}
```

2. **Set up basic permissions:**
```bash
# Set group ownership
chgrp webteam /shared/webteam
chgrp dbteam /shared/dbteam
chgrp users /shared/common

# Set permissions
chmod 2775 /shared/webteam    # SGID bit set
chmod 2775 /shared/dbteam     # SGID bit set
chmod 1777 /shared/common     # Sticky bit set
```

3. **Configure ACLs:**
```bash
# Set ACLs for webteam directory
setfacl -m g:webteam:rwx /shared/webteam
setfacl -d -m g:webteam:rwx /shared/webteam
setfacl -m u:dbadmin:r-x /shared/webteam

# Set ACLs for dbteam directory
setfacl -m g:dbteam:rwx /shared/dbteam
setfacl -d -m g:dbteam:rwx /shared/dbteam
setfacl -m u:webadmin:r-x /shared/dbteam

# Set ACLs for common directory
setfacl -m g:webteam:rwx /shared/common
setfacl -m g:dbteam:rwx /shared/common
setfacl -d -m g:webteam:rwx /shared/common
setfacl -d -m g:dbteam:rwx /shared/common
```

4. **Test permission inheritance:**
```bash
# Create test files as different users
sudo -u webadmin touch /shared/webteam/web_file.txt
sudo -u dbadmin touch /shared/dbteam/db_file.txt
sudo -u backup_user touch /shared/common/common_file.txt
```

**Verification Commands:**
```bash
# Check permissions and ACLs
ls -la /shared/
getfacl /shared/webteam
getfacl /shared/dbteam
getfacl /shared/common

# Check file ownership and permissions
ls -la /shared/webteam/
ls -la /shared/dbteam/
ls -la /shared/common/

# Test access as different users
sudo -u webadmin ls /shared/dbteam
sudo -u dbadmin ls /shared/webteam
```

---

## Week 2 Labs: System Administration - ANSWERS

### Lab 2.1: Package Management - ANSWERS

**Solution Steps:**

1. **Configure additional repositories:**
```bash
# Add EPEL repository
dnf install -y epel-release

# Add custom repository (example)
cat > /etc/yum.repos.d/custom.repo << EOF
[custom]
name=Custom Repository
baseurl=http://example.com/repo
enabled=1
gpgcheck=0
EOF
```

2. **Install package groups:**
```bash
# List available groups
dnf group list

# Install Development Tools
dnf group install -y "Development Tools"

# Install specific packages
dnf install -y vim wget curl git htop
```

3. **Update system packages:**
```bash
# Update all packages
dnf update -y

# Update specific package
dnf update -y kernel
```

4. **Install local RPM packages:**
```bash
# Download an RPM package
wget https://example.com/package.rpm

# Install local RPM
dnf localinstall -y package.rpm

# Or using rpm command
rpm -ivh package.rpm
```

5. **Manage package dependencies:**
```bash
# Check package dependencies
dnf deplist httpd

# Remove package and dependencies
dnf autoremove httpd
```

**Verification Commands:**
```bash
# List installed packages
dnf list installed | grep -i development

# Check package information
dnf info vim

# List repositories
dnf repolist

# Check package history
dnf history
```

### Lab 2.2: Service Management with systemd - ANSWERS

**Solution Steps:**

1. **Install and configure Apache:**
```bash
# Install Apache
dnf install -y httpd

# Start and enable Apache
systemctl start httpd
systemctl enable httpd

# Create a simple web page
echo "<h1>Welcome to RHCSA Lab</h1>" > /var/www/html/index.html

# Configure firewall
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
```

2. **Create custom systemd service:**
```bash
# Create application directory
mkdir -p /opt/myapp

# Create simple script
cat > /opt/myapp/myapp.sh << 'EOF'
#!/bin/bash
while true; do
    echo "$(date): MyApp is running" >> /var/log/myapp.log
    sleep 30
done
EOF

chmod +x /opt/myapp/myapp.sh

# Create systemd service file
cat > /etc/systemd/system/myapp.service << 'EOF'
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/opt/myapp/myapp.sh
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF
```

3. **Configure service dependencies:**
```bash
# Reload systemd daemon
systemctl daemon-reload

# Start and enable custom service
systemctl start myapp.service
systemctl enable myapp.service
```

4. **Monitor service logs:**
```bash
# View service status
systemctl status myapp.service

# Follow logs in real-time
journalctl -u myapp.service -f

# View logs since boot
journalctl -u myapp.service -b
```

**Verification Commands:**
```bash
# Check Apache status
systemctl status httpd
curl http://localhost

# Check custom service
systemctl status myapp.service
tail -f /var/log/myapp.log

# List all services
systemctl list-units --type=service

# Check service dependencies
systemctl list-dependencies httpd
```

### Lab 2.3: Network Configuration - ANSWERS

**Solution Steps:**

1. **Configure static IP using nmcli:**
```bash
# Show current connections
nmcli connection show

# Create new static connection
nmcli connection add type ethernet con-name static-eth0 ifname eth0
nmcli connection modify static-eth0 ipv4.addresses 192.168.1.100/24
nmcli connection modify static-eth0 ipv4.gateway 192.168.1.1
nmcli connection modify static-eth0 ipv4.dns "8.8.8.8,8.8.4.4"
nmcli connection modify static-eth0 ipv4.method manual
nmcli connection modify static-eth0 connection.autoconnect yes

# Activate connection
nmcli connection up static-eth0
```

2. **Set up hostname resolution:**
```bash
# Set hostname
hostnamectl set-hostname rhcsa-lab.example.com

# Add entries to /etc/hosts
echo "192.168.1.100 rhcsa-lab.example.com rhcsa-lab" >> /etc/hosts
echo "192.168.1.101 server2.example.com server2" >> /etc/hosts
```

3. **Configure SSH for key-based authentication:**
```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""

# Copy public key to remote server (if available)
ssh-copy-id user@server2.example.com

# Configure SSH client
cat >> ~/.ssh/config << EOF
Host server2
    HostName server2.example.com
    User student
    IdentityFile ~/.ssh/id_rsa
EOF

chmod 600 ~/.ssh/config
```

4. **Test network connectivity:**
```bash
# Test connectivity
ping -c 4 8.8.8.8
ping -c 4 google.com

# Test DNS resolution
nslookup google.com
dig google.com

# Test SSH connection
ssh server2 'hostname'
```

**Verification Commands:**
```bash
# Check IP configuration
ip addr show
nmcli connection show static-eth0

# Check hostname
hostnamectl
hostname -f

# Check SSH configuration
ssh -T server2
ssh-keygen -l -f ~/.ssh/id_rsa.pub

# Check network connectivity
ss -tuln
netstat -rn
```

---

## Week 3 Labs: Advanced Administration - ANSWERS

### Lab 3.1: Firewall Configuration - ANSWERS

**Solution Steps:**

1. **Configure firewall zones:**
```bash
# List available zones
firewall-cmd --get-zones

# Check default zone
firewall-cmd --get-default-zone

# Set default zone
firewall-cmd --set-default-zone=public

# Add interface to zone
firewall-cmd --zone=public --add-interface=eth0 --permanent
```

2. **Allow specific services and ports:**
```bash
# Add services
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-service=https --permanent
firewall-cmd --zone=public --add-service=ssh --permanent

# Add custom ports
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=3306/tcp --permanent

# Add port range
firewall-cmd --zone=public --add-port=9000-9010/tcp --permanent
```

3. **Create rich rules:**
```bash
# Allow specific source IP
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" accept' --permanent

# Block specific IP
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.0.0.100" reject' --permanent

# Allow specific service from specific source
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.50" service name="ssh" accept' --permanent

# Rate limiting
firewall-cmd --zone=public --add-rich-rule='rule service name="ssh" accept limit value="3/m"' --permanent
```

4. **Test and reload firewall:**
```bash
# Reload firewall
firewall-cmd --reload

# Test configuration
firewall-cmd --zone=public --list-all
```

**Verification Commands:**
```bash
# Check firewall status
firewall-cmd --state

# List all zones and rules
firewall-cmd --list-all-zones

# Test port access
telnet localhost 8080
nmap -p 8080 localhost

# Check rich rules
firewall-cmd --zone=public --list-rich-rules
```

### Lab 3.2: SELinux Configuration - ANSWERS

**Solution Steps:**

1. **Set SELinux to enforcing mode:**
```bash
# Check current mode
getenforce

# Set to enforcing temporarily
setenforce 1

# Set to enforcing permanently
sed -i 's/SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config
```

2. **Configure file contexts for web server:**
```bash
# Create custom web directory
mkdir -p /custom/web
echo "<h1>Custom Web Content</h1>" > /custom/web/index.html

# Set SELinux context
semanage fcontext -a -t httpd_exec_t "/custom/web(/.*)?"
restorecon -R /custom/web

# Configure Apache to use custom directory
echo "DocumentRoot /custom/web" >> /etc/httpd/conf/httpd.conf
systemctl restart httpd
```

3. **Manage SELinux booleans:**
```bash
# List all booleans
getsebool -a | grep httpd

# Enable specific booleans
setsebool -P httpd_can_network_connect on
setsebool -P httpd_can_network_connect_db on
setsebool -P httpd_execmem on
```

4. **Troubleshoot SELinux denials:**
```bash
# Generate some denials (try accessing restricted content)
curl http://localhost/custom/

# Check for denials
ausearch -m avc -ts recent

# Analyze denials
sealert -a /var/log/audit/audit.log

# Generate policy rules
ausearch -m avc -ts recent | audit2allow -M mypolicy
semodule -i mypolicy.pp
```

**Verification Commands:**
```bash
# Check SELinux status
sestatus

# Check file contexts
ls -Z /custom/web/
ls -Z /var/www/html/

# Check process contexts
ps -Z | grep httpd

# Verify booleans
getsebool httpd_can_network_connect

# Test web server access
curl http://localhost/
```

### Lab 3.3: System Monitoring and Logging - ANSWERS

**Solution Steps:**

1. **Configure persistent journal logging:**
```bash
# Create journal directory
mkdir -p /var/log/journal

# Set permissions
chown root:systemd-journal /var/log/journal
chmod 2755 /var/log/journal

# Configure journald
cat >> /etc/systemd/journald.conf << EOF
Storage=persistent
SystemMaxUse=500M
SystemMaxFiles=10
EOF

# Restart journald
systemctl restart systemd-journald
```

2. **Set up log rotation:**
```bash
# Create custom log rotation config
cat > /etc/logrotate.d/myapp << EOF
/var/log/myapp.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 644 nobody nobody
}
EOF

# Test log rotation
logrotate -d /etc/logrotate.d/myapp
```

3. **Monitor system performance:**
```bash
# Install monitoring tools
dnf install -y sysstat iotop

# Enable sysstat
systemctl enable --now sysstat

# Create monitoring script
cat > /usr/local/bin/monitor.sh << 'EOF'
#!/bin/bash
echo "=== System Monitor Report ===" >> /var/log/system-monitor.log
echo "Date: $(date)" >> /var/log/system-monitor.log
echo "Load Average: $(uptime | awk -F'load average:' '{print $2}')" >> /var/log/system-monitor.log
echo "Memory Usage:" >> /var/log/system-monitor.log
free -h >> /var/log/system-monitor.log
echo "Disk Usage:" >> /var/log/system-monitor.log
df -h >> /var/log/system-monitor.log
echo "================================" >> /var/log/system-monitor.log
EOF

chmod +x /usr/local/bin/monitor.sh
```

4. **Create custom log entries:**
```bash
# Create custom log entries
logger -p local0.info "Custom application started"
logger -p local0.warning "Custom warning message"
logger -p local0.error "Custom error message"

# Configure rsyslog for custom facility
echo "local0.*    /var/log/custom.log" >> /etc/rsyslog.conf
systemctl restart rsyslog
```

**Verification Commands:**
```bash
# Check journal persistence
journalctl --disk-usage
ls -la /var/log/journal/

# Check system logs
journalctl -f
journalctl --since "1 hour ago"
journalctl -p err

# Check performance monitoring
sar -u 1 5
iostat 1 5
top -n 1

# Check custom logs
tail -f /var/log/custom.log
tail -f /var/log/system-monitor.log
```
