# Security and SELinux

## SELinux Basics
- `getenforce` - Show current SELinux mode
- `setenforce 0|1` - Set SELinux to permissive (0) or enforcing (1)
- `/etc/selinux/config` - SELinux configuration file
- `sestatus` - Show detailed SELinux status

## SELinux Modes
- **Enforcing** - SELinux policy is enforced
- **Permissive** - SELinux prints warnings instead of enforcing
- **Disabled** - SELinux is turned off

## SELinux Contexts
- `ls -Z` - Show SELinux contexts of files
- `ps -Z` - Show SELinux contexts of processes
- `id -Z` - Show SELinux context of current user
- `chcon -t httpd_exec_t /path/to/file` - Change file context
- `restorecon /path/to/file` - Restore default context
- `restorecon -R /path/to/directory` - Restore contexts recursively

## SELinux File Contexts
- `semanage fcontext -l` - List file context rules
- `semanage fcontext -a -t httpd_exec_t "/custom/path(/.*)?"` - Add context rule
- `semanage fcontext -d "/custom/path(/.*)?"` - Delete context rule

## SELinux Booleans
- `getsebool -a` - List all SELinux booleans
- `getsebool httpd_can_network_connect` - Check specific boolean
- `setsebool httpd_can_network_connect on` - Set boolean temporarily
- `setsebool -P httpd_can_network_connect on` - Set boolean permanently
- `semanage boolean -l` - List booleans with descriptions

## SELinux Ports
- `semanage port -l` - List port contexts
- `semanage port -a -t http_port_t -p tcp 8080` - Add port context
- `semanage port -d -t http_port_t -p tcp 8080` - Delete port context

## SELinux Troubleshooting
- `ausearch -m avc -ts recent` - Search for recent AVC denials
- `sealert -a /var/log/audit/audit.log` - Analyze SELinux denials
- `audit2allow -a` - Generate policy rules from denials
- `audit2why < /var/log/audit/audit.log` - Explain why access was denied

## File Permissions and ACLs
- `getfacl file` - Show file ACLs
- `setfacl -m u:user:rwx file` - Set user ACL
- `setfacl -m g:group:rx file` - Set group ACL
- `setfacl -x u:user file` - Remove user ACL
- `setfacl -b file` - Remove all ACLs
- `setfacl -R -m u:user:rwx directory` - Set ACLs recursively
- `setfacl -d -m u:user:rwx directory` - Set default ACLs

## SSH Security
- `ssh-keygen -t rsa -b 4096` - Generate strong SSH key
- `ssh-copy-id user@host` - Copy public key to remote host
- `/etc/ssh/sshd_config` - SSH daemon configuration
  - `PermitRootLogin no` - Disable root login
  - `PasswordAuthentication no` - Disable password auth
  - `Port 2222` - Change SSH port

## Sudo Configuration
- `visudo` - Edit sudoers file safely
- `/etc/sudoers.d/` - Additional sudo configuration files
- `sudo -l` - List sudo privileges for current user
- `sudo -u user command` - Run command as specific user

## Password Policies
- `/etc/login.defs` - Default password settings
- `/etc/security/pwquality.conf` - Password quality settings
- `chage -l username` - Show password aging info
- `chage -M 90 username` - Set password max age to 90 days
- `passwd -l username` - Lock user account
- `passwd -u username` - Unlock user account
