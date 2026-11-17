ENTERPRISE HOME LAB
===================

PROJECT OVERVIEW
----------------
A personal IT infrastructure lab designed to simulate a small enterprise environment using repurposed hardware.

The purpose of this project is to develop hands-on experience with network administration, virtualization, Linux systems, cybersecurity, and infrastructure troubleshooting.

The lab uses a dedicated network separated from my primary home network.

PROJECT GOALS
-------------
- Build and maintain a virtualized server environment.
- Configure and troubleshoot network services.
- Gain experience administering Linux and Windows servers.
- Implement enterprise technologies such as Active Directory.
- Develop skills in monitoring, security, and automation.
- Document configurations, problems, and solutions.

HARDWARE
--------
Proxmox Host
  CPU: Intel Core i5-9600K
  RAM: 16 GB
  Role: Virtualization server

Secondary Server (Planned)
  CPU: Intel Core i5-8500
  RAM: 32 GB
  Role: Additional infrastructure services

Management Workstation
  Role: Windows administration and testing

Lab Router
  Model: Medialink AC1200
  Role: Dedicated lab network and DHCP

SOFTWARE AND TECHNOLOGIES
-------------------------
- Proxmox VE
- Ubuntu Server 26.04.1 LTS
- BIND9 DNS
- OpenSSH
- Windows PowerShell
- Windows Subsystem for Linux (WSL)
- Linux networking tools

NETWORK CONFIGURATION
---------------------
Network:            192.168.8.0/24
Lab Router:         192.168.8.1
Proxmox Host:       192.168.8.10
Ubuntu DNS Server:  192.168.8.116
Windows Workstation:192.168.8.20

Internal DNS Domain: lab.test

DNS Hostnames:
  pve.lab.test
  ubuntu-srv01.lab.test

COMPLETED WORK
--------------
1. Installed and configured Proxmox VE.
2. Created an Ubuntu Server virtual machine.
3. Established network connectivity between systems.
4. Configured SSH for remote server administration.
5. Installed BIND9 on Ubuntu Server.
6. Created an internal forward DNS zone.
7. Configured DNS A records for lab servers.
8. Created a reverse DNS zone with PTR records.
9. Verified forward and reverse DNS resolution.
10. Tested DNS queries from Windows PowerShell.
11. Created Proxmox snapshots for recovery.

TROUBLESHOOTING EXPERIENCE
--------------------------
Network Connectivity
  Diagnosed connectivity issues involving network
  interfaces, IP addressing, and default gateways.

Offline Package Installation
  Encountered package installation failures because
  the lab network does not have internet access.

  Used WSL on an internet-connected workstation to
  download packages and transfer them to Ubuntu Server.

DNS Configuration
  Configured BIND9 to provide internal name resolution.

  Validated DNS configuration files and zone records
  before restarting the DNS service.

  Confirmed forward and reverse DNS functionality
  using dig and PowerShell Resolve-DnsName.

CURRENT PROJECT STATUS
----------------------
Phase 1: Initial Infrastructure

Completed:
  - Proxmox installation
  - Ubuntu Server deployment
  - SSH remote administration
  - Internal DNS server
  - Forward and reverse DNS testing

In Progress:
  - Finalizing DNS client configuration
  - Verifying network isolation
  - Expanding project documentation

PLANNED DEVELOPMENT
-------------------
Phase 2: Enterprise Services
  - Deploy Windows Server
  - Configure Active Directory Domain Services
  - Implement centralized authentication
  - Configure Group Policy

Phase 3: Infrastructure Management
  - Deploy monitoring services
  - Configure centralized logging
  - Implement backup and recovery procedures
  - Develop automation scripts

Phase 4: Security and Testing
  - Implement network security controls
  - Perform vulnerability assessments
  - Test system recovery procedures
  - Document troubleshooting scenarios

PROJECT STATUS: ACTIVE DEVELOPMENT

This repository will be updated as new infrastructure,
services, and configurations are implemented.
