# RHCSA Mock Exam 3 (2025) — With Answers and Explanations

---

## Section 1: User, Group, and Permissions

1. **Create a user `backup` with UID 2025 and GID 2025.**

   **Answer:**
   ```bash
   groupadd -g 2025 backup
   useradd -u 2025 -g 2025 backup
   ```
   **Explanation:**  
   Explicitly sets UID/GID.

2. **Set up a passwordless sudo configuration for the `backup` user.**

   **Answer:**
   ```bash
   echo 'backup ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/backup
   chmod 440 /etc/sudoers.d/backup
   ```
   **Explanation:**  
   Adds a sudoers file for passwordless access.

3. **Create a directory `/srv/backup` owned by `backup` with permissions 700.**

   **Answer:**
   ```bash
   mkdir -p /srv/backup
   chown backup:backup /srv/backup
   chmod 700 /srv/backup
   ```
   **Explanation:**  
   Owner-only access.

---

## Section 2: Storage and Filesystems

4. **Add a new ext4 filesystem on `/dev/sdc1` and mount it at `/mnt/archive` at boot.**

   **Answer:**
   ```bash
   mkfs.ext4 /dev/sdc1
   mkdir -p /mnt/archive
   echo '/dev/sdc1 /mnt/archive ext4 defaults 0 0' >> /etc/fstab
   mount -a
   ```
   **Explanation:**  
   Formats and persistently mounts the disk.

5. **Set a quota of 200MB for user `backup` on `/mnt/archive`.**

   **Answer:**
   ```bash
   mount -o remount,usrquota /mnt/archive
   edquota -u backup
   # Set soft/hard blocks to 200000
   ```
   **Explanation:**  
   Enables and configures disk quota.

---

## Section 3: File Management and Scheduling

6. **Find all files in `/home` not accessed in 90 days and archive them to `/root/old-home.tar.gz`.**

   **Answer:**
   ```bash
   find /home -type f -atime +90 -print0 | tar czvf /root/old-home.tar.gz --null -T -
   ```
   **Explanation:**  
   Archives old files using `find` and `tar`.

7. **Create a systemd timer to run `/usr/local/bin/cleanup.sh` daily at 3:15am.**

   **Answer:**
   ```bash
   # /etc/systemd/system/cleanup.service
   [Unit]
   Description=Daily Cleanup

   [Service]
   Type=oneshot
   ExecStart=/usr/local/bin/cleanup.sh

   # /etc/systemd/system/cleanup.timer
   [Unit]
   Description=Run Cleanup Daily

   [Timer]
   OnCalendar=*-*-* 03:15:00
   Persistent=true

   [Install]
   WantedBy=timers.target

   # Enable timer
   systemctl daemon-reload
   systemctl enable --now cleanup.timer
   ```
   **Explanation:**  
   Uses systemd timer for scheduled tasks.

---

## Section 4: Networking and Security

8. **Configure `/etc/hosts` to resolve `web01` to `10.10.10.10`.**

   **Answer:**
   ```bash
   echo '10.10.10.10 web01' >> /etc/hosts
   ```
   **Explanation:**  
   Adds static host mapping.

9. **Set SELinux to permissive mode immediately and persistently.**

   **Answer:**
   ```bash
   setenforce 0
   sed -i 's/^SELINUX=.*/SELINUX=permissive/' /etc/selinux/config
   ```
   **Explanation:**  
   Immediate and persistent change.

10. **Block all incoming traffic except HTTP and HTTPS using firewalld.**

    **Answer:**
    ```bash
    firewall-cmd --permanent --remove-service=ssh
    firewall-cmd --permanent --add-service=http
    firewall-cmd --permanent --add-service=https
    firewall-cmd --reload
    ```
    **Explanation:**  
    Only HTTP/HTTPS allowed.

---

## Section 5: Package Management and Containers

11. **Install `chrony` and ensure it starts at boot.**

    **Answer:**
    ```bash
    dnf install -y chrony
    systemctl enable --now chronyd
    ```
    **Explanation:**  
    Installs and enables NTP service.

12. **Show all files installed by the `bash` package.**

    **Answer:**
    ```bash
    rpm -ql bash
    ```
    **Explanation:**  
    Lists package contents.

13. **Run a Podman container in the background using the `nginx` image, mapping port 8081 on the host to port 80 in the container.**

    **Answer:**
    ```bash
    podman run -d -p 8081:80 nginx
    ```
    **Explanation:**  
    Background container, port mapping.
