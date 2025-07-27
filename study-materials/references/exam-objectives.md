# RHCSA Exam Objectives (EX200) - Complete Coverage

## Official Red Hat RHCSA Exam Objectives

### Understand and use essential tools
- [ ] Access a shell prompt and issue commands with correct syntax
- [ ] Use input-output redirection (>, >>, |, 2>, etc.)
- [ ] Use grep and regular expressions to analyze text
- [ ] Access remote systems using SSH
- [ ] Log in and switch users in multiuser targets
- [ ] Archive, compress, unpack, and uncompress files using tar, star, gzip, and bzip2
- [ ] Create and edit text files
- [ ] Create, delete, copy, and move files and directories
- [ ] Create links
- [ ] List, set, and change standard ugo/rwx permissions
- [ ] Locate, read, and use system documentation including man, info, and files in /usr/share/doc

### Create simple shell scripts
- [ ] Conditionally execute code (use of: if, test, [], etc.)
- [ ] Use Looping constructs (for, etc.) to process file, command line input
- [ ] Process script inputs ($1, $2, etc.)
- [ ] Processing output of shell commands within a script

### Operate running systems
- [ ] Boot, reboot, and shut down a system normally
- [ ] Boot systems into different targets manually
- [ ] Interrupt the boot process in order to gain access to a system
- [ ] Identify CPU/memory intensive processes and kill processes
- [ ] Adjust process scheduling
- [ ] Manage tuning profiles
- [ ] Locate and interpret system log files and journals
- [ ] Preserve system journals
- [ ] Start, stop, and check the status of network services
- [ ] Securely transfer files between systems

### Configure local storage
- [ ] List, create, delete partitions on MBR and GPT disks
- [ ] Create and remove physical volumes
- [ ] Assign physical volumes to volume groups
- [ ] Create and delete logical volumes
- [ ] Configure systems to mount file systems at boot by universally unique ID (UUID) or label
- [ ] Add new partitions and logical volumes, and swap to a system non-destructively

### Create and configure file systems
- [ ] Create, mount, unmount, and use vfat, ext4, and xfs file systems
- [ ] Mount and unmount network file systems using NFS
- [ ] Configure autofs
- [ ] Extend existing logical volumes
- [ ] Create and configure set-GID directories for collaboration
- [ ] Diagnose and correct file permission problems

### Deploy, configure, and maintain systems
- [ ] Schedule tasks using at and cron
- [ ] Start and stop services and configure services to start automatically at boot
- [ ] Configure systems to boot into a specific target automatically
- [ ] Configure time service clients
- [ ] Install and update software packages from Red Hat Network, a remote repository, or from the local file system
- [ ] Modify the system bootloader

### Manage basic networking
- [ ] Configure IPv4 and IPv6 addresses
- [ ] Configure hostname resolution
- [ ] Configure network services to start automatically at boot
- [ ] Restrict network access using firewall-cmd/firewall

### Manage users and groups
- [ ] Create, delete, and modify local user accounts
- [ ] Change passwords and adjust password aging for local user accounts
- [ ] Create, delete, and modify local groups and group memberships
- [ ] Configure superuser access

### Manage security
- [ ] Configure firewall settings using firewall-cmd/firewalld
- [ ] Manage default file permissions
- [ ] Configure key-based authentication for SSH
- [ ] Set enforcing and permissive modes for SELinux
- [ ] List and identify SELinux file and process context
- [ ] Restore default file contexts
- [ ] Manage SELinux port labels
- [ ] Use boolean settings to modify system SELinux settings
- [ ] Diagnose and address routine SELinux policy violations

## Exam Format Details

### Exam Information
- **Duration:** 3 hours
- **Format:** Performance-based (hands-on)
- **Passing Score:** 210/300 (70%)
- **Environment:** Red Hat Enterprise Linux 9
- **Access:** Local console access to multiple systems

### What You Can Use During Exam
- [ ] System documentation (man pages, info pages, /usr/share/doc)
- [ ] Built-in help commands (--help options)
- [ ] Red Hat documentation (if available on local system)

### What You Cannot Use
- [ ] Internet access
- [ ] External documentation
- [ ] Notes or cheat sheets
- [ ] Communication with others

## Critical Success Tips

### Time Management
- **Read all tasks first** (5-10 minutes)
- **Prioritize high-point tasks**
- **Don't spend too much time on one task**
- **Leave time for verification** (30 minutes)

### Common Mistakes to Avoid
- [ ] Not reading instructions carefully
- [ ] Forgetting to enable services at boot
- [ ] Not testing configurations
- [ ] Incorrect file permissions
- [ ] SELinux context issues
- [ ] Firewall blocking services
- [ ] Not making changes persistent

### Verification Checklist
- [ ] Services start automatically at boot
- [ ] Firewall rules allow required access
- [ ] SELinux contexts are correct
- [ ] File permissions are appropriate
- [ ] Configurations survive reboot
- [ ] All requirements are met exactly

## Task Categories and Point Distribution

### High-Point Tasks (Focus Areas)
1. **Storage Management** (LVM, partitions, file systems)
2. **User and Group Management**
3. **Network Configuration**
4. **Service Management**
5. **Security (SELinux, firewall)**

### Medium-Point Tasks
1. **Boot Process Management**
2. **Package Management**
3. **Task Scheduling**
4. **File Permissions and ACLs**

### Lower-Point Tasks
1. **Basic file operations**
2. **Text processing**
3. **System monitoring**
4. **Documentation usage**

## Pre-Exam Preparation

### Day Before Exam
- [ ] Review all command syntax
- [ ] Practice common task combinations
- [ ] Get good night's sleep
- [ ] Prepare required identification

### Exam Day
- [ ] Arrive early
- [ ] Bring valid ID
- [ ] Stay calm and focused
- [ ] Read all instructions carefully
- [ ] Manage time effectively
