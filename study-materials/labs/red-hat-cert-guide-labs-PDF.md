# Red Hat RHCSA 9 Cert Guide - Practice Labs
*Based on Official Red Hat RHCSA 9 Cert Guide: EX200*

## 📚 Study Resources
- **Red Hat RHCSA 9 Cert Guide** - Official certification guide (content extracted)
- **Lab Environment:** 2 RHEL 9 VMs following book specifications

---

## Part I: Performing Basic System Management Tasks

### Chapter 1 Lab: Installing Red Hat Enterprise Linux
**Reference:** Chapter 1 - Lab 1.1 (Page 24)

**Objective:** Master RHEL 9 installation and initial configuration

**Pre-Lab Setup:**
```bash
# VM Requirements (from Cert Guide)
# - 4GB RAM minimum
# - 40GB disk space
# - Network connectivity
# - RHEL 9 ISO or access to installation media
```

**Tasks:**
1. **Perform Custom Installation**
   - Configure custom partitioning scheme
   - Set up separate `/home`, `/var`, and `/tmp` partitions
   - Configure swap space (2GB)
   - Set root password and create user account

2. **Post-Installation Configuration**
   ```bash
   # Register system (if using RHEL)
   sudo subscription-manager register --username your_username
   sudo subscription-manager attach --auto
   
   # Update system
   sudo dnf update -y
   
   # Verify installation
   cat /etc/redhat-release
   uname -a
   df -h
   ```

---

### Chapter 2 Lab: Using Essential Tools
**Reference:** Chapter 2 - Lab 2.1 (Page 51)

**Objective:** Master shell skills, I/O redirection, and help systems

**Tasks:**
1. **Shell Skills and Command Execution**
   ```bash
   # Practice command execution and history
   history | tail -20
   !-2  # Execute second-to-last command
   
   # Use command substitution
   echo "Today is $(date)"
   echo "Current user: $(whoami)"
   ```

2. **I/O Redirection and Pipes**
   ```bash
   # Create test files for practice
   echo "Line 1" > testfile.txt
   echo "Line 2" >> testfile.txt
   echo "Line 3" >> testfile.txt
   
   # Practice redirection
   ls -la > directory_listing.txt
   ls -la /nonexistent 2> error.log
   ls -la / &> complete_output.txt
   
   # Practice pipes
   cat /etc/passwd | grep root
   ps aux | grep systemd | head -5
   dmesg | tail -20 | grep -i error
   ```

3. **Using Help Systems**
   ```bash
   # Practice with man pages
   man ls
   man 5 passwd  # Section 5 for file formats
   
   # Use apropos to find commands
   apropos "copy files"
   apropos network
   
   # Use info pages
   info coreutils
   
   # Check documentation
   ls /usr/share/doc/ | head -10
   ```

**Verification:**
```bash
# Test your shell skills
history | wc -l
echo $SHELL
env | grep PATH
```

---

### Chapter 3 Lab: Essential File Management Tools
**Reference:** Chapter 3 - Lab 3.1 (Page 77)

**Objective:** Master file operations, links, and archives

**Tasks:**
1. **File System Navigation and Management**
   ```bash
   # Create directory structure
   mkdir -p ~/lab3/{docs,scripts,backup,temp}
   cd ~/lab3
   
   # Practice with wildcards
   touch docs/file{1..5}.txt
   touch scripts/script{A..C}.sh
   ls docs/file*.txt
   ls scripts/script[A-C].sh
   ```

2. **Working with Links (Critical for RHCSA)**
   ```bash
   # Create original file
   echo "Original content for linking practice" > ~/lab3/original.txt
   
   # Create hard link
   ln ~/lab3/original.txt ~/lab3/hardlink.txt
   
   # Create symbolic link
   ln -s ~/lab3/original.txt ~/lab3/symlink.txt
   
   # Verify link properties
   ls -li ~/lab3/
   stat ~/lab3/original.txt
   stat ~/lab3/hardlink.txt
   stat ~/lab3/symlink.txt
   
   # Test link behavior
   echo "Additional content" >> ~/lab3/hardlink.txt
   cat ~/lab3/original.txt
   cat ~/lab3/symlink.txt
   ```

3. **Archive Management with tar**
   ```bash
   # Create archives (essential RHCSA skill)
   tar -czf ~/lab3/backup/docs_backup.tar.gz ~/lab3/docs/
   tar -cjf ~/lab3/backup/scripts_backup.tar.bz2 ~/lab3/scripts/
   
   # List archive contents
   tar -tzf ~/lab3/backup/docs_backup.tar.gz
   tar -tjf ~/lab3/backup/scripts_backup.tar.bz2
   
   # Extract archives
   mkdir ~/lab3/restore_test
   tar -xzf ~/lab3/backup/docs_backup.tar.gz -C ~/lab3/restore_test/
   
   # Verify extraction
   ls -la ~/lab3/restore_test/
   ```

---

### Chapter 4 Lab: Working with Text Files (Enhanced)
**Reference:** Chapter 4 - Lab 4.1 (Page 98)

**Objective:** Master text processing tools and regular expressions with deep understanding
**Time:** 75 minutes (extended for comprehensive practice)

**Learning Goals:**
- Understand text processing pipeline concepts
- Master essential text analysis tools
- Learn regular expression patterns and matching
- Practice log file analysis techniques
- Understand field separation and data extraction

**Background Knowledge:**
- **Stream Processing:** Text tools process data line by line
- **Field Separators:** Characters that divide data into columns (space, tab, comma)
- **Regular Expressions:** Pattern matching language for text search
- **Pipes:** Connect output of one command to input of another
- **Standard Streams:** stdin (input), stdout (output), stderr (errors)

**Tasks with Detailed Explanations:**

1. **Text File Analysis Tools**
   ```bash
   # Create sample log file for practice
   cat > ~/lab4/sample.log << 'EOF'
   2024-01-15 10:30:15 INFO User alice logged in
   2024-01-15 10:31:22 ERROR Failed login attempt for user bob
   2024-01-15 10:32:45 INFO User charlie logged in
   2024-01-15 10:33:10 WARNING Disk space low on /var
   2024-01-15 10:34:55 ERROR Database connection failed
   2024-01-15 10:35:30 INFO User alice logged out
   EOF
   
   # head - Display first lines of file
   head -3 ~/lab4/sample.log
   # What happens:
   # - Shows first 3 lines of the file
   # - Useful for quickly seeing file structure
   # - Default is 10 lines if no number specified
   # - Essential for examining large log files
   
   # tail - Display last lines of file
   tail -2 ~/lab4/sample.log
   # What happens:
   # - Shows last 2 lines of the file
   # - Useful for seeing recent log entries
   # - tail -f follows file as it grows (live monitoring)
   # - Critical for real-time log analysis
   
   # wc - Word, line, character, and byte counts
   wc -l ~/lab4/sample.log
   # What happens:
   # - Counts number of lines in file
   # - -l: lines, -w: words, -c: characters, -m: characters
   # - Useful for measuring file size and content volume
   
   wc -lwc ~/lab4/sample.log
   # Shows: lines, words, characters in that order
   
   # cut - Extract specific fields/columns
   cut -d' ' -f1,2 ~/lab4/sample.log
   # What happens:
   # - -d' ': uses space as field delimiter
   # - -f1,2: extracts fields 1 and 2 (date and time)
   # - Creates columnar output from structured text
   # - Essential for parsing log files and CSV data
   
   cut -d' ' -f3 ~/lab4/sample.log
   # Extracts just the log level (INFO, ERROR, WARNING)
   
   # sort - Sort lines alphabetically or numerically
   sort ~/lab4/sample.log
   # What happens:
   # - Sorts lines alphabetically by default
   # - Useful for organizing data and finding duplicates
   # - Can sort by specific fields with -k option
   
   sort -k3 ~/lab4/sample.log
   # Sorts by third field (log level)
   
   # uniq - Remove or report duplicate lines
   cut -d' ' -f3 ~/lab4/sample.log | sort | uniq -c
   # What happens:
   # - Extracts log levels, sorts them, counts occurrences
   # - -c: shows count of each unique line
   # - Must sort before uniq for proper duplicate detection
   ```

2. **Advanced Text Processing**
   ```bash
   # awk - Pattern scanning and processing
   awk '{print $1, $2, $3}' ~/lab4/sample.log
   # What happens:
   # - $1, $2, $3: first, second, third fields
   # - Automatically splits on whitespace
   # - More powerful than cut for complex field processing
   
   awk '$3 == "ERROR" {print $0}' ~/lab4/sample.log
   # What happens:
   # - Prints entire line ($0) where third field equals "ERROR"
   # - Conditional processing based on field values
   # - More flexible than grep for structured data
   
   # sed - Stream editor for filtering and transforming text
   sed 's/ERROR/CRITICAL/g' ~/lab4/sample.log
   # What happens:
   # - s/old/new/g: substitute 'ERROR' with 'CRITICAL' globally
   # - g flag: replace all occurrences on each line
   # - Powerful for text transformation and cleanup
   
   sed -n '2,4p' ~/lab4/sample.log
   # What happens:
   # - -n: suppress default output
   # - 2,4p: print lines 2 through 4
   # - Alternative to head/tail for specific line ranges
   ```

3. **Text Processing Pipelines**
   ```bash
   # Complex pipeline example: Analyze log levels
   cat ~/lab4/sample.log | cut -d' ' -f3 | sort | uniq -c | sort -nr
   # What happens step by step:
   # 1. cat: output file contents
   # 2. cut -d' ' -f3: extract log level field
   # 3. sort: sort log levels alphabetically
   # 4. uniq -c: count unique occurrences
   # 5. sort -nr: sort by count (numeric, reverse)
   # Result: log levels ranked by frequency
   
   # Find users and their activities
   grep "User" ~/lab4/sample.log | awk '{print $5, $6, $7, $8}' | sort
   # What happens:
   # 1. grep "User": find lines containing "User"
   # 2. awk: extract user and action fields
   # 3. sort: organize results alphabetically
   # Result: sorted list of user activities
   
   # Count error types
   grep "ERROR" ~/lab4/sample.log | cut -d' ' -f4- | sort | uniq -c
   # What happens:
   # 1. grep "ERROR": find error lines
   # 2. cut -d' ' -f4-: extract from field 4 to end (error message)
   # 3. sort | uniq -c: count unique error messages
   # Result: frequency count of different errors
   ```

4. **Practical Log Analysis Scenarios**
   ```bash
   # Create more realistic log file
   cat > ~/lab4/access.log << 'EOF'
   192.168.1.100 - - [15/Jan/2024:10:30:15 +0000] "GET /index.html HTTP/1.1" 200 1234
   192.168.1.101 - - [15/Jan/2024:10:31:22 +0000] "POST /login HTTP/1.1" 401 567
   192.168.1.100 - - [15/Jan/2024:10:32:45 +0000] "GET /dashboard HTTP/1.1" 200 2345
   192.168.1.102 - - [15/Jan/2024:10:33:10 +0000] "GET /api/data HTTP/1.1" 500 123
   192.168.1.101 - - [15/Jan/2024:10:34:55 +0000] "POST /login HTTP/1.1" 200 890
   EOF
   
   # Extract IP addresses
   awk '{print $1}' ~/lab4/access.log | sort | uniq -c | sort -nr
   # Result: IP addresses sorted by request frequency
   
   # Find failed requests (4xx, 5xx status codes)
   awk '$9 >= 400 {print $1, $7, $9}' ~/lab4/access.log
   # Result: IP, URL, and status code for failed requests
   
   # Extract request methods
   awk '{print $6}' ~/lab4/access.log | sed 's/"//g' | sort | uniq -c
   # Result: count of GET, POST, etc. requests
   ```

**Verification Commands:**
```bash
# Test your understanding
echo "Test your skills with these commands:"
echo "1. Count total lines in /etc/passwd"
echo "2. Extract usernames from /etc/passwd"
echo "3. Find users with bash shell"
echo "4. Count different shell types"

# Solutions:
wc -l /etc/passwd
cut -d: -f1 /etc/passwd
grep "/bin/bash" /etc/passwd | cut -d: -f1
cut -d: -f7 /etc/passwd | sort | uniq -c
```

2. **Regular Expressions with grep**
   ```bash
   # Basic grep patterns
   grep "ERROR" ~/lab3/sample.log
   grep "^2024" ~/lab3/sample.log
   grep "alice$" ~/lab3/sample.log
   
   # Extended regular expressions
   grep -E "(ERROR|WARNING)" ~/lab3/sample.log
   grep -E "[0-9]{2}:[0-9]{2}:[0-9]{2}" ~/lab3/sample.log
   
   # Case-insensitive search
   grep -i "error" ~/lab3/sample.log
   ```

3. **Advanced Text Processing**
   ```bash
   # Count occurrences
   grep -c "INFO" ~/lab3/sample.log
   
   # Show line numbers
   grep -n "alice" ~/lab3/sample.log
   
   # Multiple file search
   grep -r "error" ~/lab3/ 2>/dev/null
   ```

---

### Chapter 5 Lab: Connecting to Red Hat Enterprise Linux 9
**Reference:** Chapter 5 - Labs 5.1 & 5.2 (Page 119)

**Objective:** Master SSH, file transfers, and remote access

**Tasks:**
1. **SSH Configuration and Usage**
   ```bash
   # Generate SSH key pair
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/rhcsa_lab_key
   
   # Copy public key to remote server (use second VM)
   ssh-copy-id -i ~/.ssh/rhcsa_lab_key.pub user@server2
   
   # Test key-based authentication
   ssh -i ~/.ssh/rhcsa_lab_key user@server2 'hostname; date'
   ```

2. **Secure File Transfer Methods**
   ```bash
   # Using scp
   scp -i ~/.ssh/rhcsa_lab_key ~/lab3/sample.log user@server2:/tmp/
   
   # Using sftp
   sftp -i ~/.ssh/rhcsa_lab_key user@server2
   # In sftp: put ~/lab3/docs_backup.tar.gz /tmp/
   
   # Using rsync
   rsync -avz -e "ssh -i ~/.ssh/rhcsa_lab_key" ~/lab3/ user@server2:/tmp/lab3_sync/
   ```

3. **SSH Tunneling and X11 Forwarding**
   ```bash
   # X11 forwarding (if GUI available)
   ssh -X -i ~/.ssh/rhcsa_lab_key user@server2
   
   # Port forwarding example
   ssh -L 8080:localhost:80 -i ~/.ssh/rhcsa_lab_key user@server2
   ```

---

## Part II: Operating Running Systems

### Chapter 6 Lab: User and Group Management
**Reference:** Chapter 6 - Labs 6.1 & 6.2 (Page 141-142)

**Objective:** Master user/group creation and management

**Tasks:**
1. **Advanced User Creation**
   ```bash
   # Create users with specific requirements
   sudo useradd -m -s /bin/bash -c "Database Administrator" -G wheel dbadmin
   sudo useradd -m -s /bin/bash -c "Web Developer" -d /opt/webdev webdev
   sudo useradd -m -s /bin/bash -c "Temporary User" -e 2024-12-31 tempuser
   
   # Set passwords
   echo "dbadmin:SecurePass123!" | sudo chpasswd
   echo "webdev:WebDev456!" | sudo chpasswd
   echo "tempuser:TempPass789!" | sudo chpasswd
   ```

2. **Password Aging and Account Management**
   ```bash
   # Configure password aging
   sudo chage -M 90 -m 7 -W 14 dbadmin
   sudo chage -E 2024-12-31 tempuser
   
   # View password information
   sudo chage -l dbadmin
   sudo chage -l tempuser
   
   # Lock/unlock accounts
   sudo usermod -L tempuser
   sudo usermod -U tempuser
   ```

3. **Group Management**
   ```bash
   # Create groups
   sudo groupadd -g 2000 developers
   sudo groupadd -g 2001 dbusers
   sudo groupadd -g 2002 webadmins
   
   # Add users to groups
   sudo usermod -aG developers,dbusers dbadmin
   sudo usermod -aG developers,webadmins webdev
   
   # Verify group membership
   groups dbadmin
   id webdev
   ```

---

### Chapter 7 Lab: Permissions Management
**Reference:** Chapter 7 - Lab 7.1 (Page 164)

**Objective:** Master file permissions and advanced attributes

**Tasks:**
1. **Basic Permission Management**
   ```bash
   # Create test directory structure
   sudo mkdir -p /shared/{public,private,group}
   
   # Set ownership and permissions
   sudo chown root:developers /shared/group
   sudo chmod 2775 /shared/group  # Set SGID
   sudo chmod 755 /shared/public
   sudo chmod 700 /shared/private
   ```

2. **Advanced Permissions (SUID, SGID, Sticky Bit)**
   ```bash
   # Create files for testing special permissions
   sudo touch /shared/suid_test
   sudo chmod u+s /shared/suid_test  # SUID
   
   sudo touch /shared/sgid_test
   sudo chmod g+s /shared/sgid_test  # SGID
   
   sudo chmod +t /shared/public  # Sticky bit
   
   # Verify special permissions
   ls -la /shared/
   ```

3. **Access Control Lists (ACLs)**
   ```bash
   # Set ACLs
   sudo setfacl -m u:webdev:rwx /shared/private
   sudo setfacl -m g:developers:rw- /shared/public
   sudo setfacl -d -m g:developers:rw- /shared/group
   
   # View ACLs
   getfacl /shared/private
   getfacl /shared/group
   ```

4. **File Attributes**
   ```bash
   # Create test file
   sudo touch /shared/immutable_file
   echo "This file cannot be modified" | sudo tee /shared/immutable_file
   
   # Set immutable attribute
   sudo chattr +i /shared/immutable_file
   
   # Test immutability
   sudo rm /shared/immutable_file  # Should fail
   
   # Remove immutable attribute
   sudo chattr -i /shared/immutable_file
   ```

---

### Chapter 8 Lab: Configuring Networking
**Reference:** Chapter 8 - Lab 8.1 (Page 193)

**Objective:** Master network configuration with NetworkManager

**Tasks:**
1. **Network Interface Configuration**
   ```bash
   # View current network configuration
   nmcli device status
   nmcli connection show
   ip addr show
   
   # Create static IP configuration
   sudo nmcli connection add type ethernet con-name static-lab ifname eth0
   sudo nmcli connection modify static-lab ipv4.addresses 192.168.1.100/24
   sudo nmcli connection modify static-lab ipv4.gateway 192.168.1.1
   sudo nmcli connection modify static-lab ipv4.dns "8.8.8.8,8.8.4.4"
   sudo nmcli connection modify static-lab ipv4.method manual
   
   # Activate connection
   sudo nmcli connection up static-lab
   ```

2. **Hostname and DNS Configuration**
   ```bash
   # Set hostname
   sudo hostnamectl set-hostname rhcsa-lab.example.com
   
   # Verify hostname
   hostnamectl
   hostname
   
   # Test DNS resolution
   nslookup google.com
   dig redhat.com
   ```

3. **Network Troubleshooting**
   ```bash
   # Test connectivity
   ping -c 4 8.8.8.8
   ping -c 4 google.com
   
   # Check routing
   ip route show
   
   # Check listening services
   ss -tuln
   ```

---

### Chapter 9 Lab: Managing Software
**Reference:** Chapter 9 - Lab 9.1 (Page 228)

**Objective:** Master package management with dnf

**Tasks:**
1. **Repository Management**
   ```bash
   # List repositories
   sudo dnf repolist
   
   # Add EPEL repository
   sudo dnf install -y epel-release
   
   # Search for packages
   dnf search httpd
   dnf info httpd
   ```

2. **Package Installation and Management**
   ```bash
   # Install packages
   sudo dnf install -y httpd mariadb-server
   
   # List installed packages
   dnf list installed | grep httpd
   
   # Update packages
   sudo dnf update
   
   # Remove packages
   sudo dnf remove mariadb-server
   ```

3. **Package Groups and History**
   ```bash
   # List package groups
   dnf group list
   
   # Install package group
   sudo dnf group install "Development Tools"
   
   # View dnf history
   dnf history
   
   # Undo last transaction
   sudo dnf history undo last
   ```

---

### Chapter 10 Lab: Managing Processes
**Reference:** Chapter 10 (Page 231+)

**Objective:** Master process management and job control

**Tasks:**
1. **Process Monitoring and Control**
   ```bash
   # Start background processes
   sleep 300 &
   dd if=/dev/zero of=/tmp/testfile bs=1M count=100 &
   
   # Monitor processes
   jobs
   ps aux | grep sleep
   pgrep dd
   
   # Control processes
   kill -STOP %1  # Suspend first job
   kill -CONT %1  # Resume first job
   killall sleep
   ```

2. **Process Priorities**
   ```bash
   # Start process with different priorities
   nice -n 10 sleep 600 &
   nice -n -5 sleep 600 &
   
   # Change priority of running process
   sudo renice 5 $(pgrep sleep)
   
   # Monitor with top
   top
   ```

3. **System Monitoring**
   ```bash
   # Monitor system resources
   vmstat 2 5
   iostat 2 5
   free -h
   uptime
   ```

---

## Advanced Labs Based on Cert Guide Chapters 11-28

### Chapter 11: Working with systemd
**Objective:** Master systemd service management

**Tasks:**
1. **Service Management**
   ```bash
   # Enable and start services
   sudo systemctl enable httpd
   sudo systemctl start httpd
   sudo systemctl status httpd
   
   # Create custom service
   sudo tee /etc/systemd/system/myapp.service << 'EOF'
   [Unit]
   Description=My Custom Application
   After=network.target
   
   [Service]
   Type=simple
   ExecStart=/usr/bin/python3 -m http.server 8080
   WorkingDirectory=/var/www/html
   Restart=always
   
   [Install]
   WantedBy=multi-user.target
   EOF
   
   sudo systemctl daemon-reload
   sudo systemctl enable myapp.service
   sudo systemctl start myapp.service
   ```

### Chapter 12: Scheduling Tasks
**Objective:** Master cron and systemd timers

**Tasks:**
1. **Cron Jobs**
   ```bash
   # Add user cron job
   crontab -e
   # Add: 0 2 * * * /usr/bin/find /tmp -type f -mtime +7 -delete
   
   # System-wide cron
   echo "30 3 * * 0 root /usr/bin/yum update -y" | sudo tee -a /etc/crontab
   ```

2. **Systemd Timers**
   ```bash
   # Create timer for backup
   sudo tee /etc/systemd/system/backup.service << 'EOF'
   [Unit]
   Description=Daily Backup
   
   [Service]
   Type=oneshot
   ExecStart=/usr/local/bin/backup.sh
   EOF
   
   sudo tee /etc/systemd/system/backup.timer << 'EOF'
   [Unit]
   Description=Run backup daily
   Requires=backup.service
   
   [Timer]
   OnCalendar=daily
   Persistent=true
   
   [Install]
   WantedBy=timers.target
   EOF
   
   sudo systemctl enable backup.timer
   sudo systemctl start backup.timer
   ```

### Chapter 13: Configuring Logging
**Objective:** Master rsyslog and journald configuration

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

2. **Configure log rotation**
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
   ```

3. **Work with journald**
   ```bash
   # Configure persistent journal
   sudo mkdir -p /var/log/journal
   sudo systemctl restart systemd-journald
   
   # Query journal logs
   journalctl --since "2024-01-01" --until "2024-01-31"
   journalctl -u sshd.service -f
   journalctl -p err
   ```

### Chapter 14-15: Storage Management
**Objective:** Master disk partitioning, LVM, and file systems

**Tasks:**
1. **Disk Partitioning**
   ```bash
   # Create partitions with parted
   sudo parted /dev/sdb mklabel gpt
   sudo parted /dev/sdb mkpart primary ext4 1MiB 1GiB
   sudo parted /dev/sdb mkpart primary xfs 1GiB 2GiB
   
   # Create file systems
   sudo mkfs.ext4 /dev/sdb1
   sudo mkfs.xfs /dev/sdb2
   ```

2. **LVM Management**
   ```bash
   # Create LVM structure
   sudo pvcreate /dev/sdb3
   sudo vgcreate vg_data /dev/sdb3
   sudo lvcreate -L 500M -n lv_web vg_data
   sudo lvcreate -L 300M -n lv_db vg_data
   
   # Format and mount
   sudo mkfs.ext4 /dev/vg_data/lv_web
   sudo mkfs.xfs /dev/vg_data/lv_db
   
   sudo mkdir -p /mnt/{web,database}
   sudo mount /dev/vg_data/lv_web /mnt/web
   sudo mount /dev/vg_data/lv_db /mnt/database
   
   # Add to fstab
   echo "/dev/vg_data/lv_web /mnt/web ext4 defaults 0 2" | sudo tee -a /etc/fstab
   echo "/dev/vg_data/lv_db /mnt/database xfs defaults 0 2" | sudo tee -a /etc/fstab
   ```

### Chapter 22: Managing SELinux
**Objective:** Master SELinux policies and troubleshooting

**Tasks:**
1. **SELinux Basics**
   ```bash
   # Check SELinux status
   getenforce
   sestatus
   
   # View contexts
   ls -Z /var/www/html/
   ps -eZ | grep httpd
   ```

2. **SELinux Policy Management**
   ```bash
   # Set boolean values
   sudo setsebool -P httpd_can_network_connect on
   
   # Manage file contexts
   sudo semanage fcontext -a -t httpd_exec_t "/web/content(/.*)?"
   sudo restorecon -Rv /web/content/
   
   # Troubleshoot denials
   sudo ausearch -m AVC -ts recent
   sudo sealert -a /var/log/audit/audit.log
   ```

### Chapter 23: Configuring a Firewall
**Objective:** Master firewalld configuration

**Tasks:**
1. **Firewall Management**
   ```bash
   # Basic firewall operations
   sudo firewall-cmd --state
   sudo firewall-cmd --get-default-zone
   sudo firewall-cmd --list-all
   
   # Add services and ports
   sudo firewall-cmd --permanent --add-service=http
   sudo firewall-cmd --permanent --add-service=https
   sudo firewall-cmd --permanent --add-port=8080/tcp
   
   # Create custom zone
   sudo firewall-cmd --permanent --new-zone=webserver
   sudo firewall-cmd --permanent --zone=webserver --add-service=http
   sudo firewall-cmd --permanent --zone=webserver --add-service=https
   
   sudo firewall-cmd --reload
   ```

### Chapter 16: Basic Kernel Management
**Objective:** Understand kernel modules and parameters

**Tasks:**
1. **Kernel Module Management**
   ```bash
   # List loaded modules
   lsmod | head -10
   
   # Get module information
   modinfo e1000
   
   # Load/unload modules
   sudo modprobe dummy
   sudo modprobe -r dummy
   
   # Configure module parameters
   echo "options dummy numdummies=2" | sudo tee /etc/modprobe.d/dummy.conf
   ```

2. **Kernel Parameters**
   ```bash
   # View kernel parameters
   sysctl -a | grep vm.swappiness
   
   # Set runtime parameters
   sudo sysctl vm.swappiness=10
   
   # Make persistent
   echo "vm.swappiness = 10" | sudo tee -a /etc/sysctl.conf
   ```

### Chapter 17: Managing Boot Procedure
**Objective:** Master GRUB and boot process

**Tasks:**
1. **GRUB Configuration**
   ```bash
   # Edit GRUB defaults
   sudo vim /etc/default/grub
   # Modify: GRUB_CMDLINE_LINUX="rhgb quiet"
   
   # Regenerate GRUB config
   sudo grub2-mkconfig -o /boot/grub2/grub.cfg
   
   # Set default boot entry
   sudo grub2-set-default 0
   ```

2. **Boot Targets**
   ```bash
   # Change default target
   sudo systemctl set-default multi-user.target
   
   # Boot to rescue mode
   sudo systemctl rescue
   
   # Emergency mode
   sudo systemctl emergency
   ```

### Chapter 18: Essential Troubleshooting Skills
**Objective:** Master system troubleshooting techniques

**Tasks:**
1. **System Analysis**
   ```bash
   # Check system status
   systemctl status
   systemctl list-failed
   
   # Analyze boot issues
   journalctl -b
   journalctl -p err
   
   # Check disk space
   df -h
   du -sh /var/log/*
   ```

2. **Network Troubleshooting**
   ```bash
   # Test connectivity
   ping -c 4 8.8.8.8
   traceroute google.com
   
   # Check ports
   ss -tuln
   nmap localhost
   
   # DNS resolution
   nslookup google.com
   dig google.com
   ```

### Chapter 19: Bash Shell Scripting
**Objective:** Basic automation with bash scripts

**Tasks:**
1. **Script Creation**
   ```bash
   # Create backup script
   cat > /usr/local/bin/backup.sh << 'EOF'
   #!/bin/bash
   DATE=$(date +%Y%m%d)
   BACKUP_DIR="/backup"
   SOURCE_DIR="/home"
   
   mkdir -p $BACKUP_DIR
   tar -czf $BACKUP_DIR/home_backup_$DATE.tar.gz $SOURCE_DIR
   
   # Keep only last 7 backups
   find $BACKUP_DIR -name "home_backup_*.tar.gz" -mtime +7 -delete
   EOF
   
   chmod +x /usr/local/bin/backup.sh
   ```

2. **Script with Variables and Conditionals**
   ```bash
   # System monitoring script
   cat > /usr/local/bin/monitor.sh << 'EOF'
   #!/bin/bash
   
   DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
   MEMORY_USAGE=$(free | awk 'NR==2{printf "%.0f", $3*100/$2}')
   
   if [ $DISK_USAGE -gt 80 ]; then
       echo "WARNING: Disk usage is ${DISK_USAGE}%"
   fi
   
   if [ $MEMORY_USAGE -gt 80 ]; then
       echo "WARNING: Memory usage is ${MEMORY_USAGE}%"
   fi
   EOF
   
   chmod +x /usr/local/bin/monitor.sh
   ```

### Chapter 20: Advanced SSH Configuration
**Objective:** Master SSH security and advanced features

**Tasks:**
1. **SSH Security Hardening**
   ```bash
   # Configure SSH daemon
   sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
   
   # Edit SSH configuration
   sudo tee -a /etc/ssh/sshd_config << 'EOF'
   PasswordAuthentication no
   PermitRootLogin no
   MaxAuthTries 3
   ClientAliveInterval 300
   ClientAliveCountMax 2
   AllowUsers alice bob
   EOF
   
   sudo systemctl restart sshd
   ```

2. **SSH Tunneling**
   ```bash
   # Local port forwarding
   ssh -L 8080:localhost:80 user@remote-server
   
   # Remote port forwarding
   ssh -R 9090:localhost:22 user@remote-server
   
   # Dynamic port forwarding (SOCKS proxy)
   ssh -D 1080 user@remote-server
   ```

### Chapter 21: Managing Apache HTTP Services
**Objective:** Configure and manage Apache web server

**Tasks:**
1. **Apache Installation and Configuration**
   ```bash
   # Install Apache
   sudo dnf install -y httpd
   
   # Configure virtual host
   sudo tee /etc/httpd/conf.d/example.conf << 'EOF'
   <VirtualHost *:80>
       ServerName example.com
       DocumentRoot /var/www/example
       ErrorLog /var/log/httpd/example_error.log
       CustomLog /var/log/httpd/example_access.log combined
   </VirtualHost>
   EOF
   
   # Create document root
   sudo mkdir -p /var/www/example
   echo "<h1>Welcome to Example.com</h1>" | sudo tee /var/www/example/index.html
   
   # Set SELinux context
   sudo setsebool -P httpd_enable_homedirs on
   sudo restorecon -Rv /var/www/example
   ```

2. **SSL Configuration**
   ```bash
   # Install SSL module
   sudo dnf install -y mod_ssl
   
   # Generate self-signed certificate
   sudo openssl req -new -x509 -days 365 -nodes \
     -out /etc/pki/tls/certs/example.crt \
     -keyout /etc/pki/tls/private/example.key
   
   # Configure SSL virtual host
   sudo tee /etc/httpd/conf.d/example-ssl.conf << 'EOF'
   <VirtualHost *:443>
       ServerName example.com
       DocumentRoot /var/www/example
       SSLEngine on
       SSLCertificateFile /etc/pki/tls/certs/example.crt
       SSLCertificateKeyFile /etc/pki/tls/private/example.key
   </VirtualHost>
   EOF
   ```

### Chapter 24: Accessing Network Storage
**Objective:** Configure NFS and autofs

**Tasks:**
1. **NFS Client Configuration**
   ```bash
   # Install NFS utilities
   sudo dnf install -y nfs-utils
   
   # Mount NFS share
   sudo mkdir -p /mnt/nfs-share
   sudo mount -t nfs server2:/shared /mnt/nfs-share
   
   # Add to fstab for persistent mount
   echo "server2:/shared /mnt/nfs-share nfs defaults 0 0" | sudo tee -a /etc/fstab
   ```

2. **Autofs Configuration**
   ```bash
   # Install autofs
   sudo dnf install -y autofs
   
   # Configure auto.master
   echo "/mnt/auto /etc/auto.misc --timeout=60" | sudo tee -a /etc/auto.master
   
   # Configure auto.misc
   echo "nfs-data -rw,soft server2:/data" | sudo tee -a /etc/auto.misc
   
   # Start autofs
   sudo systemctl enable --now autofs
   ```

### Chapter 25: Configuring Time Services
**Objective:** Configure NTP and time synchronization

**Tasks:**
1. **Chrony Configuration**
   ```bash
   # Install chrony
   sudo dnf install -y chrony
   
   # Configure time servers
   sudo tee /etc/chrony.conf << 'EOF'
   server 0.pool.ntp.org iburst
   server 1.pool.ntp.org iburst
   server 2.pool.ntp.org iburst
   
   driftfile /var/lib/chrony/drift
   makestep 1.0 3
   rtcsync
   EOF
   
   # Start and enable chrony
   sudo systemctl enable --now chronyd
   ```

2. **Time Zone Configuration**
   ```bash
   # List available time zones
   timedatectl list-timezones | grep America
   
   # Set time zone
   sudo timedatectl set-timezone America/New_York
   
   # Check time status
   timedatectl status
   chrony sources -v
   ```

### Chapter 26: Managing Containers
**Objective:** Master container management with Podman

**Tasks:**
1. **Container Operations**
   ```bash
   # Pull and run containers
   podman pull registry.redhat.io/ubi8/ubi:latest
   podman run -d --name web-server -p 8080:80 nginx:latest
   
   # Manage containers
   podman ps
   podman stop web-server
   podman start web-server
   
   # Create systemd service for container
   podman generate systemd --new --name web-server > ~/.config/systemd/user/web-server.service
   systemctl --user enable web-server.service
   ```

---

## Final Exam Preparation (Chapter 27-28)

### Practice Exam Scenarios
**Reference:** Chapters 28 and Practice Exams A-D

**Scenario 1: Complete System Setup**
1. Configure static IP: 192.168.1.50/24
2. Set hostname: exam.example.com
3. Create users: alice (wheel group), bob (developers group)
4. Configure SSH key authentication
5. Install and configure Apache
6. Create LVM volume for web content
7. Configure firewall for HTTP/HTTPS
8. Set up SELinux for web server

**Scenario 2: System Recovery**
1. Reset root password using rescue mode
2. Fix broken fstab entry
3. Restore SELinux contexts
4. Fix network configuration
5. Repair systemd service

**Scenario 3: Storage and User Management**
1. Create partition and LVM structure
2. Configure file systems and mount points
3. Set up user accounts with password policies
4. Configure ACLs and special permissions
5. Set up automated backup with cron

---

## Study Tips from the Cert Guide

### Time Management (Chapter 27)
- **Read all questions first** (5 minutes)
- **Plan your approach** for each task
- **Verify configurations** are persistent
- **Test your work** before moving on

### Common Exam Topics
1. **User and Group Management** (15-20% of exam)
2. **File Permissions and ACLs** (10-15% of exam)
3. **Storage Management** (15-20% of exam)
4. **Network Configuration** (10-15% of exam)
5. **Service Management** (15-20% of exam)
6. **SELinux** (10-15% of exam)
7. **Troubleshooting** (Throughout exam)

### Essential Commands to Master
```bash
# File Operations
ls, cp, mv, rm, find, locate, which, chmod, chown, chgrp

# Text Processing
grep, sed, awk, sort, uniq, cut, head, tail, wc

# User Management
useradd, usermod, userdel, passwd, chage, su, sudo

# Process Management
ps, top, kill, jobs, nohup, systemctl, journalctl

# Network
ip, nmcli, firewall-cmd, ss, ping, curl

# Storage
fdisk, parted, mkfs, mount, umount, lvm commands

# Archives
tar, gzip, gunzip

# SELinux
getenforce, setsebool, semanage, restorecon, ausearch
```

### Final Checklist
- [ ] All configurations are persistent (survive reboot)
- [ ] Services start automatically
- [ ] Network connectivity works
- [ ] SELinux contexts are correct
- [ ] File permissions are properly set
- [ ] Users can log in and access resources
- [ ] Firewall rules allow required traffic

**Success Strategy:** Practice these labs multiple times until you can complete them without referring to notes. The RHCSA exam is entirely hands-on, so muscle memory is crucial!

---

*Based on the official Red Hat RHCSA 9 Cert Guide: EX200 by Sander van Vugt*
