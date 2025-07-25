# Package Management (DNF/YUM)

## Basic Package Operations
- `dnf search package` - Search for packages
- `dnf info package` - Show package information
- `dnf install package` - Install a package
- `dnf remove package` - Remove a package
- `dnf update` - Update all packages
- `dnf update package` - Update specific package
- `dnf list installed` - List installed packages
- `dnf list available` - List available packages
- `dnf list updates` - List available updates

## Package Groups
- `dnf group list` - List package groups
- `dnf group info "group name"` - Show group information
- `dnf group install "group name"` - Install package group
- `dnf group remove "group name"` - Remove package group

## Repository Management
- `dnf repolist` - List enabled repositories
- `dnf repolist all` - List all repositories
- `dnf config-manager --add-repo URL` - Add repository
- `dnf config-manager --enable repo` - Enable repository
- `dnf config-manager --disable repo` - Disable repository
- `/etc/yum.repos.d/` - Repository configuration directory

## Package History
- `dnf history` - Show transaction history
- `dnf history info ID` - Show details of transaction
- `dnf history undo ID` - Undo a transaction
- `dnf history redo ID` - Redo a transaction

## Local Package Installation
- `dnf localinstall package.rpm` - Install local RPM
- `rpm -ivh package.rpm` - Install RPM with verbose output
- `rpm -Uvh package.rpm` - Upgrade RPM package
- `rpm -e package` - Remove RPM package
- `rpm -qa` - List all installed RPMs
- `rpm -qi package` - Show RPM package info
- `rpm -ql package` - List files in RPM package
- `rpm -qf /path/to/file` - Find which package owns file

## DNF Modules (RHEL 8+)
- `dnf module list` - List available modules
- `dnf module info module:stream` - Show module information
- `dnf module install module:stream` - Install module stream
- `dnf module enable module:stream` - Enable module stream
- `dnf module disable module` - Disable module
- `dnf module reset module` - Reset module

## Package Verification
- `rpm -V package` - Verify package files
- `rpm -Va` - Verify all packages
- `dnf check` - Check for problems

## Cache Management
- `dnf clean all` - Clean all cache
- `dnf makecache` - Download repository metadata
- `dnf repoquery` - Query repositories
