# 45-Day RHCSA Study Plan - 100% Success Strategy

**Total Study Time:** 90 hours (2 hours/day × 45 days)  
**Goal:** Pass RHCSA with 100% confidence  
**Study Method:** Theory + Hands-on Practice + Daily Reviews

## 📅 Study Schedule Overview

### Week 1-2: Foundation (Days 1-14)
- **Focus:** Linux basics, file systems, users/groups
- **Goal:** Master fundamental commands and concepts

### Week 3-4: System Administration (Days 15-28)
- **Focus:** Services, networking, security, package management
- **Goal:** Understand system administration tasks

### Week 5-6: Advanced Topics (Days 29-42)
- **Focus:** Storage, boot process, containers, scheduling
- **Goal:** Master complex system configurations

### Week 7: Final Review & Practice Exams (Days 43-45)
- **Focus:** Mock exams, weak area reinforcement
- **Goal:** Exam readiness and confidence building

---

## 📚 Daily Study Structure (2 hours)

### Hour 1: Theory & Commands (60 minutes)
- **20 min:** Read topic material
- **20 min:** Practice commands in terminal
- **20 min:** Take notes and create flashcards

### Hour 2: Hands-on Labs (60 minutes)
- **45 min:** Complete practical exercises
- **15 min:** Review and document solutions

---

## 🎯 Detailed Daily Plan

### **WEEK 1: Linux Fundamentals (Days 1-7)**

#### Day 1: Linux Basics & File Operations
- **Theory:** linux-basics.md
- **Practice:** File navigation, permissions, text processing
- **Lab:** Create directory structure, practice file operations
- **Commands to Master:** ls, cd, cp, mv, rm, chmod, chown, find, grep

#### Day 2: File Systems & Mounting
- **Theory:** file-systems.md (basic concepts)
- **Practice:** Mount/unmount, df, du, filesystem types
- **Lab:** Mount different filesystem types, practice fstab
- **Commands to Master:** mount, umount, df, du, lsblk, findmnt

#### Day 3: Users & Groups Management
- **Theory:** users-groups.md
- **Practice:** User creation, modification, group management
- **Lab:** Create users with different settings, manage groups
- **Commands to Master:** useradd, usermod, userdel, groupadd, passwd, id

#### Day 4: File Permissions & ACLs
- **Theory:** security.md (permissions section)
- **Practice:** Standard permissions, ACLs, special permissions
- **Lab:** Set up complex permission scenarios
- **Commands to Master:** chmod, chown, getfacl, setfacl, umask

#### Day 5: Text Processing & Archives
- **Theory:** Advanced text processing
- **Practice:** grep, sed, awk, tar, compression
- **Lab:** Process log files, create/extract archives
- **Commands to Master:** grep, sed, awk, tar, gzip, zip

#### Day 6: Process Management
- **Theory:** Process concepts
- **Practice:** Process monitoring, signals, job control
- **Lab:** Manage processes, use signals, background jobs
- **Commands to Master:** ps, top, htop, kill, jobs, bg, fg, nohup

#### Day 7: Review & Practice Exam 1
- **Review:** All Week 1 topics
- **Practice:** Complete practice scenarios
- **Assessment:** Self-test on fundamental concepts

### **WEEK 2: System Configuration (Days 8-14)**

#### Day 8: Package Management (DNF/YUM)
- **Theory:** package-management.md
- **Practice:** Install, update, remove packages and groups
- **Lab:** Configure repositories, manage package groups
- **Commands to Master:** dnf install/remove/update, dnf group, rpm

#### Day 9: System Services (systemd)
- **Theory:** services.md
- **Practice:** Service management, targets, systemctl
- **Lab:** Create custom service, manage service dependencies
- **Commands to Master:** systemctl, journalctl, systemd targets

#### Day 10: Networking Basics
- **Theory:** networking.md (basic configuration)
- **Practice:** IP configuration, hostname, basic troubleshooting
- **Lab:** Configure network interfaces, test connectivity
- **Commands to Master:** ip, nmcli, ping, netstat, ss

#### Day 11: SSH & Remote Access
- **Theory:** SSH configuration and security
- **Practice:** SSH keys, configuration, security hardening
- **Lab:** Set up passwordless SSH, secure SSH daemon
- **Commands to Master:** ssh, ssh-keygen, ssh-copy-id, scp

#### Day 12: Log Management
- **Theory:** System logging, journal
- **Practice:** Log analysis, journal queries, log rotation
- **Lab:** Analyze system logs, configure logging
- **Commands to Master:** journalctl, logger, rsyslog configuration

#### Day 13: Cron & Task Scheduling
- **Theory:** scheduling.md
- **Practice:** Cron jobs, at jobs, systemd timers
- **Lab:** Schedule various tasks, create systemd timers
- **Commands to Master:** crontab, at, systemctl (timers)

#### Day 14: Review & Practice Exam 2
- **Review:** All Week 2 topics
- **Practice:** System administration scenarios
- **Assessment:** Self-test on system configuration

### **WEEK 3: Advanced System Administration (Days 15-21)**

#### Day 15: Advanced Networking
- **Theory:** networking.md (advanced topics)
- **Practice:** NetworkManager, firewall, advanced routing
- **Lab:** Configure complex network scenarios
- **Commands to Master:** nmcli advanced, firewall-cmd, ip route

#### Day 16: Firewall Configuration
- **Theory:** Firewalld concepts and zones
- **Practice:** Zone management, service/port rules
- **Lab:** Configure firewall for web server scenario
- **Commands to Master:** firewall-cmd (all options)

#### Day 17: SELinux Fundamentals
- **Theory:** security.md (SELinux section)
- **Practice:** SELinux modes, contexts, basic troubleshooting
- **Lab:** Work with SELinux contexts and booleans
- **Commands to Master:** getenforce, setenforce, ls -Z, chcon, restorecon

#### Day 18: SELinux Advanced
- **Theory:** Advanced SELinux concepts
- **Practice:** File contexts, port contexts, booleans
- **Lab:** Troubleshoot SELinux issues, create policies
- **Commands to Master:** semanage, setsebool, audit2allow, sealert

#### Day 19: System Monitoring & Performance
- **Theory:** Performance monitoring tools
- **Practice:** System monitoring, resource usage analysis
- **Lab:** Monitor system performance, identify bottlenecks
- **Commands to Master:** top, htop, iotop, sar, vmstat, iostat

#### Day 20: Kernel & Modules
- **Theory:** boot-process.md (kernel section)
- **Practice:** Kernel parameters, module management
- **Lab:** Load/unload modules, modify kernel parameters
- **Commands to Master:** lsmod, modprobe, modinfo, sysctl

#### Day 21: Review & Practice Exam 3
- **Review:** All Week 3 topics
- **Practice:** Advanced administration scenarios
- **Assessment:** Self-test on advanced topics

### **WEEK 4: Storage & Boot Management (Days 22-28)**

#### Day 22: Disk Partitioning
- **Theory:** storage.md (partitioning section)
- **Practice:** fdisk, parted, partition management
- **Lab:** Create and manage disk partitions
- **Commands to Master:** fdisk, parted, lsblk, blkid

#### Day 23: LVM Basics
- **Theory:** storage.md (LVM section)
- **Practice:** PV, VG, LV creation and management
- **Lab:** Set up LVM, extend volumes
- **Commands to Master:** pvcreate, vgcreate, lvcreate, extend commands

#### Day 24: LVM Advanced & Snapshots
- **Theory:** Advanced LVM features
- **Practice:** LVM snapshots, advanced operations
- **Lab:** Create snapshots, perform LVM migrations
- **Commands to Master:** lvcreate -s, lvconvert, pvmove

#### Day 25: SWAP & fstab Management
- **Theory:** SWAP configuration, fstab
- **Practice:** SWAP creation, fstab entries, mount options
- **Lab:** Configure SWAP, create complex fstab entries
- **Commands to Master:** mkswap, swapon, fstab editing

#### Day 26: Boot Process & GRUB2
- **Theory:** boot-process.md
- **Practice:** GRUB2 configuration, kernel parameters
- **Lab:** Modify GRUB, recover system passwords
- **Commands to Master:** grub2-mkconfig, grub2-editenv

#### Day 27: System Recovery
- **Theory:** Recovery procedures, rescue mode
- **Practice:** Password recovery, system repair
- **Lab:** Practice recovery scenarios
- **Commands to Master:** Recovery procedures, chroot

#### Day 28: Review & Practice Exam 4
- **Review:** All Week 4 topics
- **Practice:** Storage and boot scenarios
- **Assessment:** Self-test on storage/boot topics

### **WEEK 5: Modern Technologies (Days 29-35)**

#### Day 29: Container Basics (Podman)
- **Theory:** containers.md
- **Practice:** Basic container operations
- **Lab:** Run containers, manage images
- **Commands to Master:** podman run, pull, ps, images

#### Day 30: Container Management
- **Theory:** Advanced container concepts
- **Practice:** Volumes, networking, persistent storage
- **Lab:** Create containerized services
- **Commands to Master:** podman volume, network, exec

#### Day 31: Container Services
- **Theory:** Running containers as services
- **Practice:** systemd integration, rootless containers
- **Lab:** Create container services with systemd
- **Commands to Master:** podman generate systemd

#### Day 32: Advanced Storage (RAID, Stratis)
- **Theory:** storage.md (advanced section)
- **Practice:** Software RAID, Stratis pools
- **Lab:** Configure RAID arrays, use Stratis
- **Commands to Master:** mdadm, stratis commands

#### Day 33: Disk Encryption (LUKS)
- **Theory:** LUKS encryption
- **Practice:** Encrypt partitions, manage keys
- **Lab:** Set up encrypted storage
- **Commands to Master:** cryptsetup commands

#### Day 34: Systemd Timers
- **Theory:** scheduling.md (systemd timers)
- **Practice:** Create and manage systemd timers
- **Lab:** Replace cron jobs with timers
- **Commands to Master:** systemctl (timer management)

#### Day 35: Review & Practice Exam 5
- **Review:** All Week 5 topics
- **Practice:** Modern technology scenarios
- **Assessment:** Self-test on containers and advanced storage

### **WEEK 6: Integration & Real-world Scenarios (Days 36-42)**

#### Day 36: Web Server Setup
- **Theory:** Complete web server deployment
- **Practice:** Install Apache/Nginx, configure SSL
- **Lab:** Deploy complete web server with security
- **Integration:** All previous topics combined

#### Day 37: Database Server Setup
- **Theory:** Database server deployment
- **Practice:** Install MariaDB/PostgreSQL, configure
- **Lab:** Deploy database server with backup strategy
- **Integration:** Storage, security, networking combined

#### Day 38: File Server (NFS/Samba)
- **Theory:** Network file sharing
- **Practice:** Configure NFS and Samba shares
- **Lab:** Set up file server with proper permissions
- **Integration:** Networking, security, storage

#### Day 39: Monitoring & Logging Server
- **Theory:** Centralized monitoring and logging
- **Practice:** Configure log aggregation, monitoring
- **Lab:** Set up monitoring infrastructure
- **Integration:** Services, networking, security

#### Day 40: Backup & Recovery Solutions
- **Theory:** Enterprise backup strategies
- **Practice:** Automated backups, recovery procedures
- **Lab:** Implement complete backup solution
- **Integration:** Storage, scheduling, scripting

#### Day 41: Performance Tuning
- **Theory:** System optimization techniques
- **Practice:** Tune system performance
- **Lab:** Optimize system for specific workloads
- **Integration:** All system components

#### Day 42: Comprehensive Integration Lab
- **Practice:** Complete infrastructure deployment
- **Lab:** Deploy multi-service environment
- **Assessment:** Real-world scenario testing

### **WEEK 7: Final Preparation (Days 43-45)**

#### Day 43: Mock Exam 1
- **Full Practice Exam:** 3-hour timed exam simulation
- **Review:** Identify weak areas
- **Remediation:** Focus on problem areas

#### Day 44: Mock Exam 2
- **Full Practice Exam:** 3-hour timed exam simulation
- **Review:** Compare with Day 43 performance
- **Final Preparation:** Last-minute review

#### Day 45: Final Review & Exam Day Prep
- **Quick Review:** All command references
- **Mental Preparation:** Exam strategy review
- **Rest:** Prepare for exam day

---

## 🛠️ Required Lab Environment

### Virtual Machine Setup
- **Recommended:** VMware Workstation or VirtualBox
- **OS:** RHEL 9 or CentOS Stream 9
- **Resources:** 4GB RAM, 40GB disk minimum
- **Network:** NAT + Host-only adapter

### Practice Environment
- **Multiple VMs:** At least 2 VMs for networking practice
- **Snapshots:** Take snapshots before major changes
- **Backup:** Regular backup of practice environment

---

## 📋 Daily Checklist Template

### Before Each Study Session
- [ ] Review previous day's notes
- [ ] Set up practice environment
- [ ] Have command reference ready

### During Study Session
- [ ] Complete theory reading
- [ ] Practice all commands hands-on
- [ ] Complete lab exercises
- [ ] Document new learnings

### After Each Study Session
- [ ] Update command cheat sheet
- [ ] Note any difficulties
- [ ] Plan next day's focus areas
- [ ] Commit progress to Git repository

---

## 🎯 Success Metrics

### Weekly Goals
- **Week 1:** Master basic Linux operations
- **Week 2:** Comfortable with system administration
- **Week 3:** Confident with advanced topics
- **Week 4:** Expert in storage and boot management
- **Week 5:** Proficient with modern technologies
- **Week 6:** Can handle real-world scenarios
- **Week 7:** Exam-ready with 90%+ practice scores

### Daily Metrics
- [ ] All planned commands practiced
- [ ] Lab exercises completed successfully
- [ ] Notes updated in repository
- [ ] Self-assessment score: ___/10

---

## 🚨 Critical Success Factors

1. **Consistency:** Study 2 hours every single day
2. **Hands-on Practice:** Never skip lab exercises
3. **Documentation:** Keep detailed notes
4. **Review:** Regular review of previous topics
5. **Mock Exams:** Take practice exams seriously
6. **Environment:** Maintain consistent practice setup
7. **Health:** Get adequate sleep and breaks

**Remember: The RHCSA is a practical exam. You must be able to perform tasks, not just memorize commands!**
