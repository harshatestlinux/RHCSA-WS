# RHCSA Mock Exam 2 (2025) — With Answers and Explanations

---

## Section 1: User and Group Management

1. **Create a user `labuser` with no home directory and set their shell to `/sbin/nologin`.**

   **Answer:**
   ```bash
   useradd -M -s /sbin/nologin labuser
   ```
   **Explanation:**  
   `-M` prevents home directory creation, `-s` sets the login shell.

2. **Create a group `devops` and make `labuser` its primary group.**

   **Answer:**
   ```bash
   groupadd devops
   usermod -g devops labuser
   ```
   **Explanation:**  
   `-g` sets the primary group for the user.

3. **Set a password for `labuser` that must be changed at first login.**

   **Answer:**
   ```bash
   passwd labuser
   chage -d 0 labuser
   ```
   **Explanation:**  
   `chage -d 0` forces password change at next login.

---

## Section 2: File System and Storage

4. **Create a new logical volume `lvdata` of size 400MB in volume group `vgexam`. Format as ext4 and mount at `/mnt/lvdata` persistently.**

   **Answer:**
   ```bash
   lvcreate -L 400M -n lvdata vgexam
   mkfs.ext4 /dev/vgexam/lvdata
   mkdir -p /mnt/lvdata
   echo '/dev/vgexam/lvdata /mnt/lvdata ext4 defaults 0 0' >> /etc/fstab
   mount -a
   ```
   **Explanation:**  
   Logical volume management and persistent mounting.

5. **Resize `/mnt/lvdata` to 600MB (online, if possible).**

   **Answer:**
   ```bash
   lvresize -L 600M /dev/vgexam/lvdata
   resize2fs /dev/vgexam/lvdata
   ```
   **Explanation:**  
   `lvresize` changes LV size, `resize2fs` resizes ext4 filesystem.

---

## Section 3: Permissions and File Management

6. **Create a directory `/shared` owned by root, group-owned by `devops`, with permissions so only group members can create files, and files inherit the group.**

   **Answer:**
   ```bash
   mkdir /shared
   chown root:devops /shared
   chmod 2770 /shared
   ```
   **Explanation:**  
   `2770` sets setgid and proper permissions.

7. **Find and delete all empty files in `/tmp`.**

   **Answer:**
   ```bash
   find /tmp -type f -empty -delete
   ```
   **Explanation:**  
   `-empty` matches empty files, `-delete` removes them.

8. **Copy all `.conf` files from `/etc` to `/root/conf-backup/`, preserving attributes.**

   **Answer:**
   ```bash
   mkdir -p /root/conf-backup
   cp -a /etc/*.conf /root/conf-backup/
   ```
   **Explanation:**  
   `-a` preserves all attributes.

---

## Section 4: Networking and Security

9. **Set the hostname to `rhcsa-node2.example.com` and make it persistent.**

   **Answer:**
   ```bash
   hostnamectl set-hostname rhcsa-node2.example.com
   ```
   **Explanation:**  
   `hostnamectl` persists across reboots.

10. **Allow only SSH traffic in the firewall and block all others.**

    **Answer:**
    ```bash
    firewall-cmd --permanent --add-service=ssh
    firewall-cmd --permanent --remove-service=dhcpv6-client
    firewall-cmd --permanent --remove-service=cockpit
    firewall-cmd --permanent --remove-service=mdns
    firewall-cmd --permanent --remove-service=samba-client
    firewall-cmd --permanent --remove-service=dhcpv6-client
    firewall-cmd --permanent --remove-service=ftp
    firewall-cmd --permanent --remove-service=ntp
    firewall-cmd --permanent --remove-service=dns
    firewall-cmd --permanent --remove-service=dhcp
    firewall-cmd --reload
    ```
    **Explanation:**  
    Only SSH service is allowed; all other common services are removed.

11. **Change the default SSH port to 2222 and restart the SSH service.**

    **Answer:**
    ```bash
    sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
    systemctl restart sshd
    firewall-cmd --permanent --add-port=2222/tcp
    firewall-cmd --reload
    ```
    **Explanation:**  
    Changes config, restarts SSH, and opens the new port in the firewall.

---

## Section 5: System Services and Boot

12. **Set the system to boot into graphical target by default.**

    **Answer:**
    ```bash
    systemctl set-default graphical.target
    ```
    **Explanation:**  
    Sets system to boot into GUI.

13. **Schedule a reboot every Sunday at 1:30am using `cron`.**

    **Answer:**
    ```bash
    crontab -e
    # Add:
    30 1 * * 0 /sbin/shutdown -r now
    ```
    **Explanation:**  
    Cron syntax for weekly reboot.

14. **List all failed systemd services.**

    **Answer:**
    ```bash
    systemctl --failed
    ```
    **Explanation:**  
    Shows all services in a failed state.

---

## Section 6: Package Management and Containers

15. **Install the `vsftpd` package and ensure it starts on boot.**

    **Answer:**
    ```bash
    dnf install -y vsftpd
    systemctl enable --now vsftpd
    ```
    **Explanation:**  
    Installs and enables FTP service.

16. **List all installed packages with “http” in the name.**

    **Answer:**
    ```bash
    rpm -qa | grep http
    ```
    **Explanation:**  
    Lists packages matching pattern.

17. **Run a Podman container using the `centos` image and execute `cat /etc/os-release` inside it.**

    **Answer:**
    ```bash
    podman run --rm centos cat /etc/os-release
    ```
    **Explanation:**  
    Runs a temporary CentOS container and prints OS info.
