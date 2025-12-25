## Phase 2: Security Planning and Testing Methodology (WEEK – 2)
Week 2 focuses on planning the security baseline and performance testing methodology before making any changes to the server. Rather than configuring directly, this phase establishes a structured approach to monitoring system behavior and enforcing security controls in later weeks. Security planning in advance ensures consistent, measurable and aligned with both security and performance objectives. 


## Performance testing plan:
All performance monitoring will be conducted remotely from the Ubuntu Workstation using SSH, in line with the coursework requirement that the server remains headless and is administered remotely. I will collect performance data during baseline, load, and post optimization stages to compare system behavior under different conditions.

The goal of this performance testing plan is to observe how CPU, memory, disk, I/O and network performance changes as workloads are add and security controls are enabled.

Monitoring Tools and Purpose:

|Tool	        |Metric Collected	         |Purpose                          |Usage Stage       |
|-------------|--------------------------|---------------------------------|------------------|
|top	        |CPU usage, load average   |Identify CPU saturation          |All stages        |       
|htop         |Per-process CPU and RAM   |Identify resource-heavy processes|Load testing      | 
|vmstat	      |Memory, swap, I/O activity|Detect memory pressure           |Baseline and load |
|iostat       |Disk read/write throughput|Identify disk I/O bottlenecks    |I/O testing       |
|ping         |Network latency           |Measure responsiveness           |Network testing   |
|ip -s link   |	Packet counters          |Observe network traffic          |Network testing   |
|journalctl -f|	System logs	             |Detect errors during load	       |Load testing      |

---

## Testing Stages:
**1.	Baseline Monitoring:**

The system will be monitored in an idle state to establish reference values for CPU usage, memory consumption, disk activity and network behaviour.

**2.	Application Load Testing:**

The system monitors performance metrics in real time, while selected applications and workload will be introduced to stress specific system resource.

**3.	Performance Analysis:**

The collected data will later be analysed to identify bottlenecks in CPU, memory, disk I/O, or network throughput. 

**4.	Optimisation Testing:**

The system will re-test to measure quantitative improvement after applying firewall rule refinement or memory configuration.

**5.	Record results:**

Result will be recorded using screenshots, logs and structured tables and published to the GitHub page during later coursework weeks

---

## Data Collection Method:
I will perform data collection through terminal output captured via screenshots and, where appropriate, redirected to text files. Measurements will be taken at particular time intervals to compare between baseline, stressed and optimised states. This result will later be visualised using charts and graphs to support performance analysis in week 6.

---

## Security Configuration Checklist 
The following checklist defines the planned security baseline and how each control will be verified after implementation.

|Security Area	               |Planned Configuration	                                                            |Verification Methods                        |
|------------------------------|----------------------------------------------------------------------------------|--------------------------------------------|
|SSH Hardening                 | Disable password authentication, disable root login, and enforce key-based access|Review”/etc/ssh/sshd_config”, test SSH login|
|Firewall	                     |Enable UFW and restrict SSH to the host-only network                              | ufw status verbose                         |
|Mandatory Access Control 	   |Enforce AppArmor profiles                                                         |	aa-status                                  |
|Automatic Security Updates    |Enable unattended-upgrades                                                        | review update logs                         |
|User and privilege Management |Create a non-root admin user with sudo access                                     |	id, sudo -l                                |
|Networking and System Security|Verify open and service                                                           |ss -tuln, nmap                              |

This checklist ensures that all security controls are measurable, verifiable and auditable.

---

## Verification of Remote Access and Monitoring Capability:

To validate the planned security and performance testing methodology, basic verification commands were executed without altering system configuration. These checks confirm that remote administration and monitoring can be performed as required in later phases.

![Ping Success ](Week2-img/w2-ping-sucess.png)
 
 *ICMP ping test confirming Host-Only network connectivity between the workstation and server.*

![SSH Success](Week2-img/w2-ssh-success.png)
 
 *Successful SSH connection from the workstation to the server over the Host-Only network.*
 
![HTOP Monitoring](Week2-img/w2-htop.png)
 
 *Real-time system monitoring using htop to demonstrate planned performance observation tools.*

## Thread Model:
The following threat model identifies key risks relevant to the architecture and outlines mitigation strategies.
|Threat	                |Risk	                                      |Impacts                                                 |Mitigation                                                              |
|-----------------------|-------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------|
|Brute-force SSH attacks|Attackers try thousands of passwords       |Stolen data, loss of control                            |Configure SSH keys, fail2ban, and firewall rules                        |     
|Privilege escalation   |Attackers gain root privileges             |Full system takeover, able to disable security controls |Non-root admin user, enable AppArmor                                    |
|Outdated packages      |Known CVEs can be used to attack the system|Malware entry, data leakages, system disruption         |Automatic security updates, enable regular patching, monitor update logs|

This threat model directly informs the security controls planned for implementation in weeks 4 and 5.

## REFLECTION:
This week, we focused on planning rather than implementation, but it shows how closely security and performance considerations are linked. Developing a structured performance testing methodology reinforced the importance of collecting consistent and comparable data rather that running ad-hoc tests. Creating a threat model also clarified why basic controls such as SSH hardening and firewall restrictions are crucial, even in a small virtualised environment. This planning phase ensure that future configuration and optimisation work is systematic, measurable and defensible. 

---

## References:

[1] Canonical Ltd., “OpenSSH Server Configuration on Ubuntu,” Ubuntu Documentation, 2024. [Online]. Available: https://ubuntu.com/server/docs/service-openssh. [Accessed: 14-Dec-2025].

[2] Canonical Ltd., “Security Best Practices for Ubuntu Servers,” Ubuntu Documentation, 2024. [Online]. Available: https://ubuntu.com/security. [Accessed: 14-Dec-2025].

[3] DigitalOcean, “How To Set Up a Firewall Using UFW on Ubuntu,” 2023. [Online]. Available: https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands. [Accessed: 14-Dec-2025].

[4] Fail2ban Project, “Fail2ban Documentation,” 2024. [Online]. Available: https://www.fail2ban.org/wiki/index.php/Main_Page. [Accessed: 14-Dec-2025].

[5] Canonical Ltd., “Unattended Upgrades — Automatic Security Updates,” Ubuntu Documentation, 2024. [Online]. Available: https://ubuntu.com/server/docs/package-management. [Accessed: 14-Dec-2025].



⬅️ [Previous: Week 1](../Week1/Week1.md)  ⏭️ [Next: Week 3](../Week3/Week3.md)
