\# Linux Server Security Hardening Lab



This project demonstrates a basic security assessment and hardening process for a Red Hat Enterprise Linux (RHEL) server using Kali Linux.



The lab focuses on identifying exposed services, reducing unnecessary attack surface, applying firewall segmentation, and verifying the security changes using Nmap.



\## Lab Environment



\- Kali Linux - Security assessment machine

\- Red Hat Enterprise Linux 10.2 - Target server

\- Oracle VirtualBox - Virtualization platform

\- Nmap - Network scanning and service detection

\- firewalld - Firewall configuration

\- systemd - Service and socket management



\## Network Setup



The lab uses two network adapters on each virtual machine:



\- NAT adapter for Internet access

\- Internal Network (`sec-lab`) for isolated communication between Kali Linux and RHEL



Internal lab addresses:



\- Kali Linux: `192.168.50.10`

\- RHEL Server: `192.168.50.20`



\## Baseline Assessment



An initial Nmap scan was performed from Kali Linux against the RHEL server to identify exposed services.



The scan identified two open TCP ports:



\- `22/tcp` - SSH

\- `9090/tcp` - Web administration service



!\[Baseline Nmap Scan](screenshots/01-baseline-nmap-scan.png)



\## Service Detection



A service version scan was performed to identify the software running on the exposed ports.



The results identified:



\- OpenSSH 9.9 on TCP/22

\- An active service on TCP/9090 that required further investigation



!\[Service Detection](screenshots/02-service-detection.png)



\## Finding 1 - Unnecessary Cockpit Exposure



Further investigation on the RHEL server confirmed that the Cockpit web console was enabled and listening on TCP/9090.



Because Cockpit was only used previously for training and was no longer required, keeping it enabled increased the unnecessary attack surface.



!\[Cockpit Before Hardening](screenshots/03-cockpit-before-hardening.png)



To reduce unnecessary exposure, the Cockpit socket was disabled and the Cockpit service was removed from the firewall configuration.



!\[Cockpit Hardening](screenshots/04-cockpit-hardening.png)



\## Finding 2 - Firewall Segmentation



The RHEL server initially had both network interfaces assigned to the same firewall zone.



To reduce unnecessary exposure, the interfaces were separated based on their purpose:



\- `enp0s3` was kept in the `public` zone for Internet access

\- `enp0s8` was moved to the `internal` zone for the isolated lab network

\- SSH access was removed from the public zone and kept available only through the internal network



!\[Firewall Segmentation](screenshots/05-firewall-segmentation.png)



\## Final Verification



A final Nmap scan was performed from Kali Linux after the hardening changes.



The results showed:



\- SSH remained accessible on TCP/22 through the internal lab network

\- TCP/9090 was no longer exposed



!\[Final Verification](screenshots/06-final-verification-scan.png)



\## Before vs After



| Item | Before Hardening | After Hardening |

|---|---|---|

| SSH | Open | Open on internal network |

| Cockpit / TCP 9090 | Open | Not exposed |

| Firewall zones | Both interfaces in public | Interfaces separated into public and internal zones |

| Attack surface | Larger | Reduced |



\## Skills Practiced



\- Linux system administration

\- Network scanning and service detection with Nmap

\- systemd service and socket management

\- firewalld configuration

\- Network segmentation

\- Security hardening

\- Attack surface reduction

\- Verification of security changes

\- Virtual machine networking



\## What I Learned



This lab helped me understand how a security assessment is performed from an external machine and how scan results can be verified directly on the target server.



I also learned that an open port does not automatically mean a vulnerability. The exposed service must first be identified, its purpose reviewed, and then a decision should be made based on whether the service is actually required.



The lab also demonstrated how firewall zones can be used to separate network interfaces and limit administrative access to the network where it is needed.





