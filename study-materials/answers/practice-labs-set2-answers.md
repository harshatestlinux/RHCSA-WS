# RHCSA Practice Labs Set 2 - Answer Sheet

## Week 1 Labs: Advanced Linux Fundamentals - ANSWERS

### Lab 2.1: Advanced File Operations and Text Processing - ANSWERS

**Solution Steps:**

1. **Create web development project structure:**
```bash
mkdir -p /opt/webdev/{src/{html,css,js},logs/{access,error},backups/{daily,weekly},config/{apache,php}}

# Create sample files
touch /opt/webdev/src/html/{index.html,about.html,contact.html}
touch /opt/webdev/src/css/{main.css,responsive.css}
touch /opt/webdev/src/js/{app.js,utils.js}
touch /opt/webdev/config/apache/{httpd.conf,ssl.conf}
```

2. **Advanced find commands:**
```bash
# Files modified in last 7 days larger than 1MB
find /var -type f -mtime -7 -size +1M 2>/dev/null

# Find files by multiple criteria
find /opt/webdev -type f \( -name "*.html" -o -name "*.css" \) -exec ls -la {} \;

# Find and execute commands
find /opt/webdev -name "*.js" -exec grep -l "function" {} \;

# Find with complex permissions
find /opt/webdev -type f -perm 644 -user root
```

3. **Process log files:**
```bash
# Create sample Apache log
cat > /opt/webdev/logs/access/access.log << 'EOF'
192.168.1.100 - - [25/Jul/2025:10:30:45 +0000] "GET /index.html HTTP/1.1" 200 1234
192.168.1.101 - - [25/Jul/2025:10:31:15 +0000] "POST /contact.php HTTP/1.1" 404 567
192.168.1.102 - - [25/Jul/2025:10:32:30 +0000] "GET /about.html HTTP/1.1" 200 890
EOF

# Extract IP addresses
grep -o '^[0-9.]*' /opt/webdev/logs/access/access.log

# Extract 404 errors
awk '$9 == "404" {print $1, $7}' /opt/webdev/logs/access/access.log

# Count requests per IP
awk '{print $1}' /opt/webdev/logs/access/access.log | sort | uniq -c

# Extract specific time range
sed -n '/10:31:00/,/10:32:00/p' /opt/webdev/logs/access/access.log
```

4. **Create compressed archives:**
```bash
# Create tar archive
tar -cf /opt/webdev/backups/daily/webdev-$(date +%Y%m%d).tar /opt/webdev/src/

# Create gzip compressed archive
tar -czf /opt/webdev/backups/daily/webdev-$(date +%Y%m%d).tar.gz /opt/webdev/src/

# Create bzip2 compressed archive
tar -cjf /opt/webdev/backups/weekly/webdev-$(date +%Y%m%d).tar.bz2 /opt/webdev/

# Create zip archive
zip -r /opt/webdev/backups/daily/webdev-$(date +%Y%m%d).zip /opt/webdev/src/
```

5. **File organization script:**
```bash
cat > /usr/local/bin/organize_files.sh << 'EOF'
#!/bin/bash

# File organization script
SOURCE_DIR="/tmp/organize"
TARGET_DIR="/tmp/organized"

# Create target directories
mkdir -p $TARGET_DIR/{documents,images,scripts,archives,others}

# Organize files by extension
for file in $SOURCE_DIR/*; do
    if [ -f "$file" ]; then
        case "${file##*.}" in
            txt|doc|pdf|docx)
                mv "$file" "$TARGET_DIR/documents/"
                ;;
            jpg|jpeg|png|gif|bmp)
                mv "$file" "$TARGET_DIR/images/"
                ;;
            sh|py|pl|rb)
                mv "$file" "$TARGET_DIR/scripts/"
                ;;
            tar|gz|zip|bz2)
                mv "$file" "$TARGET_DIR/archives/"
                ;;
            *)
                mv "$file" "$TARGET_DIR/others/"
                ;;
        esac
        echo "Moved $(basename $file) to appropriate directory"
    fi
done
EOF

chmod +x /usr/local/bin/organize_files.sh
```

### Lab 2.2: Advanced User and Group Management - ANSWERS

**Solution Steps:**

1. **Create multi-tier user structure:**
```bash
# Create department groups
groupadd it_dept
groupadd hr_dept
groupadd finance_dept

# Create IT users
useradd -m -G it_dept -c "IT Administrator" itadmin
useradd -m -G it_dept -c "Network Engineer" neteng
useradd -m -G it_dept -c "System Administrator" sysadmin

# Create HR users
useradd -m -G hr_dept -c "HR Manager" hrmgr
useradd -m -G hr_dept -c "HR Assistant" hrassist

# Create Finance users
useradd -m -G finance_dept -c "Finance Manager" finmgr
useradd -m -G finance_dept -c "Accountant" accountant

# Set passwords
for user in itadmin neteng sysadmin hrmgr hrassist finmgr accountant; do
    echo "TempPass123!" | passwd --stdin $user
done
```

2. **Implement password policies:**
```bash
# IT Department - 60 days max, 7 days min, 7 days warning
for user in itadmin neteng sysadmin; do
    chage -M 60 -m 7 -W 7 $user
done

# HR Department - 90 days max, 1 day min, 14 days warning
for user in hrmgr hrassist; do
    chage -M 90 -m 1 -W 14 $user
done

# Finance Department - 45 days max, 7 days min, 10 days warning
for user in finmgr accountant; do
    chage -M 45 -m 7 -W 10 $user
done
```

3. **Set up user quotas:**
```bash
# Enable quotas on filesystem (assuming /home is separate)
# Add usrquota to /etc/fstab for /home filesystem
# /dev/sdb1 /home ext4 defaults,usrquota 0 2

# Remount with quota support
mount -o remount /home

# Create quota files
quotacheck -cum /home
quotaon /home

# Set quotas for users (1GB soft, 1.5GB hard limit)
for user in itadmin neteng sysadmin hrmgr hrassist finmgr accountant; do
    setquota -u $user 1000000 1500000 0 0 /home
done
```

4. **Configure advanced sudo rules:**
```bash
# Create sudoers files for each department
cat > /etc/sudoers.d/it_dept << 'EOF'
# IT Department sudo rules
%it_dept ALL=(ALL) ALL
itadmin ALL=(ALL) NOPASSWD: ALL
neteng ALL=(ALL) /sbin/iptables, /usr/bin/systemctl, /usr/bin/nmcli
sysadmin ALL=(ALL) /usr/bin/systemctl, /usr/sbin/service, /bin/mount, /bin/umount
EOF

cat > /etc/sudoers.d/hr_dept << 'EOF'
# HR Department sudo rules
%hr_dept ALL=(ALL) /usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root
hrmgr ALL=(ALL) /usr/sbin/useradd, /usr/sbin/usermod, /usr/sbin/userdel
EOF

cat > /etc/sudoers.d/finance_dept << 'EOF'
# Finance Department sudo rules
finmgr ALL=(ALL) /usr/bin/find /home -name "*.xls*", /usr/bin/tar
accountant ALL=(ALL) /usr/bin/find /home -name "*.pdf"
EOF
```

5. **Create shared directories with ACLs:**
```bash
# Create project directories
mkdir -p /projects/{it_projects,hr_projects,finance_projects,shared}

# Set basic permissions and ownership
chgrp it_dept /projects/it_projects
chgrp hr_dept /projects/hr_projects
chgrp finance_dept /projects/finance_projects
chmod 2775 /projects/{it_projects,hr_projects,finance_projects}
chmod 1777 /projects/shared

# Configure ACLs
# IT projects - IT full access, others read-only
setfacl -m g:it_dept:rwx /projects/it_projects
setfacl -m g:hr_dept:r-x /projects/it_projects
setfacl -m g:finance_dept:r-x /projects/it_projects
setfacl -d -m g:it_dept:rwx /projects/it_projects

# HR projects - HR full access, IT read access, Finance no access
setfacl -m g:hr_dept:rwx /projects/hr_projects
setfacl -m g:it_dept:r-x /projects/hr_projects
setfacl -m g:finance_dept:--- /projects/hr_projects
setfacl -d -m g:hr_dept:rwx /projects/hr_projects

# Finance projects - Finance full access, others no access
setfacl -m g:finance_dept:rwx /projects/finance_projects
setfacl -m g:it_dept:--- /projects/finance_projects
setfacl -m g:hr_dept:--- /projects/finance_projects
setfacl -d -m g:finance_dept:rwx /projects/finance_projects
```

### Lab 2.3: Advanced File Permissions and Security - ANSWERS

**Solution Steps:**

1. **Set up secure file sharing environment:**
```bash
# Create security level directories
mkdir -p /secure/{public,confidential,secret,top_secret}

# Set different security levels
chmod 755 /secure/public
chmod 750 /secure/confidential
chmod 700 /secure/secret
chmod 700 /secure/top_secret

# Create security groups
groupadd security_public
groupadd security_confidential
groupadd security_secret
groupadd security_top_secret

# Set group ownership
chgrp security_public /secure/public
chgrp security_confidential /secure/confidential
chgrp security_secret /secure/secret
chgrp security_top_secret /secure/top_secret
```

2. **Implement advanced ACLs with inheritance:**
```bash
# Public - everyone can read
setfacl -m g:security_public:r-x /secure/public
setfacl -d -m g:security_public:r-x /secure/public
setfacl -m other:r-x /secure/public

# Confidential - specific users only
setfacl -m g:security_confidential:rwx /secure/confidential
setfacl -m u:itadmin:rwx /secure/confidential
setfacl -m u:hrmgr:r-x /secure/confidential
setfacl -d -m g:security_confidential:rwx /secure/confidential

# Secret - very limited access
setfacl -m g:security_secret:rwx /secure/secret
setfacl -m u:itadmin:rwx /secure/secret
setfacl -d -m g:security_secret:rwx /secure/secret
setfacl -m other:--- /secure/secret

# Top Secret - admin only
setfacl -m u:itadmin:rwx /secure/top_secret
setfacl -m other:--- /secure/top_secret
setfacl -m group:--- /secure/top_secret
```

3. **Configure special permissions:**
```bash
# Create SUID program directory
mkdir -p /opt/suid_programs
chmod 755 /opt/suid_programs

# Create SGID collaboration directory
mkdir -p /opt/collaboration
chgrp it_dept /opt/collaboration
chmod 2775 /opt/collaboration

# Create sticky bit directory (only owners can delete)
mkdir -p /tmp/shared_temp
chmod 1777 /tmp/shared_temp

# Example SUID script (be careful with real SUID scripts)
cat > /opt/suid_programs/check_logs.sh << 'EOF'
#!/bin/bash
# This script allows users to check specific logs
tail -n 20 /var/log/messages
EOF
chmod 4755 /opt/suid_programs/check_logs.sh
```

4. **Create secure backup directory:**
```bash
# Create backup structure
mkdir -p /backups/{daily,weekly,monthly,archive}

# Create backup group
groupadd backup_operators

# Set permissions
chgrp backup_operators /backups
chmod 750 /backups
chmod 770 /backups/{daily,weekly,monthly}
chmod 750 /backups/archive

# Add backup users to group
usermod -aG backup_operators sysadmin
usermod -aG backup_operators itadmin

# Set ACLs for backup access
setfacl -m g:backup_operators:rwx /backups/{daily,weekly,monthly}
setfacl -m g:backup_operators:r-x /backups/archive
setfacl -d -m g:backup_operators:rwx /backups/{daily,weekly,monthly}
```

5. **Implement file integrity monitoring:**
```bash
# Install AIDE (Advanced Intrusion Detection Environment)
dnf install -y aide

# Initialize AIDE database
aide --init
mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz

# Create custom AIDE configuration for critical files
cat >> /etc/aide.conf << 'EOF'
# Custom rules for critical system files
/etc/passwd f+p+u+g+s+m+c+md5
/etc/shadow f+p+u+g+s+m+c+md5
/etc/sudoers f+p+u+g+s+m+c+md5
/boot f+p+u+g+s+m+c+md5
EOF

# Create monitoring script
cat > /usr/local/bin/file_integrity_check.sh << 'EOF'
#!/bin/bash
# File integrity monitoring script
LOGFILE="/var/log/aide_check.log"
DATE=$(date)

echo "=== AIDE Check - $DATE ===" >> $LOGFILE
aide --check >> $LOGFILE 2>&1

if [ $? -ne 0 ]; then
    echo "WARNING: File integrity violations detected!" >> $LOGFILE
    # Send alert (configure mail or logging as needed)
fi
EOF

chmod +x /usr/local/bin/file_integrity_check.sh

# Schedule daily integrity checks
echo "0 2 * * * root /usr/local/bin/file_integrity_check.sh" >> /etc/crontab
```

**Verification Commands:**
```bash
# Verify user structure
for user in itadmin neteng sysadmin hrmgr hrassist finmgr accountant; do
    echo "User: $user"
    id $user
    chage -l $user
    echo "---"
done

# Verify quotas
repquota -u /home

# Verify ACLs
for dir in /projects/* /secure/*; do
    echo "Directory: $dir"
    getfacl $dir
    echo "---"
done

# Test sudo access
sudo -u neteng -l
sudo -u hrmgr -l

# Test file integrity
aide --check
```

---

## Week 2 Labs: Advanced System Administration - ANSWERS

### Lab 2.4: Advanced Package Management and Repositories - ANSWERS

**Solution Steps:**

1. **Set up local YUM/DNF repository:**
```bash
# Create repository directory
mkdir -p /var/www/html/localrepo

# Download some RPM packages
dnf download --downloaddir /var/www/html/localrepo httpd php mariadb-server

# Create repository metadata
createrepo /var/www/html/localrepo

# Configure local repository
cat > /etc/yum.repos.d/local.repo << 'EOF'
[local]
name=Local Repository
baseurl=file:///var/www/html/localrepo
enabled=1
gpgcheck=0
EOF

# Test repository
dnf repolist
dnf --disablerepo="*" --enablerepo="local" list available
```

2. **Create custom RPM package:**
```bash
# Install RPM build tools
dnf install -y rpm-build rpmdevtools

# Set up build environment
rpmdev-setuptree

# Create simple application
mkdir -p /tmp/myapp-1.0
cat > /tmp/myapp-1.0/myapp.sh << 'EOF'
#!/bin/bash
echo "MyApp version 1.0 is running"
echo "Current date: $(date)"
EOF
chmod +x /tmp/myapp-1.0/myapp.sh

# Create tarball
cd /tmp
tar -czf myapp-1.0.tar.gz myapp-1.0/

# Move to SOURCES
mv myapp-1.0.tar.gz ~/rpmbuild/SOURCES/

# Create spec file
cat > ~/rpmbuild/SPECS/myapp.spec << 'EOF'
Name:           myapp
Version:        1.0
Release:        1%{?dist}
Summary:        Simple application example
License:        GPL
Source0:        %{name}-%{version}.tar.gz
BuildArch:      noarch

%description
A simple example application for RPM building demonstration.

%prep
%setup -q

%install
mkdir -p %{buildroot}/usr/local/bin
install -m 755 myapp.sh %{buildroot}/usr/local/bin/myapp

%files
/usr/local/bin/myapp

%changelog
* Fri Jul 25 2025 Student <student@example.com> - 1.0-1
- Initial package
EOF

# Build RPM
rpmbuild -ba ~/rpmbuild/SPECS/myapp.spec

# Install custom RPM
rpm -ivh ~/rpmbuild/RPMS/noarch/myapp-1.0-1.*.noarch.rpm
```

3. **Manage package dependencies and conflicts:**
```bash
# Check dependencies
dnf deplist httpd

# Resolve conflicts manually
dnf install --allowerasing conflicting-package

# Download dependencies only
dnf download --resolve httpd

# Install with specific version
dnf install httpd-2.4.53

# Downgrade package
dnf downgrade httpd
```

4. **Configure package priorities and exclusions:**
```bash
# Install priorities plugin
dnf install -y dnf-plugin-priorities

# Configure repository priorities
cat >> /etc/yum.repos.d/local.repo << 'EOF'
priority=1
EOF

# Exclude packages from updates
cat >> /etc/dnf/dnf.conf << 'EOF'
exclude=kernel* httpd
EOF

# Or exclude from specific repository
cat >> /etc/yum.repos.d/epel.repo << 'EOF'
exclude=nginx* apache*
EOF
```

5. **Implement automated package updates:**
```bash
# Install dnf-automatic
dnf install -y dnf-automatic

# Configure automatic updates for security only
cat > /etc/dnf/automatic.conf << 'EOF'
[commands]
upgrade_type = security
random_sleep = 3600
download_updates = yes
apply_updates = yes

[emitters]
emit_via = stdio

[email]
email_from = root@localhost
email_to = admin@example.com
email_host = localhost
EOF

# Enable and start automatic updates
systemctl enable --now dnf-automatic.timer
```

### Lab 2.5: Advanced Service Management and Systemd - ANSWERS

**Solution Steps:**

1. **Create multi-service application stack:**
```bash
# Create application directories
mkdir -p /opt/webapp/{bin,config,logs}

# Create database service
cat > /etc/systemd/system/webapp-db.service << 'EOF'
[Unit]
Description=WebApp Database Service
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/sleep infinity
ExecStartPost=/bin/bash -c 'echo "Database started" >> /opt/webapp/logs/db.log'
ExecStop=/bin/bash -c 'echo "Database stopped" >> /opt/webapp/logs/db.log'
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Create web service that depends on database
cat > /etc/systemd/system/webapp-web.service << 'EOF'
[Unit]
Description=WebApp Web Service
After=network.target webapp-db.service
Requires=webapp-db.service

[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/sleep infinity
ExecStartPost=/bin/bash -c 'echo "Web service started" >> /opt/webapp/logs/web.log'
ExecStop=/bin/bash -c 'echo "Web service stopped" >> /opt/webapp/logs/web.log'
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Create target for the entire stack
cat > /etc/systemd/system/webapp.target << 'EOF'
[Unit]
Description=WebApp Complete Stack
Requires=webapp-db.service webapp-web.service
After=webapp-db.service webapp-web.service

[Install]
WantedBy=multi-user.target
EOF
```

2. **Configure service resource limits:**
```bash
# Add resource limits to web service
cat >> /etc/systemd/system/webapp-web.service << 'EOF'

# Resource limits
MemoryLimit=512M
CPUQuota=50%
TasksMax=100
EOF

# Create drop-in directory for additional limits
mkdir -p /etc/systemd/system/webapp-db.service.d

cat > /etc/systemd/system/webapp-db.service.d/limits.conf << 'EOF'
[Service]
MemoryLimit=1G
CPUQuota=75%
IOWeight=500
EOF
```

3. **Set up service monitoring and restart:**
```bash
# Configure restart policies
cat >> /etc/systemd/system/webapp-web.service << 'EOF'
RestartSec=10
StartLimitInterval=60
StartLimitBurst=3
EOF

# Create monitoring script
cat > /opt/webapp/bin/monitor.sh << 'EOF'
#!/bin/bash
LOGFILE="/opt/webapp/logs/monitor.log"

while true; do
    if ! systemctl is-active --quiet webapp-web.service; then
        echo "$(date): WebApp web service is down, attempting restart" >> $LOGFILE
        systemctl restart webapp-web.service
    fi
    
    if ! systemctl is-active --quiet webapp-db.service; then
        echo "$(date): WebApp database service is down, attempting restart" >> $LOGFILE
        systemctl restart webapp-db.service
    fi
    
    sleep 30
done
EOF

chmod +x /opt/webapp/bin/monitor.sh

# Create monitoring service
cat > /etc/systemd/system/webapp-monitor.service << 'EOF'
[Unit]
Description=WebApp Monitor Service
After=webapp.target

[Service]
Type=simple
ExecStart=/opt/webapp/bin/monitor.sh
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

4. **Create systemd timers for maintenance:**
```bash
# Create log cleanup service
cat > /etc/systemd/system/webapp-cleanup.service << 'EOF'
[Unit]
Description=WebApp Log Cleanup

[Service]
Type=oneshot
ExecStart=/bin/find /opt/webapp/logs -name "*.log" -mtime +7 -delete
ExecStartPost=/bin/bash -c 'echo "$(date): Log cleanup completed" >> /opt/webapp/logs/maintenance.log'
EOF

# Create timer for cleanup
cat > /etc/systemd/system/webapp-cleanup.timer << 'EOF'
[Unit]
Description=WebApp Daily Cleanup Timer
Requires=webapp-cleanup.service

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
EOF

# Create backup service
cat > /etc/systemd/system/webapp-backup.service << 'EOF'
[Unit]
Description=WebApp Backup Service

[Service]
Type=oneshot
ExecStart=/bin/tar -czf /backups/webapp-$(date +\%Y\%m\%d).tar.gz /opt/webapp
ExecStartPost=/bin/bash -c 'echo "$(date): Backup completed" >> /opt/webapp/logs/backup.log'
EOF

# Create timer for backup
cat > /etc/systemd/system/webapp-backup.timer << 'EOF'
[Unit]
Description=WebApp Weekly Backup Timer
Requires=webapp-backup.service

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
EOF
```

5. **Enable and test services:**
```bash
# Reload systemd
systemctl daemon-reload

# Enable services
systemctl enable webapp-db.service webapp-web.service webapp-monitor.service
systemctl enable webapp-cleanup.timer webapp-backup.timer

# Start services
systemctl start webapp.target
systemctl start webapp-cleanup.timer webapp-backup.timer

# Test service dependencies
systemctl stop webapp-db.service
systemctl status webapp-web.service
```

**Verification Commands:**
```bash
# Check service status
systemctl status webapp.target
systemctl list-dependencies webapp.target

# Check resource usage
systemctl show webapp-web.service | grep -E "(Memory|CPU|Tasks)"

# Check timers
systemctl list-timers

# Check logs
journalctl -u webapp-web.service
journalctl -u webapp-db.service

# Test restart behavior
systemctl kill webapp-web.service
sleep 15
systemctl status webapp-web.service
```

This comprehensive answer sheet provides detailed solutions for the advanced practice labs. The solutions include proper command syntax, configuration files, and verification steps to ensure everything works correctly. Each lab builds upon previous knowledge while introducing more complex scenarios that mirror real-world RHCSA exam requirements.
