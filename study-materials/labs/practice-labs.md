# RHCSA Practice Labs - Hands-on Exercises

## 📚 Study Resources
- **[Red Hat Certification Study Guide](./preview-9780138096151_A46998912.pdf)** - Comprehensive RHCSA exam preparation material

---

## Lab Environment Setup

### Required VMs
- **VM1 (server1):** RHEL 9 - Primary practice system
- **VM2 (server2):** RHEL 9 - Secondary for networking/NFS
- **Resources:** 4GB RAM, 40GB disk each
- **Network:** Both VMs on same network segment

---

## Week 1 Labs: Linux Fundamentals

### Lab 1.1: File System Navigation and Operations
**Objective:** Master basic file operations and permissions

**Tasks:**
1. Create directory structure: `/home/student/projects/{web,db,backup}`
2. Create files with different permissions in each directory
3. Practice file operations (copy, move, delete)
4. Use find to locate files by various criteria
5. Create symbolic and hard links

**Commands to Practice:**
```bash
mkdir -p /home/student/projects/{web,db,backup}
touch /home/student/projects/web/{index.html,style.css}
chmod 755 /home/student/projects/web/
find /home/student -name "*.html" -type f
ln -s /home/student/projects/web/index.html /tmp/index_link
```

### Lab 1.2: User and Group Management
**Objective:** Create and manage users with specific requirements

**Tasks:**
1. Create users: webadmin, dbadmin, backup_user
2. Create groups: webteam, dbteam, backupteam
3. Set password policies and expiration
4. Configure sudo access for specific users
5. Test user switching and permissions

**Verification:**
- Users can log in with assigned passwords
- Group memberships are correct
- Sudo access works as configured

### Lab 1.3: File Permissions and ACLs
**Objective:** Implement complex permission scenarios

**Tasks:**
1. Create shared directories for team collaboration
2. Set up ACLs for fine-grained permissions
3. Configure sticky bit on shared directories
4. Test permission inheritance
5. Troubleshoot permission issues

**Example Scenario:**
```bash
mkdir /shared/webteam
setfacl -m g:webteam:rwx /shared/webteam
setfacl -d -m g:webteam:rwx /shared/webteam
chmod +t /shared/webteam
```

---

## Week 2 Labs: System Administration

### Lab 2.1: Package Management
**Objective:** Master DNF/YUM package operations

**Tasks:**
1. Configure additional repositories
2. Install package groups (Development Tools)
3. Update system packages
4. Install local RPM packages
5. Manage package dependencies

**Practice Commands:**
```bash
dnf config-manager --add-repo http://example.com/repo
dnf group install "Development Tools"
dnf localinstall package.rpm
dnf history undo last
```

### Lab 2.2: Service Management with systemd
**Objective:** Configure and manage system services

**Tasks:**
1. Install and configure Apache web server
2. Create custom systemd service
3. Configure service dependencies
4. Set up service to start at boot
5. Monitor service logs with journalctl

**Custom Service Example:**
```ini
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
User=myapp
ExecStart=/opt/myapp/start.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

### Lab 2.3: Network Configuration
**Objective:** Configure network interfaces and services

**Tasks:**
1. Configure static IP address using nmcli
2. Set up hostname resolution
3. Configure SSH for key-based authentication
4. Test network connectivity and troubleshooting
5. Configure network bonding (if multiple interfaces available)

**Network Configuration:**
```bash
nmcli connection add type ethernet con-name static-eth0 ifname eth0
nmcli connection modify static-eth0 ipv4.addresses 192.168.1.100/24
nmcli connection modify static-eth0 ipv4.gateway 192.168.1.1
nmcli connection modify static-eth0 ipv4.dns 8.8.8.8
nmcli connection modify static-eth0 ipv4.method manual
nmcli connection up static-eth0
```

---

## Week 3 Labs: Advanced Administration

### Lab 3.1: Firewall Configuration
**Objective:** Secure system with firewalld

**Tasks:**
1. Configure firewall zones
2. Allow specific services and ports
3. Create rich rules for complex scenarios
4. Test firewall rules
5. Make configurations permanent

**Firewall Scenarios:**
```bash
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" accept' --permanent
firewall-cmd --reload
```

### Lab 3.2: SELinux Configuration
**Objective:** Work with SELinux contexts and policies

**Tasks:**
1. Set SELinux to enforcing mode
2. Configure file contexts for web server
3. Manage SELinux booleans
4. Troubleshoot SELinux denials
5. Create custom SELinux policies

**SELinux Practice:**
```bash
semanage fcontext -a -t httpd_exec_t "/custom/web(/.*)?"
restorecon -R /custom/web
setsebool -P httpd_can_network_connect on
ausearch -m avc -ts recent | audit2allow -M mypolicy
```

### Lab 3.3: System Monitoring and Logging
**Objective:** Monitor system performance and manage logs

**Tasks:**
1. Configure persistent journal logging
2. Set up log rotation
3. Monitor system performance
4. Create custom log entries
5. Analyze system logs for issues

**Logging Configuration:**
```bash
mkdir -p /var/log/journal
systemctl restart systemd-journald
journalctl --disk-usage
journalctl -u httpd.service --since "1 hour ago"
```

---

## Week 4 Labs: Storage Management

### Lab 4.1: Disk Partitioning and File Systems
**Objective:** Create and manage disk partitions

**Tasks:**
1. Add new disk to VM
2. Create GPT partition table
3. Create multiple partitions with different file systems
4. Configure automatic mounting in fstab
5. Test mount options and permissions

**Partitioning Practice:**
```bash
parted /dev/sdb mklabel gpt
parted /dev/sdb mkpart primary ext4 1MiB 1GiB
parted /dev/sdb mkpart primary xfs 1GiB 2GiB
mkfs.ext4 /dev/sdb1
mkfs.xfs /dev/sdb2
```

### Lab 4.2: LVM Management
**Objective:** Implement Logical Volume Management

**Tasks:**
1. Create physical volumes from multiple disks
2. Create volume group spanning multiple PVs
3. Create logical volumes with different sizes
4. Extend logical volumes and file systems
5. Create and manage LVM snapshots

**LVM Lab Scenario:**
```bash
pvcreate /dev/sdb /dev/sdc
vgcreate vg_data /dev/sdb /dev/sdc
lvcreate -L 2G -n lv_web vg_data
lvcreate -L 1G -n lv_db vg_data
mkfs.ext4 /dev/vg_data/lv_web
lvextend -L +1G /dev/vg_data/lv_web
resize2fs /dev/vg_data/lv_web
```

### Lab 4.3: SWAP and Advanced Storage
**Objective:** Configure SWAP and advanced storage features

**Tasks:**
1. Create SWAP partition and SWAP file
2. Configure encrypted storage with LUKS
3. Set up software RAID (if multiple disks available)
4. Configure Stratis storage pools
5. Test storage performance and monitoring

**SWAP Configuration:**
```bash
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo '/swapfile none swap sw 0 0' >> /etc/fstab
```

---

## Week 5 Labs: Modern Technologies

### Lab 5.1: Container Management with Podman
**Objective:** Deploy and manage containers

**Tasks:**
1. Pull and run various container images
2. Create persistent volumes for containers
3. Configure port mapping and networking
4. Create containers as systemd services
5. Manage rootless containers

**Container Lab:**
```bash
podman pull httpd
podman run -d --name web-server -p 8080:80 -v /opt/web:/usr/local/apache2/htdocs:Z httpd
podman generate systemd --name web-server > /etc/systemd/system/web-container.service
systemctl enable --now web-container.service
```

### Lab 5.2: Advanced Scheduling
**Objective:** Implement task scheduling with cron and systemd timers

**Tasks:**
1. Create various cron jobs for different users
2. Set up systemd timers for system maintenance
3. Configure at jobs for one-time tasks
4. Monitor and troubleshoot scheduled tasks
5. Create backup automation scripts

**Timer Example:**
```ini
# backup.timer
[Unit]
Description=Daily backup timer

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

---

## Week 6 Labs: Integration Scenarios

### Lab 6.1: Web Server Deployment
**Objective:** Deploy complete web server infrastructure

**Tasks:**
1. Install and configure Apache/Nginx
2. Set up SSL/TLS certificates
3. Configure virtual hosts
4. Implement security hardening
5. Set up log monitoring and rotation

**Integration Points:**
- Package management (httpd installation)
- Service management (systemd)
- Networking (firewall configuration)
- Security (SELinux contexts)
- Storage (web content directories)

### Lab 6.2: Database Server Setup
**Objective:** Deploy and secure database server

**Tasks:**
1. Install MariaDB/PostgreSQL
2. Configure database security
3. Create databases and users
4. Set up automated backups
5. Monitor database performance

**Security Configuration:**
```bash
mysql_secure_installation
firewall-cmd --add-service=mysql --permanent
setsebool -P mysql_connect_any on
```

### Lab 6.3: File Server with NFS
**Objective:** Set up network file sharing

**Tasks:**
1. Configure NFS server
2. Create and export file shares
3. Set up client-side mounting
4. Configure autofs for automatic mounting
5. Implement security and access controls

**NFS Configuration:**
```bash
# Server side
echo '/shared/data 192.168.1.0/24(rw,sync,no_root_squash)' >> /etc/exports
systemctl enable --now nfs-server
exportfs -ra

# Client side
mount -t nfs server1:/shared/data /mnt/nfs
```

---

## Week 7 Labs: Mock Exams

### Mock Exam 1: Comprehensive Scenario
**Time Limit:** 3 hours
**Scenario:** Set up a multi-service environment

**Tasks:**
1. Configure network with static IP
2. Create users and groups with specific requirements
3. Set up LVM storage with multiple logical volumes
4. Install and configure web server with SSL
5. Configure firewall and SELinux
6. Set up automated backups with cron
7. Deploy container services
8. Configure system monitoring

### Mock Exam 2: Troubleshooting Focus
**Time Limit:** 3 hours
**Scenario:** Fix broken system configurations

**Tasks:**
1. Fix boot issues and recover root password
2. Resolve SELinux denials
3. Fix network connectivity problems
4. Repair file system errors
5. Resolve service startup failures
6. Fix permission and ownership issues
7. Troubleshoot storage problems
8. Resolve container deployment issues

### Mock Exam 3: Performance and Security
**Time Limit:** 3 hours
**Scenario:** Optimize and secure existing system

**Tasks:**
1. Implement security hardening measures
2. Optimize system performance
3. Configure advanced networking features
4. Set up comprehensive monitoring
5. Implement backup and recovery procedures
6. Configure high-availability services
7. Deploy containerized applications
8. Create automation scripts

---

## Lab Verification Checklist

### After Each Lab
- [ ] All tasks completed successfully
- [ ] Configurations survive system reboot
- [ ] Services start automatically
- [ ] Security settings are properly configured
- [ ] Documentation is updated
- [ ] Commands and procedures are noted

### Weekly Review
- [ ] All lab objectives met
- [ ] Weak areas identified and addressed
- [ ] Command proficiency improved
- [ ] Real-world scenarios practiced
- [ ] Integration concepts understood

**Remember: The key to RHCSA success is hands-on practice. Complete every lab exercise and verify your work thoroughly!**
