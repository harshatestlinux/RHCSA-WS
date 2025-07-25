# File System Management

## Mounting File Systems
- `mount [options] device mountpoint` - Mount a filesystem
  - `-t type` - Specify filesystem type (ext4, xfs, etc.)
  - `-o options` - Mount options (ro, rw, noexec, etc.)
  - `-a` - Mount all filesystems in /etc/fstab
- `umount [options] mountpoint|device` - Unmount a filesystem
  - `-l` - Lazy unmount
- `findmnt` - Show all mounted filesystems
- `lsblk` - List block devices

## File System Creation and Maintenance
- `mkfs.[type] [options] device` - Create a filesystem
  - `mkfs.ext4 /dev/sdX1`
  - `mkfs.xfs /dev/sdX1`
- `fsck [options] device` - Check and repair a filesystem
  - `-f` - Force check even if clean
  - `-y` - Automatically repair
- `tune2fs [options] device` - Tune ext2/3/4 filesystem parameters
  - `-l` - Show superblock information
  - `-c max-mount-counts` - Set max mount count
  - `-i interval-between-checks` - Set interval between checks

## Disk Usage
- `df [options]` - Show disk space usage
  - `-h` - Human-readable format
  - `-T` - Show filesystem type
- `du [options] [file/directory]` - Show directory space usage
  - `-h` - Human-readable format
  - `-s` - Summary total
  - `--max-depth=N` - Show total for directories N levels deep

## LVM (Logical Volume Management)
### Physical Volumes
- `pvcreate /dev/sdX` - Initialize a disk for LVM
- `pvdisplay` - Display physical volumes
- `pvmove /dev/sdX` - Move data off a PV
- `pvextend /dev/sdX` - Extend a PV

### Volume Groups
- `vgcreate vg_name /dev/sdX` - Create a VG
- `vgextend vg_name /dev/sdY` - Add a PV to VG
- `vgreduce vg_name /dev/sdY` - Remove a PV from VG
- `vgdisplay` - Display VG information

### Logical Volumes
- `lvcreate -L size -n lv_name vg_name` - Create LV
- `lvextend -L +size /dev/vg_name/lv_name` - Extend LV
- `lvreduce -L -size /dev/vg_name/lv_name` - Reduce LV
- `lvdisplay` - Display LV information
- `lvremove /dev/vg_name/lv_name` - Remove LV

## File System Quotas
- `quotacheck -cugm /mount/point` - Check quota files
- `edquota username` - Edit user quotas
- `quotaon /mount/point` - Enable quotas
- `quotaoff /mount/point` - Disable quotas
- `repquota /mount/point` - Report on quotas

## File System Types
- `ext4` - Standard Linux filesystem
- `xfs` - High-performance filesystem
- `vfat` - Windows-compatible filesystem
- `nfs` - Network File System
- `tmpfs` - Temporary filesystem in RAM
