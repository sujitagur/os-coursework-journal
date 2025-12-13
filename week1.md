# Phase 1: System Planning and Distribution Selection (Week 1)

## Part A - System Architecture Diagram:
Here, I have designed and built the basic virtual lab environment for the coursework. The setup uses two virtual machines on VirtualBox: Ubuntu Server and Ubuntu Desktop Workstation. Both are connected with NAT for internet access and a Host-Only network for private communication.
![Architecture Diagram]

## Part B - Distribution Selection Justification:
Linux Distribution Family Comparison:
|Family         |Example | Strengths |   weakness|
|---------------|--------|-----------|-----------|
|Debian Family  |Debian , Ubuntu|Very Stable,widely Supported |Debian packages are often outdated|
|Red Hat Family |	RHEL, CentOS stream	Enterprise-grade tools	|CentOS Stream changes too frequently|
|Arch Family	|Arch Linux, Manjiro 	|Very flexible and up-to-date 	|Requires a lot of manual setup|
|SUSE Family	|OpenSUSE, SLES	Powerful admin tools 	|More complex for beginners|
|Chosen: Ubuntu server 24.04 LTS|	  --	|Easy, familiar, LTS support	|Slightly heavier than minimal distros|

WHY I CHOSE UBUNTU SERVER 24.04.LTS:

I picked Ubuntu server because it’s reliable, easy to learn, and something I’m already familiar with from my lab sessions. It provides long-term support with five years of security updates, which makes it suitable for my coursework for testing requirements.

Compared with CentOS Stream, Debian, Arch and SUSE, Ubuntu is simply the most practical option here.  It has newer packages than Debian, it has fewer unexpected changes than CentOS Stream, and less complexity than SUSE or Arch. It works smoothly with VirtualBox and supports SSH, AppArmor, firewall tools, and performance testing, which are required in my project.

Overall, Ubuntu Server is perfect for my coursework, balancing stability and usability.

# PART C: Workstation Configuration decision:
 Chosen: Ubuntu Desktop (VirtualBox VM)

I chose Ubuntu Desktop as my workstation because it offers a complete graphical environment while still giving me full access to the Linux command-line tools I need for remote administration. By using a separate VM for the workstation, I’m following the dual-system architecture required for my coursework, where the server stays headless, and all configurations are done over SSH.

Having a desktop Linux distribution makes it much easier to run monitoring tools, manage SSH connections, capture screenshots, and organise files for my technical journal. Plus, it helps me avoid compatibility issues that might arise when using Windows as the host system. Since Ubuntu Desktop shares the same underlying architecture as the Ubuntu Server VM, my workflow remains consistent, and the commands work the same way in both environments.

This setup reflects real-world industry practices, where an administrator manages remote servers from a separate workstation.

## Part D: Network Configuration Documentation:
For my Network setup, I used two network adapters:  NAT and Host-Only. Each adapter serves a specific purpose, and together they create a setup that’s both secure and practical for remote management.

Adapter 1 — NAT (Internet Access)

The first adapter is configured to NAT, which allows the server to access the internet through my host machine. This is super important because the server will need to download security updates and install packages later on in the coursework. NAT is great because it works automatically without any extra fuss, and it keeps the server separate from my actual home network.
![Adapter 1]

Adapter 2 — Host-Only Network (Private SSH Network)

The second network adapter is set to Host-Only, creating a private virtual network between the workstation VM and the server VM. This adapter doesn’t connect to the internet at all — it only allows communication within VirtualBox. I will use this network for SSH access later.

Host-Only network keeps the server isolated from external networks, only connecting to my workstation. This means only communication between virtual machines on the same host.

Having a Host-Only network is key because it keeps the server completely isolated from external networks, ensuring that only my workstation can connect to it. This aligns with the module’s requirement that all server administration must be done remotely over a secure connection.
![Adapter 1]

Why does this setup work?

- NAT keeps the server updated and functional
- Host-Only provides a controlled and secure environment for remote management

It also reflects how real servers are often set up and how one interface is connected with the internet for updates, and managed by others on a private internal network. This makes the environment realistic while still following the module’s safety and security requirements.

## Part E: System Specifications:

Ubuntu server:

To document the initial state of my Ubuntu Server VM, I used several Linux command-line to check the system memory, storage, tools to collect hardware, memory, storage, network interfaces, operating system version, and kernel version.

Below is a proper summary of all my commands, which are run directly on the server terminal, and I have also included the screenshots and my outputs.

1.	Memory Information – free -h

  	free -h analysis memory and swap usages and printed the result in human-readable units (Mib/ Gib). Here, my server VM has a total of 3.8 GB of RAM and mostly is available cause I have not run any heavy processes at this stage and also Swap: 0 B means no swap space is currently configured on the system.
2.   Disk Usage – df -h

      Using df -h, I have checked the disk layout and free space. The result shows that my root partition(/dev/sda2) is 25GB and only a small part(12%) is used after installation. The tmpfs filesystems, which are located in RAM and are restored upon reboot used for runtime operation and improve system performance by avoiding disk I/O. This means my server is cleanly configured and ready for operation.

3.    Network Interfaces – ip addr

      The `ip addr` command shows that both of my virtual network adapters—NAT and Host-Only—are up and running: 
- ens3 (NAT): receives an address in the 10.0.2.x range 
- enp0s8 (Host-Only): gets an address from the VirtualBox host-only range

These IP addresses matter because I’ll be using the Host-Only IP later on when I set up SSH from the workstation.

4.    Distribution Information - lsb_release -a
  
When I run lsb_release -a, it confirms that the server is operating on Ubuntu Server 24.04.3. Plus, it aligns perfectly with the version I am using in my lab sessions, making it a easy to follow the documentation and keep up with class activities.

5.	Kernel and System Information - uname -a
   
The output from uname -a indicates that the server is running the Linux 6.8.x kernel on a 64-bit architecture. This reassures me that the system is current and ready to work with the tools I’ll need down the line.

## Workstation – Ubuntu Desktop:
1.	Kernel and System Information – uname -a
The workstation is running a 64-bit Linux kernel, which is standard for Ubuntu 24.04LTS and supports modern hardware like for desktop use.

2.	Memory Information - free -h
The virtual machine is set up with 4GB of RAM, which is very little but still works fine for Ubuntu Desktop. Swap space is not enabled, which is standard for VirtualBox VMs unless you decide to set it up manually.
 

3.	Storage information - df -h
The workstation uses a 25GB virtual disk, offering plenty of free space for desktop applications and any lab work. We can see the root filesystem is neatly organised.
 

4.	Network Interfaces – ip addr
The workstation is running on VirtualBox’s NAT networking, which means it gets an automatically assigned IP address (10.0.2.15) from internal DHCP. This setup works well for a desktop VM unless we need to enable communication between multiple VMs. In that case, we can switch to Host-Only or Internal networks.

5.	Distribution Information – lsb_release – a
This machine is running in Ubuntu 24.04.3 LTS, which means a long-term support version that’s stable and perfect for a workstation environment.
 

## Overall Summary:
Collecting these system specifications gives me a complete technical snapshot of my Ubuntu Server VM at the starting point. Having this baseline recorded will help me understand the impact of the security configurations, installed applications, and performance tests I carry out in the upcoming weeks.

## REFLECTION:
This week was all about the foundation for the entire coursework by setting up both virtual machines and configuring the network correctly. I initially faced some issues with VirtualBox, especially with the network adapter and the server installation freezing, but after working and understanding the problems, I have learn important of correct VM settings. Setting up Ubuntu Server in a headless environment pushed me to fully rely on the command line, which boosted my confidence with basic Linux commands. I have also gathered system information using tools like free, df, ip addr and uname gave me a solid baseline which use in later weeks to analyse.

      
 [ Back to Home] (../index.md) | 




