# Enterprise Windows Server Infrastructure Lab

## Project Overview

This project demonstrates the design, configuration, and administration of a multi-server Windows Server infrastructure using VMware Workstation.

The lab simulates a small enterprise IT environment, focusing on centralized identity management, network services, security policies, remote access, and server backup.

## Full Project Documentation

For detailed implementation steps, configuration details, screenshots, and testing results, see the complete project documentation:

[View Full Project Documentation](PROJECT-DOCUMENTATION.md)

## Technologies Used

- Windows Server 2022
- Windows 11 Pro
- VMware Workstation
- Active Directory Domain Services (AD DS)
- DNS and DHCP
- Group Policy (GPO)
- DFS Namespaces and DFS Replication
- Routing and Remote Access Service (RRAS)
- Network Policy Server (NPS) and RADIUS
- IKEv2 VPN
- Active Directory Certificate Services (AD CS)
- Windows Server Backup

## Project Objectives

- Deploy two domain controllers and configure Active Directory replication.
- Configure DNS, DHCP scopes, and DHCP failover.
- Organize users, groups, and organizational units.
- Implement security policies using Group Policy.
- Configure file sharing, access permissions, drive mapping, and DFS.
- Implement network routing, NAT, and internet connectivity.
- Configure secure remote access using IKEv2 VPN and NPS/RADIUS authentication.
- Deploy a certificate authority and issue a VPN server certificate.
- Enable remote server administration over VPN.
- Configure full-server backups and scheduled backup automation.

## Lab Environment

- **Domain:** `ad.lab`
- **Virtualization:** VMware Workstation
- **Operating Systems:** Windows Server 2022 and Windows 11 Pro

## Project Status

Core infrastructure configuration and testing completed.

Screenshots, configuration details, and validation results are documented in this repository.

## Disclaimer

This is an educational home lab built in a virtualized environment. It is not a production deployment.
