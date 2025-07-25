# User and Group Management

## User Management
- `useradd [options] username` - Create new user
  - `-c "Comment"` - User description
  - `-d /home/username` - Home directory
  - `-s /bin/bash` - Default shell
  - `-G group1,group2` - Additional groups
  - `-u UID` - User ID
- `usermod [options] username` - Modify user account
  - `-l new_username` - Change username
  - `-aG group` - Add user to supplementary group
  - `-L` - Lock account
  - `-U` - Unlock account
- `userdel [options] username` - Delete user account
  - `-r` - Remove home directory and mail spool

## Group Management
- `groupadd [options] groupname` - Create new group
  - `-g GID` - Group ID
- `groupmod [options] groupname` - Modify group
  - `-n newname` - Change group name
- `groupdel groupname` - Delete group

## Password Management
- `passwd [username]` - Change password
  - `-l` - Lock user's password
  - `-u` - Unlock user's password
  - `-e` - Expire password
- `chage [options] username` - Change password expiry information
  - `-l` - Show account aging information
  - `-E YYYY-MM-DD` - Account expiration date
  - `-M days` - Maximum password lifetime

## File Permissions
- `chmod [options] mode file` - Change file mode bits
  - Symbolic: `u=rwx,g=rx,o=r`
  - Octal: `755` (rwxr-xr-x)
- `chown [options] user:group file` - Change file owner and group
  - `-R` - Recursive
- `chgrp [options] group file` - Change group ownership
- `umask [value]` - Set default file permissions

## Sudo Configuration
- `visudo` - Edit the sudoers file
- `/etc/sudoers` - Main configuration file
  - `username ALL=(ALL) ALL` - Full sudo access
  - `%group ALL=(ALL) ALL` - Group-based sudo access

## User Information
- `id [username]` - Show user and group information
- `who` - Show who is logged on
- `w` - Show who is logged on and what they are doing
- `last` - Show listing of last logged in users
- `finger [username]` - User information lookup
