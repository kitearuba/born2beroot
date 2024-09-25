# 🛡️ **Born2BeRoot**

## 📝 Project Overview

**Born2BeRoot** is a system administration project from 42 School designed to introduce students to virtualization and server configuration. You will set up your first virtual machine following strict guidelines, using either VirtualBox or UTM. By the end of the project, you will be able to configure your own operating system with a set of stringent security rules.

## 📋 Table of Contents

1. [⚙️ Pre-requisites](#️-pre-requisites)
2. [🏗️ Project Structure](#️-project-structure)
3. [🔧 Mandatory Part](#️-mandatory-part)
4. [✨ Bonus Part](#️-bonus-part)
5. [🖥️ Scripts](#️-scripts)
6. [🚀 Project Delivery and Evaluation](#️-project-delivery-and-evaluation)
7. [📚 Resources](#️-resources)

## ⚙️ Pre-requisites

- 🖥️ **Virtualization Software**: VirtualBox or UTM (if VirtualBox doesn't work)
- 🛠️ **Operating System**: Latest stable version of **Debian** or **Rocky Linux**
- 📚 **Knowledge**: Basic understanding of Linux command line and system administration

## 🏗️ Project Structure

- **Virtual Machine**: Create and configure a virtual machine using VirtualBox or UTM.
- **System Configuration**: Follow strict rules to configure your server.
- **Security**: Implement security measures such as strong password policies, configuring `sudo` and `SSH`, and setting up a firewall.

## 🔧 Mandatory Part

1. **Operating System**:
   - Use the latest stable version of Debian or Rocky Linux.
   - No graphical interface allowed (No X.org or similar).

2. **Partitioning**:
   - Create at least 2 encrypted partitions using LVM.

3. **Security Policies**:
   - Implement a strong password policy:
     - Minimum length of 10 characters
     - Must contain a number and an uppercase letter
     - Password expires every 30 days
   - Configure `sudo` with:
     - Limited authentication attempts
     - Custom error message for incorrect passwords
     - Log all `sudo` commands
   - Install and configure the SSH service on port `4242` with restricted root access.

4. **Firewall**:
   - Configure `ufw` for Debian or `firewalld` for Rocky Linux, allowing only necessary ports.

5. **Hostname**:
   - Set the hostname to your login followed by "42" (e.g., `user42`).

6. **Monitoring Script**:
   - Create a script named `monitoring.sh` that displays system information every 10 minutes using the `wall` command. It should display:
     - OS architecture and kernel version
     - Number of physical and virtual cores
     - RAM usage and available disk space
     - CPU load
     - Last boot time
     - LVM status
     - Active connections and users
     - IP address and MAC address
     - Number of `sudo` commands executed

## ✨ Bonus Part

1. **Advanced Partitioning**:
   - Configure partitions to achieve a specified structure.

2. **Web Server**:
   - Set up a functional WordPress site using `lighttpd`, `MariaDB`, and `PHP`.

3. **Additional Service**:
   - Configure an additional useful service (excluding `NGINX`/`Apache2`). You must justify your choice during the defense.

## 🖥️ Scripts

### Monitoring Script Example
```bash
#!/bin/bash
# Simple monitoring script to display system information every 10 minutes

architecture=$(uname -a)
cpu_physical=$(grep "physical id" /proc/cpuinfo | wc -l)
cpu_virtual=$(grep "processor" /proc/cpuinfo | wc -l)
ram_usage=$(free --mega | awk '$1 == "Mem:" {printf "%.2f%%\n", $3/$2 * 100}')
disk_usage=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{total+=$3} END {print total}')
cpu_load=$(vmstat 1 2 | tail -1 | awk '{print $15}')
last_boot=$(who -b | awk '{print $3 " " $4}')
lvm_use=$(lsblk | grep -q lvm && echo "Yes" || echo "No")
tcp_connections=$(ss -ta | grep ESTAB | wc -l)
user_log=$(who | awk '{print $1}' | sort | uniq | wc -l)
ip=$(hostname -I)
mac=$(ip link show | grep link/ether | awk '{print $2}')
sudo_commands=$(grep -c "COMMAND" /var/log/sudo/sudo.log)

echo "#Architecture: $architecture"
echo "#CPU physical: $cpu_physical"
echo "#vCPU: $cpu_virtual"
echo "#Memory Usage: $ram_usage"
echo "#Disk Usage: $disk_usage"
echo "#CPU load: $cpu_load"
echo "#Last boot: $last_boot"
echo "#LVM use: $lvm_use"
echo "#TCP Connections: $tcp_connections"
echo "#User log: $user_log"
echo "#Network: IP $ip ($mac)"
echo "#Sudo : $sudo_commands cmd"
```

## 🚀 Project Delivery and Evaluation

- 📁 **Submit**: Only a `signature.txt` file in the root of your repository.
- 📝 **Content**: The file should contain the SHA-1 signature of your virtual disk.
- ⚠️ **Important**: Ensure your VM is configured correctly as it will be evaluated based on the criteria listed above.

## 📚 Resources

- 📘 [Debian Documentation](https://www.debian.org/doc/)
- 📙 [Rocky Linux Documentation](https://docs.rockylinux.org/)
- 📗 [LVM Guide](https://www.tldp.org/HOWTO/LVM-HOWTO/)
- 📕 [UFW Documentation](https://help.ubuntu.com/community/UFW)

---

Feel free to contribute to this project by submitting issues or pull requests! 😊

🛡️ **Born2BeRoot** - © 42 School, 2024
