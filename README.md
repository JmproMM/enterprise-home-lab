# Enterprise Home Lab

## Project Overview

A personal IT infrastructure lab designed to simulate a small enterprise environment using repurposed hardware.

The purpose of this project is to develop hands-on experience with network administration, virtualization, Linux and Windows server administration, cybersecurity, and infrastructure troubleshooting.

The lab operates on a dedicated network separated from my primary home network and the internet.

## Project Goals

- Build and maintain a virtualized server environment.
- Configure and troubleshoot network services.
- Gain experience administering Linux and Windows servers.
- Implement enterprise technologies such as Active Directory.
- Develop skills in monitoring, security, and automation.
- Document configurations, problems, and solutions.

## Hardware

| Device | Specifications | Role |
|---|---|---|
| Proxmox Host | Intel Core i5-9600K, 16 GB RAM | Virtualization server |
| Secondary Server (Planned) | Intel Core i5-8500, 32 GB RAM | Additional infrastructure services |
| Management Workstation | Windows PC | Administration and testing |
| Lab Router | Medialink AC1200 | Isolated network and DHCP |

## Software and Technologies

- Proxmox VE
- Ubuntu Server 26.04.1 LTS
- Windows Server 2025
- BIND9 DNS
- OpenSSH
- VirtIO Drivers
- QEMU Guest Agent
- Windows PowerShell
- Windows Subsystem for Linux (WSL)
- Linux networking tools

## Network Configuration

| Device | IP Address | Role |
|---|---|---|
| Lab Router | 192.168.8.1 | DHCP and local networking |
| Proxmox Host | 192.168.8.10 | Virtualization management |
| Ubuntu Server | 192.168.8.116 | DNS and Linux services |
| Windows Server | 192.168.8.30 | Future Active Directory server |
| Windows Workstation | 192.168.8.20 | Management and testing |

**Network:** `192.168.8.0/24`

**DHCP Range:** `192.168.8.100–200`

**Internal DNS Domain:** `lab.test`

### DNS Hostnames

| Hostname | IP Address | Status |
|---|---|---|
| pve.lab.test | 192.168.8.10 | Configured |
| ubuntu-srv01.lab.test | 192.168.8.116 | Configured |
| win-srv01.lab.test | 192.168.8.30 | Planned DNS record |

### Network Isolation

The lab network is physically separated from the primary home network.

- The lab router has no internet connection.
- The Windows management workstation connects to the lab through Ethernet and to the internet through Wi-Fi.
- IP forwarding between network interfaces is disabled.
- Ubuntu Server cannot reach external internet addresses.
- Windows Server has no default gateway configured.
- Internal communication between lab systems has been verified.

## Virtual Machines

### VM 100 — Ubuntu Server

| Setting | Value |
|---|---|
| Operating System | Ubuntu Server 26.04.1 LTS |
| Hostname | ubuntu-srv01 |
| IP Address | 192.168.8.116 |
| RAM | Approximately 3.3 GB |
| Disk | 32 GB |
| Network | vmbr0 |

**Configured Services:**

- OpenSSH for remote administration.
- BIND9 for internal DNS.
- Forward DNS zone for `lab.test`.
- Reverse DNS zone for `192.168.8.0/24`.
- DNS client configured to use `192.168.8.116`.

Forward and reverse DNS resolution have been tested successfully.

### VM 101 — Windows Server

| Setting | Value |
|---|---|
| Operating System | Windows Server 2025 |
| Hostname | WIN-SRV01 |
| IP Address | 192.168.8.30 |
| CPU | 2 vCPUs |
| RAM | 4 GB |
| Disk | 60 GB |
| BIOS | OVMF (UEFI) |
| TPM | Version 2.0 |
| Network | vmbr0, Intel E1000 |
| DNS Server | 192.168.8.116 |
| Default Gateway | None |

**Completed Configuration:**

- Installed Windows Server 2025 with Desktop Experience.
- Loaded VirtIO SCSI drivers during installation.
- Installed VirtIO guest tools.
- Configured a static IPv4 address.
- Configured internal DNS.
- Verified connectivity to Proxmox and Ubuntu Server.
- Installed and verified QEMU Guest Agent.
- Created a Proxmox recovery snapshot.

**Snapshot:** `windows-server-base-config`

Active Directory Domain Services has not yet been installed.

## Completed Work

1. Installed and configured Proxmox VE.
2. Created an Ubuntu Server virtual machine.
3. Established connectivity between the lab systems.
4. Configured SSH for remote Linux administration.
5. Installed BIND9 using offline package installation.
6. Created internal forward DNS records.
7. Configured reverse DNS with PTR records.
8. Verified forward and reverse DNS resolution.
9. Configured Windows DNS resolution for the internal domain.
10. Configured Ubuntu Server to use its own DNS service.
11. Tested network isolation and connectivity.
12. Installed Windows Server 2025 on Proxmox.
13. Configured Windows Server hostname and static networking.
14. Installed VirtIO drivers and QEMU Guest Agent.
15. Created Proxmox snapshots for recovery.
16. Documented the infrastructure and troubleshooting process.

## Troubleshooting Experience

### Network Connectivity

Diagnosed network connectivity problems involving Ethernet connections, IP addressing, routing, and network interfaces.

Identified and corrected a loose Ethernet connection that prevented access to Proxmox.

### Offline Package Installation

Encountered package installation failures because the lab network does not have internet access.

Used WSL on an internet-connected workstation to download packages and transfer them to Ubuntu Server for offline installation.

### DNS Configuration

Installed and configured BIND9 to provide internal name resolution.

Validated DNS configuration files and zone records before restarting the DNS service.

Verified forward and reverse DNS functionality using `dig`, `resolvectl`, and PowerShell `Resolve-DnsName`.

Configured Windows DNS namespace rules so internal lab queries use the lab DNS server without disrupting normal internet DNS resolution.

### Windows Server Deployment

Installed Windows Server 2025 in a Proxmox virtual machine.

Windows Setup initially could not detect the virtual SCSI disk. Resolved the issue by loading the VirtIO SCSI storage driver from a separate ISO.

Configured static networking and verified connectivity between Windows Server, Ubuntu Server, and Proxmox.

### Network Isolation

Tested communication between lab devices while confirming that the isolated network could not reach external internet addresses.

Verified that IP forwarding was disabled on the Windows management workstation.

## Current Project Status

### Phase 1 — Initial Infrastructure

**Completed**

- Proxmox installation
- Ubuntu Server deployment
- Windows Server 2025 deployment
- SSH remote administration
- Internal BIND9 DNS server
- Forward and reverse DNS configuration
- DNS client configuration
- Network isolation testing
- Static IP configuration
- VirtIO drivers and QEMU Guest Agent
- Proxmox recovery snapshots
- Initial GitHub documentation

**In Progress**

- Expanding infrastructure documentation
- Preparing Active Directory deployment
- Planning integration between Active Directory and existing DNS services

## Planned Development

### Phase 2 — Enterprise Services

- Configure Windows Server DNS records.
- Install Active Directory Domain Services.
- Configure an internal Active Directory domain.
- Implement centralized user authentication.
- Create domain users and security groups.
- Configure Group Policy.
- Test domain administration and authentication.

### Phase 3 — Infrastructure Management

- Deploy monitoring services.
- Configure centralized logging.
- Implement backup and recovery procedures.
- Develop automation scripts.
- Expand infrastructure documentation.

### Phase 4 — Security and Testing

- Implement network security controls.
- Perform vulnerability assessments.
- Test system recovery procedures.
- Document troubleshooting scenarios.
- Practice incident response and system administration.

## Documentation

Detailed documentation of the home lab infrastructure, configuration, and troubleshooting.

- [Network Topology and Configuration](docs/network-topology.md) — Network architecture, IP addressing, BIND9 DNS, isolation testing, and troubleshooting.

Additional documentation will be added as the lab expands.

---

**Project Status:** Active Development

This repository will be updated as new infrastructure, services, and configurations are implemented.
