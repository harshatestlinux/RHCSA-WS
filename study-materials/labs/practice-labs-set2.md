# RHCSA Practice Labs - Set 2 (Additional Practice)

## Lab Environment Setup

### Required VMs
- **VM1 (rhcsa1):** RHEL 9 - Primary practice system
- **VM2 (rhcsa2):** RHEL 9 - Secondary for advanced scenarios
- **Resources:** 4GB RAM, 40GB disk each + additional 20GB disk for storage labs
- **Network:** Both VMs on same network segment

---

## Week 1 Labs: Advanced Linux Fundamentals

### Lab 2.1: Advanced File Operations and Text Processing
**Objective:** Master advanced file operations, text processing, and shell scripting basics

**Tasks:**
1. Create a directory structure for a web development project
2. Use advanced find commands with multiple criteria
3. Process log files using grep, sed, and awk
4. Create and manage file archives with different compression methods
5. Write a simple shell script to automate file organization

**Detailed Requirements:**
- Create `/opt/webdev/{src,logs,backups,config}` structure
- Find all files modified in the last 7 days that are larger than 1MB
- Extract specific information from Apache log files
- Create compressed archives using tar, gzip, and bzip2
- Write a script that organizes files by extension

### Lab 2.2: Advanced User and Group Management
**Objective:** Implement complex user management scenarios

**Tasks:**
1. Create a multi-tier user structure for a company
2. Implement password policies and account restrictions
3. Set up user quotas on specific directories
4. Configure advanced sudo rules with command restrictions
5. Create shared directories with complex ACL configurations

**Detailed Requirements:**
- Create departments: IT (3 users), HR (2 users), Finance (2 users)
- Set different password aging policies per department
- Configure disk quotas for user home directories
- Create sudo rules allowing specific commands only
- Set up project directories with department-specific access

### Lab 2.3: Advanced File Permissions and Security
**Objective:** Implement complex permission scenarios and security measures

**Tasks:**
1. Set up a secure file sharing environment
2. Implement advanced ACLs with inheritance
3. Configure special permissions (SUID, SGID, Sticky bit)
4. Create a secure backup directory structure
5. Implement file integrity monitoring

**Detailed Requirements:**
- Create shared directories for different security levels
- Use ACLs to grant specific users read-only access to sensitive files
- Set up directories where only file owners can delete their files
- Create a backup system with proper permissions
- Monitor critical system files for unauthorized changes

---

## Week 2 Labs: Advanced System Administration

### Lab 2.4: Advanced Package Management and Repositories
**Objective:** Master complex package management scenarios

**Tasks:**
1. Set up a local YUM/DNF repository
2. Create custom RPM packages
3. Manage package dependencies and conflicts
4. Configure package priorities and exclusions
5. Implement automated package updates with restrictions

**Detailed Requirements:**
- Create a local repository from downloaded RPMs
- Build a simple RPM package from source
- Resolve dependency conflicts manually
- Exclude specific packages from updates
- Set up automatic security updates only

### Lab 2.5: Advanced Service Management and Systemd
**Objective:** Create complex service configurations and dependencies

**Tasks:**
1. Create a multi-service application stack
2. Implement service dependencies and ordering
3. Configure service resource limits
4. Set up service monitoring and automatic restart
5. Create systemd timers for maintenance tasks

**Detailed Requirements:**
- Create a web application with database dependency
- Configure services to start in specific order
- Limit CPU and memory usage for services
- Set up automatic service restart on failure
- Create timers for log rotation and cleanup tasks

### Lab 2.6: Advanced Network Configuration and Troubleshooting
**Objective:** Implement complex networking scenarios

**Tasks:**
1. Configure network bonding/teaming
2. Set up VLAN configuration
3. Implement advanced routing scenarios
4. Configure network bridges
5. Troubleshoot complex network issues

**Detailed Requirements:**
- Create a bonded interface with failover
- Configure VLAN tagging on interfaces
- Set up static routes for specific networks
- Create a bridge for virtual machines
- Diagnose and fix network connectivity problems

---

## Week 3 Labs: Advanced Security and Storage

### Lab 2.7: Advanced Firewall and Network Security
**Objective:** Implement comprehensive firewall configurations

**Tasks:**
1. Create custom firewall zones
2. Implement port forwarding and NAT
3. Set up firewall logging and monitoring
4. Configure fail2ban for intrusion prevention
5. Create complex rich rules for specific scenarios

**Detailed Requirements:**
- Create DMZ zone for web servers
- Forward external ports to internal services
- Log all firewall denials for analysis
- Automatically ban IPs after failed login attempts
- Create rules based on time, source, and service combinations

### Lab 2.8: Advanced SELinux Configuration
**Objective:** Master complex SELinux scenarios

**Tasks:**
1. Create custom SELinux policies
2. Configure SELinux for custom applications
3. Troubleshoot complex SELinux denials
4. Implement SELinux user mappings
5. Set up SELinux monitoring and alerting

**Detailed Requirements:**
- Write custom SELinux policy for a new service
- Configure contexts for applications in non-standard locations
- Use audit2allow to create policy modules
- Map Linux users to SELinux users
- Set up automated SELinux violation reporting

### Lab 2.9: Advanced Storage Management
**Objective:** Implement complex storage scenarios

**Tasks:**
1. Set up software RAID with hot spares
2. Configure LVM with snapshots and thin provisioning
3. Implement disk encryption with LUKS
4. Set up Stratis storage pools
5. Configure automated storage monitoring

**Detailed Requirements:**
- Create RAID 5 array with spare disk
- Use LVM thin pools for efficient storage
- Encrypt sensitive data partitions
- Create Stratis pools with multiple filesystems
- Monitor disk health and space usage

---

## Week 4 Labs: Integration and Real-World Scenarios

### Lab 2.10: Complete Web Server Deployment
**Objective:** Deploy a complete, secure web server environment

**Tasks:**
1. Install and configure Apache with SSL
2. Set up virtual hosts for multiple domains
3. Configure PHP and database connectivity
4. Implement security hardening
5. Set up monitoring and log analysis

**Detailed Requirements:**
- Configure SSL certificates for HTTPS
- Host multiple websites on same server
- Connect web applications to MariaDB database
- Harden Apache configuration for security
- Monitor web server performance and errors

### Lab 2.11: Complete Database Server Setup
**Objective:** Deploy and secure a database server

**Tasks:**
1. Install and configure MariaDB
2. Set up database replication
3. Implement database security measures
4. Configure automated backups
5. Set up performance monitoring

**Detailed Requirements:**
- Configure master-slave replication
- Create database users with limited privileges
- Set up encrypted connections
- Automate daily database backups
- Monitor database performance metrics

### Lab 2.12: Complete File Server with NFS and Samba
**Objective:** Deploy a comprehensive file sharing solution

**Tasks:**
1. Configure NFS server with multiple exports
2. Set up Samba for Windows compatibility
3. Implement autofs for automatic mounting
4. Configure file sharing security
5. Set up file server monitoring

**Detailed Requirements:**
- Export different directories with various permissions
- Share directories accessible from Windows clients
- Configure automatic mounting of network shares
- Implement Kerberos authentication
- Monitor file server usage and performance

---

## Week 5 Labs: Modern Technologies and Automation

### Lab 2.13: Advanced Container Management
**Objective:** Deploy complex containerized applications

**Tasks:**
1. Create multi-container applications
2. Set up container networking
3. Implement persistent storage for containers
4. Configure container security
5. Set up container monitoring

**Detailed Requirements:**
- Deploy web application with separate database container
- Create custom container networks
- Use volumes for database persistence
- Run containers with security constraints
- Monitor container resource usage

### Lab 2.14: Advanced Task Scheduling and Automation
**Objective:** Implement comprehensive automation solutions

**Tasks:**
1. Create complex cron job schedules
2. Set up systemd timers with dependencies
3. Write advanced shell scripts for automation
4. Implement log rotation and cleanup automation
5. Set up system maintenance automation

**Detailed Requirements:**
- Schedule tasks with complex timing requirements
- Create timers that depend on service states
- Write scripts with error handling and logging
- Automate log management across multiple services
- Create automated system health checks

### Lab 2.15: System Performance Tuning and Monitoring
**Objective:** Optimize system performance and implement monitoring

**Tasks:**
1. Analyze system performance bottlenecks
2. Tune kernel parameters for optimization
3. Configure system resource limits
4. Set up comprehensive monitoring
5. Implement alerting for critical issues

**Detailed Requirements:**
- Identify CPU, memory, and I/O bottlenecks
- Modify sysctl parameters for performance
- Configure cgroups for resource management
- Set up monitoring with graphs and metrics
- Create alerts for system threshold violations

---

## Mock Exam Scenarios

### Mock Exam Set 2: Advanced Integration
**Time Limit:** 3 hours
**Scenario:** Deploy a complete e-commerce infrastructure

**Tasks:**
1. Configure a web server cluster with load balancing
2. Set up a database cluster with replication
3. Implement shared storage with NFS
4. Configure comprehensive security (firewall, SELinux, SSL)
5. Set up monitoring and alerting
6. Implement automated backup solutions
7. Configure container-based microservices
8. Set up log aggregation and analysis

### Mock Exam Set 2: Disaster Recovery
**Time Limit:** 3 hours
**Scenario:** Implement disaster recovery and high availability

**Tasks:**
1. Configure system backup and restore procedures
2. Set up database replication and failover
3. Implement network redundancy
4. Configure automated monitoring and alerting
5. Set up log shipping and analysis
6. Implement security incident response
7. Configure service failover mechanisms
8. Test and document recovery procedures

### Mock Exam Set 2: Security Hardening
**Time Limit:** 3 hours
**Scenario:** Implement comprehensive security hardening

**Tasks:**
1. Harden system configuration and services
2. Implement advanced firewall rules
3. Configure SELinux for custom applications
4. Set up intrusion detection and prevention
5. Implement file integrity monitoring
6. Configure secure remote access
7. Set up security logging and analysis
8. Create security incident response procedures

---

## Lab Verification Checklist

### Advanced Verification Requirements
- [ ] All configurations survive system reboot
- [ ] Services start automatically in correct order
- [ ] Security settings are properly configured
- [ ] Performance meets specified requirements
- [ ] Monitoring and alerting functions correctly
- [ ] Backup and recovery procedures work
- [ ] Documentation is complete and accurate
- [ ] Integration between components functions properly

### Performance Benchmarks
- [ ] Web server responds within 2 seconds
- [ ] Database queries complete within acceptable time
- [ ] File transfers achieve expected throughput
- [ ] System resources are within normal ranges
- [ ] Network latency is minimized
- [ ] Storage I/O performs adequately
- [ ] Container startup time is optimized
- [ ] Backup completion time is reasonable

**Remember: These advanced labs build upon the fundamentals. Master the basics first, then progress to these complex scenarios for comprehensive RHCSA preparation!**
