# Phase 1: System Planning and Distribution Selection (Week 1)

This week, I will focus on building a reliable virtualised environment for the rest of the coursework. I have set up two virtual machines inside Oracle VirtualBox on my Windows host system:  

•	Workstation VM: Ubuntu Desktop 24.04

•	Server VM: Ubuntu Server 24.04

Both VMS are connected using a dual-adapter configuration:

•	Adapter 1- NAT: provide internet access for installation and updates.

•	Adapter 2- Host-Only Network: provides a private, isolated internal network that exist only between the workstation to the server.

## Part A - System Architecture Diagram:
![Architecture Diagram](./w1-architecture.png)

This dual-adapter design ensures the server is reachable only from
the workstation, replicating a secure administration model used in real data centres.

## Part B - Distribution Selection Justification:
Linux Distribution Family Comparison:
|Family         |Example | Strengths |   weakness| Reason Not Selected |
|---------------|--------|-----------|-----------|---------------------|
|Debian Family  |Debian , Ubuntu|Very Stable,widely Supported |Debian packages are often outdated |Ubuntu offer stability +fresher packages|
|Red Hat Family |	RHEL, CentOS stream|	Enterprise-grade tools	|CentOS Stream changes too frequently |Overkill for small VM environment|
|Arch Family	|Arch Linux, Manjiro 	|Very flexible and up-to-date 	|Requires a lot of manual setup |Not Ideal for a server workload|
|SUSE Family	|OpenSUSE, SLES|	Powerful admin tools 	|More complex for beginners |Minimal learning resources|

WHY I CHOSE UBUNTU SERVER 24.04.LTS:

I picked Ubuntu for several reasons:

•	LTS reliability: It provides long-term support with five years of security updates.

•	Strong documentation: Reliable for commands, services and hardening procedures

•	Included AppArmor by default: required access control for later in my project.

•	Simple and dependable networking: For a headless environment, Netplan with system-networkd is perfect.

•	Industry relevance: Due to Ubuntu’s dominance of cloud platforms (AWS, Azure and GCP), this setup is professionally valuable. 

 Lastly, considering all the above reasons, Ubuntu server provides simple, secure and up-to-date features.

## PART C: Workstation Configuration decision:
 
 I have chosen Ubuntu Desktop as my workstation environment compared to a Windows host or a hybrid setup. Reason:

 WHY UBUNTU DESKTOP IS THE BEST CHOICE?

•	Native Linux tooling:  It included SSH, client, package managers, monitoring tools, and scripting support by default.

•	Consistency: Using Linux on both virtual machines (VMs) minimises command mismatches and makes troubleshooting easier.

•	Safe separation: To avoid accidental configuration on the wrong system, the workstation (VMs) isolates all administrative activity from the Windows host.

•	Ideal for scripting week: Bash scripts behave consistently on both the  workstation and server.

Why not choose a Windows host directly?

•	For SSH, Windows uses PowerShell PuTTY, which makes later scripting and automation tasks more difficult.

•	The industry standard, particularly for server environments, is Linux-to-Linux administration. 

## Part D: Network Configuration Documentation:
For my Network setup, I used two network adapters:  NAT and Host-Only. 

Adapter 1 — NAT (Internet Access)

The first adapter is set to NAT, which provides internet access through the host machine to download, security updates, and package installation. 

![Adapter 1 (NAT)](./w1-adapter1.png)

Adapter 2 — Host-Only Network (Private SSH Network)

The second network adapter is set to Host-Only, a private virtual network between the workstation VM and the server VM. 

![Adapter 2 (Host-Only)](./w1-adapter2.png)

Host-Only Network:

•	IPv4 Network: 192.168.56.0/24

•	Host Adapter: 192.168.56.1

•	DHCP Range: 192.168.56.101 – 192.168.56.254

Final IP Addresses:

•	Workstation VM (enp0s8): 192.168.56.103

•	Server VM (enp0s8): 192.168.56.102

## Part E: System Specifications:

Ubuntu server:

As required by the coursework, I used the baseline to gather hardware and OD data to record the baseline configuration of the Ubuntu server 

1.	Memory Information

 Command: free -h

 Purpose: Analyse memory and swap usages and print the result in human-readable units (Mib/    Gib). 
  	
 ![free1](./w1-free1.png) 

 Summary:

  Here, the VM has a total of 3.8 GB of RAM and mostly is free since no heavy processes         have been run yet. Swap is not set up. This confirms that the server has enough capacity      for further performance workload and monitoring 


2. Disk Usage

Command:  df -h

Purpose: It displays disk layout and free space in a human-readable format 

![df1](./w1-df1.png)

The result shows the root filesystem (/dev/sda2) is 25GB total with only 12% being used. This confirms that I have plenty of space for the future and is cleanly configured.

3. Distribution Information

Command: lsb_release -a
 
Purpose: It confirmed the operating system version 

![lsb_release1](./w1-lsb1.png)

Summary: The server is running Ubuntu Server 24.04.3 LTS (noble)

4.	Kernel and System Information

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

•	enpos8(Host-Only): 193.164.56.102

Workstation VM Output:

![ip addr2](./w1-ipaddr2.png)

Result:

•	enp0s3 (NAT): DHCP via VirtualBox

•	enp0s8 (Host-Only): 192.168.56.103

Why this is important:

These results verify that an isolated Host-Only network has been used correctly on both virtual machines. This setup ensures that all administration of the server is performed remotely via SSH, meeting the required security and architectural constraints.

## REFLECTION:
This week was all about the foundation for the entire coursework by setting up both virtual machines and configuring the network correctly. I initially faced some issues with VirtualBox, especially with the network adapter and the server installation freezing, but after working and understanding the problems, I have learn important of correct VM settings. Setting up Ubuntu Server in a headless environment pushed me to fully rely on the command line, which boosted my confidence with basic Linux commands. I have also gathered system information using tools like free, df, ip addr and uname gave me a solid baseline which use in later weeks to analyse.

      
 [ Back to Home](../index.md)  




