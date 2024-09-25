Certainly! I’ll integrate the relevant and useful code snippets from your notes into the README for the **Born2BeRoot** project, specifically within the **Scripts** section where the `monitoring.sh` script is described. This will provide a more comprehensive guide on system monitoring and command usage.

---

# 🛡️ **Born2BeRoot**

## 📝 Project Overview

**Born2BeRoot** is a system administration project from 42 School designed to introduce students to virtualization and server configuration. You will set up your first virtual machine following strict guidelines, using either VirtualBox or UTM. By the end of the project, you will be able to configure your own operating system with a set of stringent security rules.

## 📋 Table of Contents

1. [⚙️ Pre-requisites](#⚙️-pre-requisites)
2. [🏗️ Project Structure](#🏗️-project-structure)
3. [🔧 Mandatory Part](#🔧-mandatory-part)
4. [✨ Bonus Part](#✨-bonus-part)
5. [🚀 Approach and Method](#🚀-approach-and-method)
6. [🖥️ Scripts](#🖥️-scripts)
7. [🚀 Project Delivery and Evaluation](#🚀-project-delivery-and-evaluation)
8. [📚 Resources](#📚-resources)
9. [🎉 Conclusion](#🎉-conclusion)
10. [👨‍💻 Author](#👨‍💻-author)

## ⚙️ Pre-requisites

- 🖥️ **Virtualization Software**: VirtualBox or UTM (if VirtualBox doesn't work)
- 🛠️ **Operating System**: Latest stable version of **Debian** or **Rocky Linux**
- 📚 **Knowledge**: Basic understanding of Linux command line and system administration

## 🏗️ Project Structure

The project is organized as follows:

```text
📦 Born2BeRoot
├── 📄 signature.txt        # SHA-1 signature of the virtual disk
├── 📁 config
│   └── 📄 setup.sh          # Configuration script for server setup
├── 📁 scripts
│   └── 📄 monitoring.sh     # Monitoring script for system information
└── 📄 README.md            # Project documentation
```

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
     - Minimum length of 10 characters.
     - Must contain a number and an uppercase letter.
     - Password expires every 30 days.
   - Configure `sudo` with:
     - Limited authentication attempts.
     - Custom error message for incorrect passwords.
     - Log all `sudo` commands.
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

## 🚀 Approach and Method

This project emphasizes practical experience in system administration by requiring strict adherence to security policies and configuration rules. The method involves:

1. **Planning**: Before setting up the virtual machine, create a plan for the partitioning and security configurations.
2. **Configuration**: Follow a step-by-step approach to configure the operating system, firewall, and security policies.
3. **Automation**: Utilize shell scripts (`setup.sh`, `monitoring.sh`) to automate repetitive tasks and ensure consistency.
4. **Testing**: Test each configuration step, especially the firewall and SSH setup, to ensure the server is secure and follows the project requirements.
5. **Documentation**: Document each step and configuration change in the project README and `setup.sh` script to facilitate review and evaluation.

## 🖥️ Scripts

### Monitoring Script: `monitoring.sh`

The `monitoring.sh` script provides essential system information at regular intervals using the `wall` command. This script will help monitor the system's performance and status.

#### Script Overview

```bash
#!/bin/bash
# Simple monitoring script to display system information every 10 minutes

# Get system architecture and kernel version
architecture=$(uname -a)

# Count physical and virtual CPUs
cpu_physical=$(grep "physical id" /proc/cpuinfo | wc -l)
cpu_virtual=$(grep "processor" /proc/cpuinfo | wc -l)

# Get RAM usage details
ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')
ram_used=$(free --mega | awk '$1 == "Mem:" {print $3}')
ram_usage=$(free --mega | awk '$1 == "Mem:" {printf "%.2f%%", $3/$2*100}')

# Disk usage excluding /boot
disk_usage=$(df -m --output=source,used | awk '/^\/dev\// && !/boot/ {total += $2} END {printf "%.2fGB", total/1024}')

# Current CPU load percentage
cpu_load=$(vmstat 1 2 | tail -1 | awk '{printf "%.2f%%", 100-$15}')

# Last boot time
last_boot=$(who -b | awk '$1 == "system" {print $3 " " $4}')

# Check if LVM is active
lvm_status=$(lsblk | grep -q lvm && echo "Yes" || echo "No")

# Active TCP connections
tcp_connections=$(ss -ta | awk '$1 ~ /ESTAB/ {total += 1} END {print total}')

# Unique logged-in users count
user_log=$(who | awk '{print $1}' | sort | uniq | wc -l)

# IP and MAC address
ip=$(hostname -I)
mac=$(ip link show | grep link/ether | awk '{print $2}')

# Count of sudo commands executed
sudo_commands=$(grep -c "COMMAND" /var/log/sudo/sudo.log)

# Display information using wall
wall "
#Architecture: $architecture
#CPU physical: $cpu_physical
#vCPU: $cpu_virtual
#Memory Usage: $ram_used/$ram_total MB ($ram_usage)
#Disk Usage: $disk_usage
#CPU Load: $cpu_load
#Last Boot: $last_boot
#LVM Active: $lvm_status
#TCP Connections: $tcp_connections
#Logged-in Users: $user_log
#Network: IP $ip (MAC $mac)
#Sudo Commands: $sudo_commands cmd
"
```

### Explanation of Key Commands

- **System Information**:
  - `uname -a`: Displays the architecture and kernel version of the operating system.
  - `grep "physical id" /proc/cpuinfo | wc -l`: Counts the number of physical CPUs.
  - `grep "processor" /proc/cpuinfo | wc -l`: Counts the number of virtual CPUs.
  
- **Memory and Disk Usage**:
  - `free --mega`: Provides RAM usage in megabytes.
  - `df -m --output=source,used`: Displays disk usage in megabytes.

- **CPU Load**:
  - `vmstat 1 2 | tail -1 | awk '{printf "%.2f%%", 100-$15}'`: Calculates the current CPU load percentage.

- **LVM Status**:
  - `lsblk | grep -q lvm && echo "Yes" || echo "No"`: Checks if LVM is active.

- **Active Connections**:
  - `ss -ta | awk '$1 ~ /ESTAB/ {total += 1} END {print total}'`: Counts the number of active TCP connections.

- **Sudo Command Count**:
  - `grep -c "COMMAND" /var/log/sudo/sudo.log`: Counts the number of sudo commands executed.

## 🚀 Project Delivery and Evaluation

- 📁 **Submit**: Only a `signature.txt` file in the root of your repository.
- 📝 **Content**: The file should contain the SHA-1 signature of your virtual disk.
- ⚠️ **Important**: Ensure your VM is configured correctly as it will be evaluated based on the

 criteria listed above.

## 📚 Resources

- 📘 [Debian Documentation](https://www.debian.org/doc/)
- 📙 [Rocky Linux Documentation](https://docs.rockylinux.org/)
- 📗 [LVM Guide](https://www.tldp.org/HOWTO/LVM-HOWTO/)
- 📕 [UFW Documentation](https://help.ubuntu.com/community/UFW)

## 🎉 Conclusion

The **Born2BeRoot** project is an excellent opportunity to deepen your understanding of system administration, virtualization, and security. By configuring your own server from scratch and adhering to strict security policies, you will gain invaluable hands-on experience that is essential for any aspiring system administrator. Completing this project will not only equip you with practical skills but also prepare you for more advanced topics in system and network administration.

Good luck, and happy coding! 🚀


## 👨‍💻 **Author**

**chrrodri**  
_42 Barcelona_

[GitHub Profile](https://github.com/kitearuba)


Feel free to reach out for collaboration or any questions regarding this project! 😊

---

🛡️ **Born2BeRoot** - © 42 School, 2024

---
