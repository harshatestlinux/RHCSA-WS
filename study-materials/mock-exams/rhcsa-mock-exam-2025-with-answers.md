# RHCSA Mock Exam (2025 Style) — With Answers and Explanations

---

## Section 1: System and User Management

1. **Create a new user called `examuser` with the home directory `/home/examuser` and set the password to `RedHat2025!`.**

   **Answer:**
   ```bash
   useradd -m -d /home/examuser examuser
   echo "RedHat2025!" | passwd --stdin examuser
   ```
   **Explanation:**  
   `useradd -m -d /home/examuser` creates a user with a specified home directory and creates the directory if it doesn’t exist.  
   The `echo ... | passwd --stdin` command sets the password non-interactively (useful for scripting or labs).

2. **Create a group called `projectgrp` and add `examuser` to this group.**

   **Answer:**
   ```bash
   groupadd projectgrp
   usermod -aG projectgrp examuser
   ```
   **Explanation:**  
   `groupadd` creates a new group.  
   `usermod -aG` appends the user to the group without removing them from other groups.

3. **Set the password for `examuser` to expire in 30 days.**

   **Answer:**
   ```bash
   chage -M 30 examuser
   ```
   **Explanation:**  
   `chage -M 30` sets the maximum number of days before a password must be changed.

4. **Lock the `examuser` account and then unlock it.**

   **Answer:**
   ```bash
   passwd -l examuser   # Lock
   passwd -u examuser   # Unlock
   ```
   **Explanation:**  
   `passwd -l` disables the account by locking the password.  
   `passwd -u` unlocks it.

---

## Section 2: File System and Storage

5. **Create a new 300MB partition on the second disk (`/dev/sdb`) and format it as XFS. Mount it persistently at `/mnt/data`.**

   **Answer:**
   ```bash
   fdisk /dev/sdb
   # (n, p, 1, +300M, w)
   mkfs.xfs /dev/sdb1
   mkdir -p /mnt/data
   echo '/dev/sdb1 /mnt/data xfs defaults 0 0' >> /etc/fstab
   mount -a
   ```
   **Explanation:**  
   `fdisk` is used to create a partition interactively.  
   `mkfs.xfs` formats the new partition as XFS.  
   Adding the mount entry to `/etc/fstab` ensures it mounts at boot.  
   `mount -a` applies all entries in `/etc/fstab` immediately.

6. **Set permissions so only members of `projectgrp` can write to `/mnt/data`, but all users can read.**

   **Answer:**
   ```bash
   chown root:projectgrp /mnt/data
   chmod 2775 /mnt/data
   ```
   **Explanation:**  
   `chown` sets the group owner.  
   `chmod 2775` sets read/write for owner/group, read for others, and the setgid bit so new files inherit the group.

7. **Create a 1GB swap file at `/swapfile1` and enable it permanently.**

   **Answer:**
   ```bash
   dd if=/dev/zero of=/swapfile1 bs=1M count=1024
   chmod 600 /swapfile1
   mkswap /swapfile1
   swapon /swapfile1
   echo '/swapfile1 swap swap defaults 0 0' >> /etc/fstab
   ```
   **Explanation:**  
   `dd` creates a 1GB file.  
   `chmod 600` secures it.  
   `mkswap` and `swapon` set up and activate the swap file.  
   Adding to `/etc/fstab` ensures it’s enabled after reboot.

---

## Section 3: File Management

8. **Find all files in `/var/log` larger than 10MB and save the list to `/root/large-logs.txt`.**

   **Answer:**
   ```bash
   find /var/log -type f -size +10M > /root/large-logs.txt
   ```
   **Explanation:**  
   `find` searches for files (`-type f`) larger than 10MB (`-size +10M`). Output is redirected to the specified file.

9. **Create a compressed tar archive of `/etc` named `/root/etc-backup.tar.gz`.**

   **Answer:**
   ```bash
   tar czvf /root/etc-backup.tar.gz /etc
   ```
   **Explanation:**  
   `tar c` creates an archive, `z` compresses with gzip, `v` is verbose, `f` specifies the filename.

10. **Set up a cron job for `examuser` to run `date >> /tmp/date.log` every day at 6am.**

    **Answer:**
    ```bash
    crontab -u examuser -e
    # Add this line:
    0 6 * * * date >> /tmp/date.log
    ```
    **Explanation:**  
    The cron line schedules the command daily at 6am.  
    `crontab -u examuser -e` edits the crontab for `examuser`.

---

## Section 4: Networking and Security

11. **Configure a static IP address (`192.168.100.50/24`, gateway `192.168.100.1`, DNS `8.8.8.8`) on interface `eth1`.**

    **Answer:**
    ```bash
    nmcli con mod eth1 ipv4.addresses 192.168.100.50/24
    nmcli con mod eth1 ipv4.gateway 192.168.100.1
    nmcli con mod eth1 ipv4.dns 8.8.8.8
    nmcli con mod eth1 ipv4.method manual
    nmcli con up eth1
    ```
    **Explanation:**  
    `nmcli` commands configure the network interface, gateway, and DNS, then activate the connection.

12. **Open port 8080/tcp in the firewall and reload the rules.**

    **Answer:**
    ```bash
    firewall-cmd --permanent --add-port=8080/tcp
    firewall-cmd --reload
    ```
    **Explanation:**  
    `firewall-cmd --permanent` adds the rule persistently.  
    `--reload` applies the new rules.

13. **Set SELinux to enforcing mode and verify the status.**

    **Answer:**
    ```bash
    setenforce 1
    sed -i 's/^SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config
    getenforce
    ```
    **Explanation:**  
    `setenforce 1` changes the mode immediately.  
    Editing `/etc/selinux/config` makes it persistent.  
    `getenforce` checks current status.

14. **Configure SSH to allow only key-based authentication (disable password login).**

    **Answer:**
    ```bash
    sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
    systemctl reload sshd
    # Ensure keys are in ~/.ssh/authorized_keys for users
    ```
    **Explanation:**  
    Editing `sshd_config` disables password logins.  
    Reloading the service applies changes.  
    Users must have public keys in `~/.ssh/authorized_keys` to log in.

---

## Section 5: Package Management and System Boot

15. **Install the `httpd` web server, enable and start the service.**

    **Answer:**
    ```bash
    dnf install -y httpd
    systemctl enable --now httpd
    ```
    **Explanation:**  
    `dnf install` gets the package.  
    `systemctl enable --now` starts the service immediately and at boot.

16. **Set the default boot target to multi-user (non-graphical).**

    **Answer:**
    ```bash
    systemctl set-default multi-user.target
    ```
    **Explanation:**  
    This sets the system to boot into a text-based, multi-user environment by default.

17. **List all enabled repositories and save to `/root/repos.txt`.**

    **Answer:**
    ```bash
    dnf repolist enabled > /root/repos.txt
    ```
    **Explanation:**  
    Lists all currently enabled repositories and saves the output.

18. **Find and remove any orphaned packages.**

    **Answer:**
    ```bash
    dnf repoquery --unneeded
    dnf remove $(dnf repoquery --unneeded)
    ```
    **Explanation:**  
    The first command lists orphaned (no longer needed) packages.  
    The second removes them.

---

## Section 6: Containers (Bonus)

19. **Install Podman and run an `alpine` container that prints “Hello RHCSA” and exits.**

    **Answer:**
    ```bash
    dnf install -y podman
    podman run --rm alpine echo "Hello RHCSA"
    ```
    **Explanation:**  
    Installs Podman, then runs a temporary Alpine Linux container that prints the message and exits.

20. **Create a persistent volume for containers at `/var/lib/containers/storage` and ensure it is mounted at boot.**

    **Answer:**
    ```bash
    mkdir -p /var/lib/containers/storage
    # Example: add to /etc/fstab if on a separate partition or disk
    # /dev/sdc1 /var/lib/containers/storage xfs defaults 0 0
    mount -a
    ```
    **Explanation:**  
    The directory is created for container storage.  
    If using a dedicated partition, add the mount to `/etc/fstab` for persistence.  
    `mount -a` applies all mounts.
