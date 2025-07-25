# Boot Process and Kernel Management

## GRUB2 Boot Loader
- `/boot/grub2/grub.cfg` - GRUB2 configuration file (don't edit directly)
- `/etc/default/grub` - GRUB2 default settings
- `grub2-mkconfig -o /boot/grub2/grub.cfg` - Regenerate GRUB config
- `grub2-editenv list` - Show GRUB environment variables
- `grub2-set-default 0` - Set default boot entry

## Boot Process Steps
1. **BIOS/UEFI** - Hardware initialization
2. **Boot Loader (GRUB2)** - Load kernel and initramfs
3. **Kernel** - Initialize hardware and mount root filesystem
4. **systemd** - Start services and reach target

## Kernel Parameters
- `cat /proc/cmdline` - Show current kernel parameters
- Edit GRUB entry at boot: press 'e' and modify linux line
- Permanent changes: edit `/etc/default/grub` and run `grub2-mkconfig`

## Common Kernel Parameters
- `systemd.unit=rescue.target` - Boot to rescue mode
- `systemd.unit=emergency.target` - Boot to emergency mode
- `rd.break` - Break into initramfs shell
- `init=/bin/bash` - Boot directly to bash shell
- `selinux=0` - Disable SELinux
- `enforcing=0` - Set SELinux to permissive

## Password Recovery
1. Boot system and interrupt GRUB
2. Edit kernel line, add `rd.break`
3. Boot into initramfs shell
4. `mount -o remount,rw /sysroot`
5. `chroot /sysroot`
6. `passwd root`
7. `touch /.autorelabel` (if SELinux enabled)
8. `exit` and `reboot`

## Kernel Management
- `uname -r` - Show current kernel version
- `uname -a` - Show all system information
- `lsmod` - List loaded kernel modules
- `modinfo module` - Show module information
- `modprobe module` - Load kernel module
- `modprobe -r module` - Remove kernel module
- `rmmod module` - Remove module (if not in use)

## Module Configuration
- `/etc/modprobe.d/` - Module configuration directory
- `/etc/modules-load.d/` - Modules to load at boot
- `lsmod | grep module` - Check if module is loaded
- `modprobe -c` - Show module configuration

## Initramfs
- `lsinitrd` - List contents of initramfs
- `dracut` - Regenerate initramfs
- `dracut -f` - Force regenerate initramfs
- `/boot/initramfs-$(uname -r).img` - Current initramfs file

## System Recovery
### Rescue Mode
- `systemctl rescue` - Switch to rescue mode
- Boot with `systemd.unit=rescue.target`

### Emergency Mode
- `systemctl emergency` - Switch to emergency mode
- Boot with `systemd.unit=emergency.target`

### Single User Mode
- Boot with `single` or `1` parameter
- Minimal system with root access
