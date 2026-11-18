# Network Topology and Configuration

## Overview

This document describes the network architecture, addressing, DNS services, and isolation controls implemented in my enterprise home lab.

The lab uses a dedicated Medialink AC1200 router and a Proxmox virtualization host. My Windows management workstation connects to the lab through Ethernet while maintaining a separate Wi-Fi connection for normal internet access.

The lab is designed to operate without an internet connection.

## Network Topology

```text
             Home Network / Internet
                       |
                     Wi-Fi
                       |
             Windows Management PC
              Ethernet: 192.168.8.20
                       |
                     Ethernet
                       |
               Medialink AC1200
                  RTR-01
                192.168.8.1
                WAN Disconnected
                       |
                     Ethernet
                       |
                Proxmox Host
                   PVE-01
                192.168.8.10
                       |
                 Virtual Bridge
                       |
               Ubuntu Server VM
                    VM 100
                192.168.8.116
               BIND9 DNS + SSH
```

## Network Addressing

| Device | Hostname | IP Address | Purpose |
|---|---|---|---|
| Lab Router | RTR-01 | 192.168.8.1 | Lab networking and DHCP |
| Proxmox Host | pve.lab.test | 192.168.8.10 | Virtualization |
| Ubuntu Server | ubuntu-srv01.lab.test | 192.168.8.116 | DNS and SSH |
| Windows Management PC | N/A | 192.168.8.20 | Remote administration |

Subnet: `192.168.8.0/24`

Internal DNS domain: `lab.test`

## Virtualization

The virtualization host uses Proxmox VE on an Intel Core i5-9600K system with 16 GB of RAM.

Ubuntu Server 26.04.1 LTS is deployed as VM 100.

The Ubuntu VM provides internal DNS services through BIND9 and remote administration through OpenSSH.

Proxmox snapshots are used to preserve working configurations before major changes.

## DNS Configuration

BIND9 is configured as the authoritative DNS server for the internal `lab.test` domain.

### Forward DNS

| Hostname | IPv4 Address |
|---|---|
| pve.lab.test | 192.168.8.10 |
| ubuntu-srv01.lab.test | 192.168.8.116 |

### Reverse DNS

| IPv4 Address | PTR Record |
|---|---|
| 192.168.8.10 | pve.lab.test |
| 192.168.8.116 | ubuntu-srv01.lab.test |

Ubuntu is configured through Netplan to use `192.168.8.116` for DNS while retaining DHCP addressing.

Windows uses a DNS Name Resolution Policy Table (NRPT) rule to direct `.lab.test` queries to the Ubuntu DNS server. Normal internet DNS remains on Wi-Fi.

## Network Isolation

The lab is intentionally separated from the internet.

The following controls were verified:

- The lab router reports its WAN connection as disconnected.
- Internet Connection Sharing is disabled on the Windows management PC.
- IPv4 forwarding is disabled on Windows network interfaces.
- The Windows lab Ethernet interface has no default gateway.
- Ubuntu could not reach the external IP address `1.1.1.1` during testing.
- Internal network communication remains functional.

These tests verify the intended isolation configuration for the network paths examined.

## Connectivity Testing

### Internal Connectivity

Ubuntu successfully reached the Proxmox host:

```bash
ping -c 4 192.168.8.10
```

Result: 4 packets received, 0% packet loss.

### Internal DNS

DNS resolution was tested using:

```bash
resolvectl query pve.lab.test
resolvectl query ubuntu-srv01.lab.test
```

Both hostnames resolved to their expected internal IP addresses.

### External Connectivity

```bash
ping -c 4 1.1.1.1
```

Result: 100% packet loss.

### Windows Network Forwarding

```powershell
Get-NetIPInterface -AddressFamily IPv4 |
    Format-Table InterfaceAlias,Forwarding
```

Result: Forwarding disabled on all listed interfaces.

## Troubleshooting and Lessons Learned

### Physical Network Connectivity

A loose Ethernet cable temporarily prevented access to Proxmox and its Ubuntu virtual machine.

The issue was identified through ping testing and physical connection inspection. Connectivity was restored after reconnecting the cable.

### DNS Performance

The Windows management PC initially had the isolated BIND9 server configured as a general DNS resolver.

External DNS queries through that server took approximately 10 seconds, compared with approximately 64 milliseconds through the normal Wi-Fi DNS server.

The configuration was changed to use an NRPT rule for `.lab.test` queries, preserving internal name resolution without directing ordinary internet DNS traffic to the isolated server.

A subsequent normal DNS query completed in approximately 14.5 milliseconds.

### Internet Connection Sharing

Windows Internet Connection Sharing was found enabled, creating an additional `192.168.137.1` address on the Ethernet adapter.

ICS was disabled, and the management workstation's static lab IP address was restored to `192.168.8.20`.

Connectivity to Proxmox and Ubuntu was verified afterward.

### Ubuntu DNS Client Configuration

Ubuntu initially received the lab router's DNS address through DHCP.

Netplan was updated to use the internal BIND9 server while retaining DHCP.

An initial configuration validation failed because the DHCPv4 and DHCPv6 DNS override settings did not match.

After correcting the configuration, `netplan generate` succeeded and `netplan try` was used to safely apply the changes.

## Current Status

**Phase 1 — Core Network Infrastructure: Completed**

The lab currently supports virtualization, remote Linux administration, internal forward and reverse DNS, and an isolated network environment.

Future work will focus on enterprise server services, centralized management, monitoring, backups, and automation.
