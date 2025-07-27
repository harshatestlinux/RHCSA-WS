# RHCSA Troubleshooting & Disaster Recovery Lab Set

Mastering troubleshooting and disaster recovery is key for both the RHCSA exam and real-world sysadmin work. Each scenario below is designed to simulate a real emergency. Attempt to solve each issue as if you were on-call!

---

## Lab 1: Boot Failure – Broken /etc/fstab

**Scenario:**
- The system fails to boot after a recent reboot. You suspect a typo in `/etc/fstab`.

**Tasks:**
1. Boot the system into rescue or emergency mode.
2. Mount the root filesystem if not done automatically.
3. Locate and fix the error in `/etc/fstab`.
4. Reboot and verify normal operation.

**Solution:**
1. Reboot and at the GRUB menu, select your kernel and press `e` to edit.
2. Add `rd.break` or `systemd.unit=rescue.target` to the kernel line and boot.
3. Mount the root filesystem if not already mounted:
   ```bash
   mount -o remount,rw /
   ```
4. Edit `/etc/fstab` and correct any typos (e.g., wrong device, missing fields).
5. Save and exit. Reboot:
   ```bash
   reboot
   ```
6. System should boot normally.

---


## Lab 3: SELinux Blocking a Service

**Scenario:**
- The Apache (httpd) service will not start. SELinux is enforcing.

**Tasks:**
1. Use `journalctl` and `sealert` to diagnose the failure.
2. Apply the correct SELinux context to `/var/www/html` if needed.
3. Use `restorecon`, `chcon`, or `semanage fcontext` as appropriate.
4. Restart httpd and verify it works.

---

## Lab 4: Network Misconfiguration

**Scenario:**
- The system cannot reach the network after a manual change to network configs.

**Tasks:**
1. Use `nmcli` and `ip` to diagnose the issue.
2. Fix any errors in `/etc/sysconfig/network-scripts/` or NetworkManager configs.
3. Restart networking and verify connectivity.

---

## Lab 5: Service Fails to Start

**Scenario:**
- The `sshd` service fails to start after a config change.

**Tasks:**
1. Use `systemctl status sshd` and `journalctl -xe` to find the error.
2. Fix the error in `/etc/ssh/sshd_config` (e.g., invalid option or typo).
3. Reload the service and verify remote SSH access.

---

## Lab 6: Disk Full – Emergency Cleanup

**Scenario:**
- The root filesystem is 100% full. Normal commands may fail.

**Tasks:**
1. Use `du`, `find`, and `ls` to locate large files.
2. Remove or compress unnecessary files (check `/var/log`, `/tmp`, `/home`).
3. Free up enough space for normal operation.

---

## Lab 7: LVM Recovery – Missing Physical Volume

**Scenario:**
- A physical volume in a VG is missing (e.g., after disk replacement or unplug).

**Tasks:**
1. Use `vgs`, `lvs`, and `pvs` to diagnose the problem.
2. Re-add the PV if possible, or restore from backup/snapshot.
3. Activate the LV and verify data access.

---

## Lab 8: Firewall Lockout

**Scenario:**
- You applied a new firewall rule and lost remote access.

**Tasks:**
1. Access the system locally or via console.
2. Use `firewall-cmd` to fix the rules.
3. Restore remote access and document a safer workflow for future changes.

---

## Pro Tips & Checklist
- Always check logs (`journalctl`, `/var/log/messages`, `/var/log/audit/audit.log`).
- Use rescue/emergency mode for unbootable systems.
- Practice with both SELinux enforcing and permissive modes.
- Document every step you take—this is good practice for the real world and the exam!

---

## Lab 13: User Cannot Write to Home Directory

**Scenario:**
- User `alice` complains she cannot create files in her home directory after a recent permission change.

**Tasks:**
1. Diagnose the issue with permissions or ownership.
2. Restore correct permissions and ownership for `/home/alice`.

**Solution:**
1. Check permissions:
   ```bash
   ls -ld /home/alice
   ```
2. Correct ownership and permissions:
   ```bash
   chown alice:alice /home/alice
   chmod 700 /home/alice
   ```

---

## Lab 14: Container Networking Broken

**Scenario:**
- A Podman container running Apache is not accessible from the host on port 8080.

**Tasks:**
1. Diagnose the issue (container, firewall, or SELinux).
2. Fix so the web service is accessible on `http://localhost:8080`.

**Solution:**
1. Check if container is running and port is mapped:
   ```bash
   podman ps
   podman port <container_id>
   ```
2. Check firewall:
   ```bash
   firewall-cmd --list-ports
   firewall-cmd --add-port=8080/tcp --permanent
   firewall-cmd --reload
   ```
3. Check SELinux:
   ```bash
   getenforce
   # If enforcing, ensure correct context or set to permissive for test
   setsebool -P httpd_can_network_connect 1
   ```

---

## Lab 15: Package Installation Fails Due to Missing Repo

**Scenario:**
- `dnf install` fails with "No available repository" error after a repo configuration change.

**Tasks:**
1. Diagnose the repo configuration.
2. Restore access to the required repository.

**Solution:**
1. Check repo files:
   ```bash
   ls /etc/yum.repos.d/
   cat /etc/yum.repos.d/*.repo
   dnf repolist all
   ```
2. Fix or restore the repo file (e.g., enable `enabled=1`, correct baseurl).
3. Clean metadata and retry:
   ```bash
   dnf clean all
   dnf repolist
   dnf install <package>
   ```

---

## Lab 16: SSH Key Authentication Not Working

**Scenario:**
- User `bob` cannot log in via SSH key authentication, but password login works.

**Tasks:**
1. Diagnose the SSH key setup for `bob`.
2. Fix permissions and configuration for key-based login.

**Solution:**
1. Check permissions:
   ```bash
   ls -ld /home/bob /home/bob/.ssh
   ls -l /home/bob/.ssh/authorized_keys
   ```
2. Correct permissions:
   ```bash
   chmod 700 /home/bob/.ssh
   chmod 600 /home/bob/.ssh/authorized_keys
   chown -R bob:bob /home/bob/.ssh
   ```
3. Check `/etc/ssh/sshd_config` for `PubkeyAuthentication yes` and reload sshd:
   ```bash
   systemctl reload sshd
   ```

---

## Lab 17: System Time Incorrect After Reboot

**Scenario:**
- System time is wrong after every reboot; NTP is installed but not syncing.

**Tasks:**
1. Diagnose the time sync issue.
2. Ensure time is correct and persists across reboots.

**Solution:**
1. Check chrony/ntpd status:
   ```bash
   systemctl status chronyd
   chronyc tracking
   ```
2. Enable and start chronyd:
   ```bash
   systemctl enable --now chronyd
   ```
3. Force a time sync:
   ```bash
   chronyc makestep
   ```
4. Ensure correct servers are set in `/etc/chrony.conf`.

---

## Lab 18: Cannot Mount NFS Share

**Scenario:**
- An NFS share at `192.168.1.100:/exports/data` fails to mount at `/mnt/nfsdata`.

**Tasks:**
1. Diagnose the mount failure.
2. Fix the issue so the share mounts successfully and persists after reboot.

**Solution:**
1. Check network connectivity:
   ```bash
   ping 192.168.1.100
   ```
2. Install NFS utilities if missing:
   ```bash
   dnf install -y nfs-utils
   ```
3. Check `/etc/fstab` entry:
   ```bash
   echo '192.168.1.100:/exports/data /mnt/nfsdata nfs defaults 0 0' >> /etc/fstab
   mount -a
   ```
4. Check firewall and SELinux if still failing:
   ```bash
   firewall-cmd --add-service=nfs --permanent
   firewall-cmd --reload
   setsebool -P use_nfs_home_dirs 1
   ```

---

## Lab 19: User Locked Out After Too Many Failed Logins

**Scenario:**
- User `dave` is locked out after repeated failed login attempts due to `pam_faillock`.

**Tasks:**
1. Diagnose the lockout.
2. Unlock the user account.

**Solution:**
1. Check faillock status:
   ```bash
   faillock --user dave
   ```
2. Unlock the account:
   ```bash
   faillock --user dave --reset
   ```

---

## Lab 20: LVM Volume Full, Filesystem Cannot Grow

**Scenario:**
- The `/data` filesystem on an LVM logical volume is full; you have free space in the volume group.

**Tasks:**
1. Extend the logical volume and filesystem by 500MB.

**Solution:**
1. Check free space:
   ```bash
   vgs
   lvs
   ```
2. Extend LV and filesystem (assuming XFS):
   ```bash
   lvextend -L +500M /dev/vg_data/lv_data
   xfs_growfs /data
   ```

---

## Lab 21: NetworkManager Connection Not Auto-Starting

**Scenario:**
- After reboot, the `eth1` interface does not come up automatically.

**Tasks:**
1. Diagnose and fix the issue so `eth1` auto-starts at boot.

**Solution:**
1. Check connection settings:
   ```bash
   nmcli con show
   nmcli con mod eth1 connection.autoconnect yes
   nmcli con up eth1
   ```

---

## Lab 22: SELinux Prevents Custom Script Execution

**Scenario:**
- A script `/usr/local/bin/backup.sh` cannot be executed by cron due to SELinux.

**Tasks:**
1. Diagnose SELinux denial.
2. Fix the context so cron can execute the script.

**Solution:**
1. Check audit logs:
   ```bash
   ausearch -m avc -ts recent
   sealert -a /var/log/audit/audit.log
   ```
2. Restore correct context:
   ```bash
   restorecon -v /usr/local/bin/backup.sh
   ```
3. If needed, use semanage:
   ```bash
   semanage fcontext -a -t bin_t /usr/local/bin/backup.sh
   restorecon -v /usr/local/bin/backup.sh
   ```

---

## Lab 23: Firewall Blocks DNS Resolution

**Scenario:**
- After enabling firewalld, DNS resolution stops working.

**Tasks:**
1. Diagnose and fix the firewall rules to allow DNS.

**Solution:**
1. Add DNS service to firewall:
   ```bash
   firewall-cmd --add-service=dns --permanent
   firewall-cmd --reload
   ```
2. Test with:
   ```bash
   dig google.com
   ```

---

## Lab 24: Podman Container Cannot Write to Host Volume

**Scenario:**
- A Podman container fails to write to a mounted host directory due to permissions or SELinux.

**Tasks:**
1. Diagnose and fix so the container can write to the volume.

**Solution:**
1. Check host directory permissions:
   ```bash
   ls -ld /srv/ctodata
   chmod 777 /srv/ctodata  # or set appropriate owner
   ```
2. If SELinux is enforcing, use correct context:
   ```bash
   chcon -Rt container_file_t /srv/ctodata
   ```

---

## Lab 25: Service Fails Due to Missing Dependency

**Scenario:**
- The `httpd` service fails to start because a required package is missing.

**Tasks:**
1. Diagnose the missing dependency.
2. Install the required package and start the service.

**Solution:**
1. Check status and logs:
   ```bash
   systemctl status httpd
   journalctl -xe
   ```
2. Identify missing package (e.g., `mod_ssl`):
   ```bash
   dnf install mod_ssl
   systemctl restart httpd
   ```

---

## Lab 26: User Added to Group, Permissions Not Reflected Until Re-login

**Scenario:**
- User `carol` is added to the `wheel` group, but cannot use `sudo` until she logs out and back in.

**Tasks:**
1. Explain why this happens and how to verify group membership.

**Solution:**
- Group membership is determined at login. Use `id` to check current groups:
   ```bash
   id carol
   ```
- To apply new group membership, user must log out and back in, or use `newgrp wheel` to update group in current session.

---

## Lab 27: Cron Job Not Running

**Scenario:**
- A cron job for user `eve` is not executing as expected.

**Tasks:**
1. Diagnose why the cron job is not running.
2. Fix the issue.

**Solution:**
1. Check cron logs:
   ```bash
   grep CRON /var/log/cron
   ```
2. Check permissions and paths in the crontab:
   ```bash
   crontab -u eve -l
   which <command>
   ```
3. Ensure scripts are executable and have correct shebang (`#!/bin/bash`).

---

## Lab 28: Disk Quota Exceeded

**Scenario:**
- User `frank` cannot create files on `/home` due to quota exceeded.

**Tasks:**
1. Diagnose and fix user quota on `/home`.

**Solution:**
1. Check quota:
   ```bash
   repquota -a
   quota -u frank
   ```
2. Adjust quota:
   ```bash
   edquota -u frank
   ```
3. User can create files once quota is increased.

---

## Lab 29: System Boots to Emergency Mode Due to Missing Mount

**Scenario:**
- System fails to boot normally because of a missing disk referenced in `/etc/fstab`.

**Tasks:**
1. Diagnose and fix the boot issue.

**Solution:**
1. At emergency shell, edit `/etc/fstab` and comment out or fix the missing mount line.
2. Reboot:
   ```bash
   reboot
   ```

---

## Lab 30: Cannot Enable and Start a Systemd Service

**Scenario:**
- Attempting to enable and start a custom systemd service fails.

**Tasks:**
1. Diagnose and fix the unit file so the service starts and enables at boot.

**Solution:**
1. Check the unit file for syntax errors:
   ```bash
   systemctl status <service>
   journalctl -xe
   systemd-analyze verify /etc/systemd/system/<service>.service
   ```
2. Fix errors (missing `[Install]` section, wrong `ExecStart`, etc.), reload systemd:
   ```bash
   systemctl daemon-reload
   systemctl enable --now <service>
   ```

---

## Lab 31: Multi-User File Access with ACLs

**Scenario:**
- Both `alice` and `bob` need full access to `/project/data`, but not other users.

**Tasks:**
1. Configure permissions so both users have full access, even if not in the same group.

**Solution:**
1. Set base permissions and use ACLs:
   ```bash
   mkdir -p /project/data
   chown root:root /project/data
   chmod 770 /project/data
   setfacl -m u:alice:rwx /project/data
   setfacl -m u:bob:rwx /project/data
   getfacl /project/data
   ```

---

## Lab 32: LVM Snapshot and Restore

**Scenario:**
- You need to take a snapshot of `/dev/vg_data/lv_data` before a risky operation and restore if needed.

**Tasks:**
1. Create a snapshot, simulate a change, and roll back using the snapshot.

**Solution:**
1. Create a snapshot:
   ```bash
   lvcreate -L 200M -s -n data_snap /dev/vg_data/lv_data
   ```
2. (Simulate change: e.g., remove files)
3. Roll back:
   ```bash
   umount /data
   lvconvert --merge /dev/vg_data/data_snap
   mount /data
   ```

---

## Lab 33: Chain Reaction – SELinux, Firewall, and Service

**Scenario:**
- Apache is running, but cannot serve content from `/srv/webdata` due to both SELinux and firewall issues.

**Tasks:**
1. Diagnose and fix both SELinux and firewall so the site is accessible on port 8081.

**Solution:**
1. Fix SELinux:
   ```bash
   semanage fcontext -a -t httpd_sys_content_t '/srv/webdata(/.*)?'
   restorecon -Rv /srv/webdata
   setsebool -P httpd_can_network_connect 1
   ```
2. Open firewall port:
   ```bash
   firewall-cmd --add-port=8081/tcp --permanent
   firewall-cmd --reload
   ```
3. Update Apache config to listen on 8081 and restart:
   ```bash
   # Edit /etc/httpd/conf/httpd.conf
   systemctl restart httpd
   ```

---

## Lab 34: Container Fails with Permission Denied on Host Volume

**Scenario:**
- A Podman container running as a non-root user fails to write to `/mnt/shared` mounted from the host.

**Tasks:**
1. Diagnose and fix the permission and SELinux issues for persistent container writes.

**Solution:**
1. Ensure host directory is owned by the container user UID:
   ```bash
   chown 1000:1000 /mnt/shared
   chmod 770 /mnt/shared
   ```
2. Set SELinux context:
   ```bash
   chcon -Rt container_file_t /mnt/shared
   ```

---

## Lab 35: Systemd Timer Not Executing

**Scenario:**
- A systemd timer is enabled but the associated service never runs.

**Tasks:**
1. Diagnose the timer and service unit files and fix the issue.

**Solution:**
1. Check timer and service status:
   ```bash
   systemctl status <timer>
   systemctl status <service>
   systemctl list-timers
   ```
2. Verify `[Install]` and `[Service]` sections, correct `ExecStart`, and ensure timer is enabled:
   ```bash
   systemctl daemon-reload
   systemctl enable --now <timer>
   ```

---

## Lab 36: Root Filesystem Remounted Read-Only

**Scenario:**
- After a disk error, `/` is remounted read-only and system is unstable.

**Tasks:**
1. Diagnose and repair filesystem, then remount as read-write.

**Solution:**
1. Check dmesg and logs for disk errors:
   ```bash
   dmesg | grep -i error
   journalctl -xb
   ```
2. Reboot into rescue mode, run fsck:
   ```bash
   fsck -y /dev/mapper/rhel-root
   reboot
   ```
3. Remount `/` as read-write if needed:
   ```bash
   mount -o remount,rw /
   ```

---

## Lab 37: Sudoers Syntax Error Locks Out All Sudo Access

**Scenario:**
- A typo in `/etc/sudoers` or a file in `/etc/sudoers.d/` prevents all sudo access.

**Tasks:**
1. Regain root access and fix the syntax error.

**Solution:**
1. Switch to root via direct login or by using the modern RHCSA 9-approved recovery method (e.g., rd.break and chroot).
2. Use `visudo` to fix `/etc/sudoers` or remove/repair broken file in `/etc/sudoers.d/`.
3. Always use `visudo` to validate syntax in the future.

---

## Lab 38: LVM PV Missing, Data at Risk

**Scenario:**
- One physical volume in a VG is missing (e.g., disk unplugged), but you need to recover as much data as possible.

**Tasks:**
1. Diagnose the missing PV and recover data or restore from backup.

**Solution:**
1. Check LVM status:
   ```bash
   vgs
   pvs
   lvs
   ```
2. If PV is missing but VG is not critical, mount available LVs:
   ```bash
   vgchange -ay
   mount /mnt/data
   ```
3. If VG is critical, restore from LVM backup:
   ```bash
   vgcfgrestore <vgname>
   ```

---

## Lab 39: System Boots but Network Fails Due to Predictable Interface Names

**Scenario:**
- After a kernel/udev update, network interfaces are renamed (e.g., `enp0s3` instead of `eth0`) and networking fails.

**Tasks:**
1. Diagnose and fix network config to use new interface names.

**Solution:**
1. List interfaces:
   ```bash
   ip link
   ```
2. Update `/etc/sysconfig/network-scripts/ifcfg-*` or NetworkManager configs to match new names.
3. Restart network:
   ```bash
   nmcli con reload
   nmcli con up <new-interface>
   ```

---

## Lab 40: Container Networking Isolation

**Scenario:**
- Containers on the same host cannot communicate with each other over the default bridge network.

**Tasks:**
1. Diagnose and fix network isolation so containers can communicate.

**Solution:**
1. Check Podman network configuration:
   ```bash
   podman network ls
   podman network inspect podman
   ```
2. Ensure containers are on the same network:
   ```bash
   podman run --network podman --name c1 ...
   podman run --network podman --name c2 ...
   ```
3. If custom bridge needed, create and attach containers:
   ```bash
   podman network create mynet
   podman run --network mynet ...
   ```

---

## Lab 41: SELinux Blocks Samba Share

**Scenario:**
- A Samba share at `/srv/samba` is inaccessible from Windows clients, even though the service is running and the firewall is open.

**Tasks:**
1. Diagnose and fix SELinux so the share is accessible.

**Solution:**
1. Check SELinux booleans and context:
   ```bash
   getsebool -a | grep samba
   setsebool -P samba_export_all_rw 1
   restorecon -Rv /srv/samba
   ```
2. Ensure correct context:
   ```bash
   chcon -Rt samba_share_t /srv/samba
   ```

---

## Lab 42: Network Bonding Fails After Reboot

**Scenario:**
- A bonded interface (`bond0`) does not come up after reboot, causing network outage.

**Tasks:**
1. Diagnose and fix the bonding configuration so it persists and activates at boot.

**Solution:**
1. Check bonding config files:
   ```bash
   cat /etc/sysconfig/network-scripts/ifcfg-bond0
   cat /etc/sysconfig/network-scripts/ifcfg-eth*
   ```
2. Ensure `NM_CONTROLLED=yes` and bonding options are correct.
3. Reload NetworkManager:
   ```bash
   nmcli con reload
   nmcli con up bond0
   ```

---

## Lab 43: Systemd Service Restart Loop

**Scenario:**
- A custom systemd service keeps restarting rapidly and fills the logs.

**Tasks:**
1. Diagnose the cause and fix the service so it runs stably.

**Solution:**
1. Check service status and logs:
   ```bash
   systemctl status <service>
   journalctl -u <service>
   ```
2. Look for `Restart=always` or `RestartSec=0` in the unit file; increase `RestartSec` or fix the script/executable.
3. Reload systemd:
   ```bash
   systemctl daemon-reload
   systemctl restart <service>
   ```

---

## Lab 44: Forgotten Root Password Recovery (Modern RHCSA 9 Method)

> **Note:** This is the official, modern method required for RHCSA 9 exam and real-world RHEL 8/9 systems. The legacy single-user mode approach is no longer valid for the exam.

**Scenario:**
- You have lost the root password and need to reset it.

**Tasks:**
1. Recover root access and set a new password using the `rd.break` method.

**Solution:**
1. Reboot the system. At the GRUB menu, select your kernel and press `e` to edit.
2. Find the line starting with `linux` and append:
   ```
   rd.break
   ```
3. Press `Ctrl+X` to boot with the modified kernel line.
4. At the initramfs prompt, remount the sysroot as read/write:
   ```bash
   mount -o remount,rw /sysroot
   chroot /sysroot
   ```
5. Set a new root password:
   ```bash
   passwd root
   ```
6. (Recommended) Relabel SELinux to avoid login issues:
   ```bash
   touch /.autorelabel
   ```
7. Exit the chroot and reboot:
   ```bash
   exit
   reboot
   ```

**Reference:**
- This is the only method accepted on the RHCSA 9 exam. See also: Lab 1 if boot issues are related to /etc/fstab, and Lab 37 if sudo access is broken.

---

## Lab 45: Container Fails to Start Due to Missing Image

**Scenario:**
- A Podman container fails to start because the required image is missing locally and the host cannot reach external registries.

**Tasks:**
1. Import the image manually and start the container.

**Solution:**
1. Obtain the image tarball from another system:
   ```bash
   podman save -o myimage.tar <image>
   scp myimage.tar user@target:/tmp/
   ```
2. Import on the target system:
   ```bash
   podman load -i /tmp/myimage.tar
   podman run ...
   ```

---

## Lab 46: File System Corruption Detected on Boot

**Scenario:**
- System drops to emergency mode on boot due to file system corruption on `/dev/sdb1`.

**Tasks:**
1. Repair the file system and restore normal boot.

**Solution:**
1. At emergency shell, run:
   ```bash
   fsck -y /dev/sdb1
   reboot
   ```

---

## Lab 47: DNS Resolver Misconfiguration

**Scenario:**
- The system cannot resolve hostnames after a manual change to `/etc/resolv.conf`.

**Tasks:**
1. Diagnose and restore correct DNS resolver configuration.

**Solution:**
1. Check `/etc/resolv.conf`:
   ```bash
   cat /etc/resolv.conf
   ```
2. Restore correct content (e.g., `nameserver 8.8.8.8`).
3. If using NetworkManager, set DNS via nmcli:
   ```bash
   nmcli con mod <connection> ipv4.dns "8.8.8.8"
   nmcli con up <connection>
   ```

---

## Lab 48: User Cannot Change Password Due to Expired Account

**Scenario:**
- User `jane` cannot change her password because her account is expired.

**Tasks:**
1. Diagnose and unlock the user account.

**Solution:**
1. Check account status:
   ```bash
   chage -l jane
   ```
2. Unlock and reset expiration:
   ```bash
   chage -E -1 jane
   passwd jane
   ```

---

## Lab 49: Firewalld Blocks Cockpit Web Console

**Scenario:**
- Cockpit service is running, but you cannot access it on port 9090 from a remote machine.

**Tasks:**
1. Diagnose and fix firewalld settings so Cockpit is accessible.

**Solution:**
1. Open Cockpit port:
   ```bash
   firewall-cmd --add-service=cockpit --permanent
   firewall-cmd --reload
   ```
2. Test from remote:
   ```bash
   curl http://<host>:9090
   ```

---

## Lab 50: System Boots but Fails to Start Graphical Target

**Scenario:**
- System boots to multi-user (text) target instead of graphical, even though GUI is installed.

**Tasks:**
1. Diagnose and set the default target to graphical.

**Solution:**
1. Check current target:
   ```bash
   systemctl get-default
   ```
2. Set to graphical:
   ```bash
   systemctl set-default graphical.target
   systemctl isolate graphical.target
   ```
