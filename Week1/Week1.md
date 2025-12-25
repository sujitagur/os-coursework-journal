# Phase 1: System Planning and Distribution Selection (Week 1)

  This week, I will focus on building a reliable virtualised environment for the rest of the coursework. I have set up two virtual machines inside Oracle VirtualBox on my Windows host system:  

•	Workstation VM: Ubuntu Desktop 24.04

•	Server VM: Ubuntu Server 24.04

Both VMs are connected using a dual-adapter configuration:

•	Adapter 1- NAT: provide internet access for installation and updates.

•	Adapter 2- Host-Only Network: provides a private, isolated internal network that exists only between the workstation and the server.

## Part A - System Architecture Diagram
![Architecture Diagram](./w1-architecture1.png)

The system architecture follows a dual-system administration model containing a Linux workstation and a headless Linux server. The server does not expose a graphical interface and is managed exclusively via SSH.

Here, both virtual machines are connected via two network interfaces: a Host-Only adapter provides an isolated management network, while a NAT adapter enables outbound internet access. To ensure that the server is inaccessible from the external network, SSH communication occurs strictly over the Host-Only interface.

This setup is similar to how real cloud servers are managed, where servers are accessed through a private network rather than directly from the internet. It also ensures that all server administration occurs remotely. 

---

## Part B - Distribution Selection Justification:

Linux Distribution Family Comparison:

|Family         |Example | Strengths |   weakness| Reason Not Selected |
|---------------|--------|-----------|-----------|---------------------|
|Debian Family   |Debian , Ubuntu|High stability, strong community |Debian packages are often outdated |Ubuntu offer stability with newer kernels and tooling|
|Red Hat Family |	RHEL, CentOS stream|	Enterprise-grade tools, SELinux	|CentOS Stream changes too frequently which reduce predictability |Complexity for a contolled VM-based coursework|
|Arch Family	|Arch, Manjiro 	|Very flexible and up-to-date pacakages 	|Requires a lot of manual setup, high maintenance |Not Ideal security testing due to risk of instability|
|SUSE Family	|OpenSUSE, SLES|	Powerful admin tools 	|More complex for beginners |Limited benefit relatives to coursework requirements|

**WHY I CHOSE UBUNTU SERVER 24.04.LTS**

I picked Ubuntu Server 24.04 LTS  due to its balance of stability, security and up-to-date features. The LTS provides five years of security updates, which is essential for maintaining a consistent security baseline throughout the coursework.

Ubuntu also includes AppArmor enabled by default, directly supports the mandatory access control required in later phases without additional configuration complexity. Furthermore, Ubuntu have strong documentation and industry adoption in cloud platforms such as AWS and Azure, ensuring that configuration practices used in this coursework are professional standards.

  Even though other distributions offer stronger enterprise tooling, they come with unnecessary complexity for a controlled virtualised environment, which is required for this coursework and also provide zero benefits.

---

## PART C: Workstation Configuration decision:
 
  I have selected Ubuntu Desktop 24.04 as my workstation environment to ensure native compatibility with Linux administration tools such as SSH, system monitoring utilities and Bash scripting. Using a Linux-based workstation minimises command inconsistencies and makes troubleshooting easier.
  
  The additional CPU and memory overhead of operating a desktop virtual machine is a known trade-off of this strategy. However, this cost is reasonable as the  workstation VM is used exclusively for administration and monitoring, and the host system provides sufficient resources.

  Using a Linux workstation also reduces the risk of accidental configuration changes on the Windows host system and ensures consistent behaviour when developing and executing scripts that will later run against the server.  
  
---

## Part D: Network Configuration Documentation:
  In this section, I will explain how both VMs are connected inside VirtualBox using NAT and Host-Only networking, providing screenshots to illustrate the process.

**D.1 VirtualBox Network Adapters – Workstation VM**

Adapter 1 — NAT (Internet Access)

The first adapter is set to NAT, which provides internet access through the host machine to download, security updates, and package installation.

![Adapter 1 (NAT)](./w1-adapter1.png)

Adapter 2 — Host-Only Network (Private SSH Network)

The second network adapter is set to Host-Only, which connects the workstation directly to the server VM inside and isolates the VirtualBox subnet.

![Adapter 2 (Host-Only)](./w1-adapter2.png)

**D.2 VirtualBox Network Adapters – Server VM**

Adapter 1: NAT

The first network adapter is a NAT similar to that in a workstation, and it is for external package updates.

![Adapter 3 (Host-Only)](./w1-adapter3.png)

Adapter 2: Host-Only

The second network adapter is set to Host-Only, for private SSH access for the workstation. This ensures the server is isolated and cannot be accessed from outside the host-only network.

![Adapter 4 (Host-Only)](./w1-adapter4.png)


**D.3 Host-Only Network Configuration(vboxnet0)**

Adapter Settings:

![Adapter S (Host-Only)](./w1-adapterS.png)

This shows the Host-Only network adapter(vboxnet0), IPv4 address 192.168.56.1 and IPv4 Network Mask 255.255.255.0.

This address ensures that all VMs are connected to the Host-Only network.

DHCP Server settings:

![DHCP 1 (Host-Only)](./w1-DHCP1.png)

•	The DHCP Server for vboxnet0 is enabled and addresses between 192.168.56.101 -192.168.56.254.

•	My server VM (192.168.56.102) and workstation VM (192.168.56.103) both use Ips inside this range.

•	This ensures reliable, private, isolated communication via SSH for testing.

Overall, this network configuration enforces a strict separation between external connectivity and administrative access. By ensuring that SSH traffic is confined to the Host-Only network, the server is protected from unintended exposure while still retaining controlled internet access for updates. This design directly supports later firewall hardening and intrusion prevention tasks, where access will be restricted to a single trusted workstation.

## Part E: System Specifications:
The following system measurements establish a baseline reference point against which performance and resource utilisation will be evaluated in later phases. Capturing this baseline ensures that changes in CPU, memory, disk, and network behaviour can be attributed to specific workloads or security controls rather than initial configuration differences.

At this stage, swap is intentionally disabled to allow direct observation of memory pressure during performance testing, avoiding masked behaviour caused by disk-based swapping.
 
  As required by the coursework, I used the baseline to gather hardware and OS data to record the baseline configuration of the Ubuntu Server VM using the required CLI tools.

1.	Memory Information

 Command: free -h

 Purpose: Analyse memory and swap usages and print the result in human-readable units (Mib/    Gib). 
  	
 ![free1](./w1-free1.png) 

The server is allocated approximately 3.8 GB of RAM, with minimal usage at baseline due to the absence of active workloads. This provides sufficient headroom for essential security services such as SSH, firewall rules, and fail2ban, while still allowing controlled stress and application testing in later phases. Monitoring memory behaviour without swap enabled will allow clearer identification of memory saturation and performance degradation under load.
  
2. Disk Usage

Command:  df -h

Purpose: It displays disk layout and free space in a human-readable format 

![df1](./w1-df1.png)

 The result shows the root filesystem (/dev/sda2) is 25GB total with only 12% being used. This confirms that sufficient disk capacity is available for log generation, security auditing tools (e.g. Lynis), application installation, and performance testing without risking storage-related bottlenecks.

3. Distribution Information

Command: lsb_release -a
 
Purpose: It confirmed the operating system version 

![lsb_release1](./w1-lsb1.png)

Summary: The server is running Ubuntu Server 24.04.3 LTS (noble)

4. Kernel and System Information

Command: uname -a
   
Purpose: Display kernel version and system architecture

![uname1](./w1-uname1.png)

Summary: Kernel version: 6.8.0-40-generic on x86_64 architecture 

5. Network Interfaces

Command: ip addr

Purpose: Display all network interfaces and assigned IP addresses. Used to validate Correct VirtualBox networking(NAT + Host-Only).

Server VM Output:     

![ip addr1](./w1-ipaddr1.png)

Result:

•	enp0s3 (NAT): 10.0.2.15

•	enp0s8(Host-Only): 192.168.56.102

Workstation VM Output:

![ip addr2](./w1-ipaddr3.png)

Result:

•	enp0s3 (NAT): DHCP via VirtualBox

•	enp0s8 (Host-Only): 192.168.56.103

**Why This Matters**

 These results confirm that an isolated Host-Only network has successfully connected both virtual machines. This setup ensures that all administration server is run remotely via SSH, meet the necessary architectural and security requirements.


## WEEK 1: REFLECTION:

 The primary challenge during this phase was configuring the Host-Only network correctly. Initially, the workstation VM failed to receive the secondary interface due to a misalignment between VirtualBox adapter settings and the guest OS network configuration. This resulted in a missing enp0s8 interface and prevented private SSH communication.
 
Resolving this issue required understanding that Linux networking is dependent not only on operating system configuration but also on hypervisor-level networking. Rebuilding the workstation VM and reapplying the Host-Only adapter corrected the issue.

This experience taught me the importance of validating infrastructure assumptions early in system deployment. A misconfigured network at this stage would invalidate all subsequent security hardening and performance testing. As a result, this week established a reliable foundation for remote administration, monitoring, and security enforcement in later phases.
 

## References :

[1] Canonical Ltd., "Ubuntu Server 24.04 LTS Documentation," 2024.
    Available: https://ubuntu.com/server/docs. Accessed: 15 Dec 2025
    
[2] Oracle Corporation, "VirtualBox User Manual - Chapter 6: Virtual Networking," 2024.
    Available: https://www.virtualbox.org/manual/ch06.html. Accessed: 15 Dec 2025.

[3] Canonical Ltd.,  "Ubuntu Desktop 24.04 LTS Documentation," 2024.
    Available: https://ubuntu.com/desktop. Accessed: 15 Dec 2025.

[4] Linux Foundation, "Linux Manual Pages: uname(1), free(1), df(1), ip(8)," 2024.
    Available: https://man7.org/linux/man-pages/. Accessed:15 Dec 2025.

[5] Canonical Ltd., "Netplan- Official Configuration Reference," 2024.
    Available: https://netplan.io/reference. Accessed: 15 Dec 2025.

[6] Oracle Corporation, "VirtualBox Host-Only Networking Explained," 2024.
    Available: https://www.virtualbox.org/manual/ch06.html#network_hostonly. Accessed: 15 Dec 2025.
      
 [ Back to Home](../index.md)  




