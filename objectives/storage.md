# Storage Management

## Partitioning
- `fdisk -l` - List all disk partitions
- `fdisk /dev/sdX` - Interactive partition editor
- `parted /dev/sdX` - GNU parted partition editor
- `lsblk` - List block devices in tree format
- `blkid` - Show block device attributes

## Partition Commands (fdisk)
- `n` - Create new partition
- `d` - Delete partition
- `p` - Print partition table
- `w` - Write changes and exit
- `q` - Quit without saving
- `t` - Change partition type

## SWAP Management
- `swapon -s` - Show swap usage
- `swapon /dev/sdX1` - Enable swap on partition
- `swapoff /dev/sdX1` - Disable swap on partition
- `mkswap /dev/sdX1` - Create swap filesystem
- `swapon -a` - Enable all swap in /etc/fstab

## /etc/fstab Configuration
```
# Device    Mount Point    FS Type    Options    Dump    Pass
/dev/sda1   /              ext4       defaults   1       1
/dev/sda2   /home          ext4       defaults   1       2
/dev/sda3   swap           swap       defaults   0       0
UUID=xxx    /data          xfs        defaults   0       2
```

### fstab Options
- `defaults` - Default mount options
- `noauto` - Don't mount automatically at boot
- `user` - Allow users to mount
- `ro` - Read-only
- `rw` - Read-write
- `noexec` - Don't allow execution
- `nosuid` - Ignore suid bits

## LVM Advanced Operations
### Extending Filesystems
- `lvextend -L +1G /dev/vg/lv` - Extend LV by 1GB
- `resize2fs /dev/vg/lv` - Resize ext4 filesystem
- `xfs_growfs /mount/point` - Resize XFS filesystem

### LVM Snapshots
- `lvcreate -L 1G -s -n snap /dev/vg/lv` - Create snapshot
- `lvremove /dev/vg/snap` - Remove snapshot
- `lvconvert --merge /dev/vg/snap` - Merge snapshot back

## RAID Management
- `mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1` - Create RAID 1
- `mdadm --detail /dev/md0` - Show RAID details
- `cat /proc/mdstat` - Show RAID status
- `mdadm --add /dev/md0 /dev/sdc1` - Add device to RAID
- `mdadm --fail /dev/md0 /dev/sda1` - Mark device as failed
- `mdadm --remove /dev/md0 /dev/sda1` - Remove device from RAID

## Disk Encryption (LUKS)
- `cryptsetup luksFormat /dev/sdX1` - Format partition with LUKS
- `cryptsetup luksOpen /dev/sdX1 myencrypted` - Open encrypted partition
- `cryptsetup luksClose myencrypted` - Close encrypted partition
- `cryptsetup luksAddKey /dev/sdX1` - Add new passphrase
- `cryptsetup luksRemoveKey /dev/sdX1` - Remove passphrase

## Stratis (Modern Storage)
- `stratis pool create mypool /dev/sdb` - Create storage pool
- `stratis filesystem create mypool myfs` - Create filesystem
- `stratis pool list` - List storage pools
- `stratis filesystem list` - List filesystems
- `mount /stratis/mypool/myfs /mnt` - Mount stratis filesystem

## VDO (Virtual Data Optimizer)
- `vdo create --name=myvdo --device=/dev/sdb --vdoLogicalSize=10T` - Create VDO
- `vdo list` - List VDO volumes
- `vdo status --name=myvdo` - Show VDO status
- `vdo start --name=myvdo` - Start VDO volume
- `vdo stop --name=myvdo` - Stop VDO volume
