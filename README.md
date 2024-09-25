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

### Monitoring Script Example

The `monitoring.sh` script provides essential system information at regular intervals.

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

### Summary of Enhancements:
1. **Conclusion**: Added a section summarizing the project’s benefits and skills gained.
2. **Author Section**: Included a dedicated section for author information and contact.
3. **Consistent Layout**: Maintained a clear, structured format throughout the README.

This README now has a comprehensive and professional feel, similar to the `ft_printf` style, while being tailored to the **Born2BeRoot** project. Feel free to modify the author details and personalize the content as needed!
- 📘 [Debian Documentation](https://www.debian.org/doc/)
- 📙 [Rocky Linux Documentation](https://docs.rockylinux.org/)
- 📗 [LVM Guide](https://www.tldp.org/HOWTO/LVM-HOWTO/)
- 📕 [UFW Documentation](https://help.ubuntu.com/community/UFW)

## 🎉 Conclusion

The **Born2BeRoot** project is an excellent opportunity to deepen your understanding of system administration, virtualization, and security. By configuring your own server from scratch and adhering to strict security policies, you will gain invaluable hands-on experience that is essential for any aspiring system administrator. Completing this project will not only equip you with practical skills but also prepare you for more advanced topics in system and network administration.

Good luck, and happy coding! 🚀

## 👨‍💻 Author

- **Author**: [Your Name]
- **42 School**: Your Campus (e.g., 42 Barcelona)
- **GitHub Profile**: [Your GitHub Profile Link]

Feel free to reach out for collaboration or any questions regarding this project! 😊

---

🛡️ **Born2BeRoot** - © 42 School, 2024

---
