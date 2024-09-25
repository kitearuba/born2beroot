
---

# 📘 **Born2BeRoot Project Notes**

## 🛠️ **System Setup and Configuration**

### 🔹 User Management

- **Create a User**:
  ```bash
  sudo adduser <username>
  ```

- **Add User to Sudo Group**:
  ```bash
  sudo usermod -aG sudo <username>
  ```

- **Change Username**:
  ```bash
  sudo usermod -l <new_name> <old_name>
  ```

- **Change User Home Directory**:
  ```bash
  sudo usermod -d /home/<new_folder_name> -m <username>
  ```

- **Delete User**:
  ```bash
  sudo deluser <username>
  ```

- **Switch Users**:
  ```bash
  su - <username>
  ```

- **Kill User Processes**:
  ```bash
  sudo pkill -KILL -u <username>
  ```

- **Check if User Exists**:
  ```bash
  id <username>
  ```

- **Check User Processes**:
  ```bash
  ps -u <username>
  ```

- **Check User Groups**:
  ```bash
  getent group <group_name>
  # or
  cat /etc/group
  ```

### 🔹 Hostname Configuration

- **Check Current Hostname**:
  ```bash
  hostnamectl
  ```

- **Set New Hostname**:
  ```bash
  sudo hostnamectl set-hostname <new_hostname>
  ```

- **Update Hostname in Configuration Files**:
  ```bash
  sudo nano /etc/hosts
  sudo nano /etc/hostname
  ```

- **Restart Hostname Service**:
  ```bash
  sudo reboot
  # or
  sudo systemctl restart systemd-hostnamed
  ```

### 🔹 SSH Configuration

- **Update System**:
  ```bash
  sudo apt update
  ```

- **Install SSH Server**:
  ```bash
  sudo apt install openssh-server
  ```

- **Check SSH Status**:
  ```bash
  sudo service ssh status
  ```

- **Edit SSH Configuration**:
  ```bash
  sudo nano /etc/ssh/sshd_config
  sudo nano /etc/ssh/ssh_config
  ```

- **Restart SSH Service**:
  ```bash
  sudo service ssh restart
  ```

- **Connect via SSH**:
  ```bash
  ssh <username>@localhost -p 4242
  ```

## 🔒 **Security Configuration**

### 🔹 Firewall (UFW) Setup

- **Install UFW**:
  ```bash
  sudo apt install ufw
  ```

- **Enable Firewall**:
  ```bash
  sudo ufw enable
  ```

- **Allow Port 4242**:
  ```bash
  sudo ufw allow 4242
  ```

- **Check Firewall Status**:
  ```bash
  sudo ufw status
  ```

### 🔹 Sudo Password Policy

- **Create Custom Sudo Configuration File**:
  ```bash
  sudo touch /etc/sudoers.d/<file_name>
  ```

- **Set Password Policy in File**:
  ```bash
  Defaults passwd_tries=3
  Defaults badpass_message="Custom error message"
  Defaults logfile="/var/log/sudo/sudo_config"
  Defaults log_input, log_output
  Defaults iolog_dir="/var/log/sudo"
  Defaults requiretty
  Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
  ```

### 🔹 Password Policy for Users

1. **Password must meet these criteria**:
    - Minimum of 10 characters.
    - Must contain at least one uppercase letter and one number.
    - Cannot contain the username.
    - Cannot have more than 3 consecutive identical characters.

2. **Password Expiration**:
    - Password expires every 30 days.
    - Minimum of 2 days before the password can be changed again.
    - Warning message 7 days before expiration.

3. **Additional Rules for Root Password**:
    - Must have at least 7 characters that are not part of the old password.
    - The root password should follow the same policies as above.

### 🔹 Additional Sudo Policies

1. **Limited Authentication Attempts**:
   ```bash
   Defaults passwd_tries=3
   ```

2. **Custom Error Message**:
   ```bash
   Defaults badpass_message="Incorrect password, try again."
   ```

3. **Log Sudo Commands**:
   ```bash
   Defaults logfile="/var/log/sudo/sudo_config"
   Defaults log_input, log_output
   Defaults iolog_dir="/var/log/sudo"
   ```

4. **Restrict Sudo Usable Directories**:
   ```bash
   Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
   ```

5. **Enable TTY Mode for Security**:
   ```bash
   Defaults requiretty
   ```

## 📊 **System Monitoring and Information**

### 🔹 System Information Commands

- **Check System Architecture and Kernel Version**:
  ```bash
  uname -a
  ```

- **Check Number of Physical Processors**:
  ```bash
  grep "physical id" /proc/cpuinfo | wc -l
  ```

- **Check Number of Virtual Processors**:
  ```bash
  grep "processor" /proc/cpuinfo | wc -l
  ```

- **Check RAM Usage**:
  ```bash
  free --mega
  ```

- **Check Last Boot Time**:
  ```bash
  who -b
  ```

### 🔹 Disk and CPU Usage Monitoring

- **Monitor Disk Usage with `df` and `awk`**:
  ```bash
  df -m | grep "/dev/" | grep -v "/boot/" | awk '{total += $3} END {print total}'
  df -m --output=source,used | awk '/^\/dev\// && !/boot/ {total += $2} END {print total}'
  df -m | awk '$1 ~ /^\/dev\// && $6 !~ /^\/boot\// {total += $2 ; use_m += $3} END {printf ("%.2f%%\n", use_m/total*100)}'
  ```

- **Monitor CPU Usage**:
  ```bash
  vmstat 1 2 | tail -1 | awk '{print $15}'
  vmstat 1 2 | awk 'NR == 4 {print $15}'
  vmstat 1 2 | awk 'NR == 4 {printf ("%.2f%%\n", 100-$15)}'
  ```

- **Check Active Connections**:
  ```bash
  ss -ta | awk '$1 ~ /ESTAB/ {total += 1} END {print total}'
  ```

### 🔹 LVM and User Monitoring

- **Check LVM Status**:
  ```bash
  lsblk | awk '$6 ~/lvm/ {found = 1} END {if(found) {print "Yes"} else {print "No"}}'
  ```

- **Count Unique Logged-in Users**:
  ```bash
  who | awk '{print $1}' | sort | uniq | wc -l
  ```

## 📋 **Project Requirements and To-Do**

1. **Create `signature.txt`**: Add this file to the root of your repository.
2. **Encrypted Partitions**: Create at least 2 encrypted partitions using LVM.
3. **SSH Configuration**: Ensure SSH is running on port 4242 and root login is disabled.
4. **Firewall Configuration**: UFW must be active, and only necessary ports should be open.
5. **Hostname**: Set hostname to your login followed by "42" (e.g., `wil42`).
6. **User Creation**: Besides root, a user with your login name should exist and belong to `user42` and `sudo` groups.
7. **Password Policy**: Implement a strong password policy as outlined above.
8. **Monitoring Script**: Create a `monitoring.sh` script that displays key system information every 10 minutes using `wall`.

## 🎯 **Bonus Objectives**

1. **Advanced Partitioning**: Configure partitions to achieve the required structure.
2. **Web Server Setup**: Configure a functional WordPress site using `lighttpd`, `MariaDB`, and `PHP`.
3. **Additional Service**: Configure another useful service and justify your choice during the defense.
4. **Custom Services**: Add more services if necessary, and adapt UFW/Rocky rules as needed.

---

