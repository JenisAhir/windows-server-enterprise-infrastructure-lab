# Enterprise Windows Server Infrastructure Lab

## 1. Project Overview

This project demonstrates the design, deployment, configuration, and administration of an enterprise-style Windows Server infrastructure in a virtualized home lab using VMware Workstation.

The environment consists of two Windows Server 2022 domain controllers, a routing and remote access server, and Windows 11 client virtual machines.

The lab was designed to practice core infrastructure administration, including centralized identity management, network services, Group Policy, file sharing, distributed file systems, secure remote access, and server backup.

The project also demonstrates how multiple infrastructure services work together to provide centralized management, network connectivity, and remote administration.

All configurations and testing were performed in an isolated virtual lab environment for educational and portfolio purposes.

## 2. Lab Architecture and Network Topology

The lab was built using VMware Workstation with an isolated internal network and a separate NAT network for external connectivity.

The internal network uses the `192.168.50.0/24` subnet. Domain controllers and internal clients communicate through this network, with Router1 providing routing and NAT services.

### 2.1 Network Configuration

| Network Component | Configuration |
|---|---|
| Internal Network | VMnet10 |
| Internal Subnet | 192.168.50.0/24 |
| External/NAT Network | VMnet8 |
| Router1 LAN Address | 192.168.50.1 |
| Router1 WAN Address | DHCP |
| DC1 Address | 192.168.50.10 |
| DC2 Address | 192.168.50.11 |
| DHCP Address Pool | 192.168.50.100–192.168.50.200 |
| VPN Address Pool | 192.168.60.10–192.168.60.20 |
| Active Directory Domain | ad.lab |

### 2.2 Virtual Machines

| Virtual Machine | Operating System |
|---|---|
| DC1 | Windows Server 2022 |
| DC2 | Windows Server 2022 |
| Router1 | Windows Server 2022 |
| Client-PC | Windows 11 Pro |
| VPN-Client | Windows 11 Pro |

### 2.3 Network Topology

The following diagram illustrates the virtual infrastructure, internal network, routing configuration, domain controllers, and remote VPN connectivity.

![Windows Server Infrastructure Network Topology](Network-Diagram/Diagram.png)

### 2.4 Network Design

- **VMnet10:** Provides internal connectivity between the domain controllers, Router1, and internal client.
- **VMnet8:** Provides NAT connectivity for Router1's WAN interface and the VPN client.
- **Router1:** Connects the internal network to external connectivity through RRAS and NAT.
- **DC1 and DC2:** Provide redundant domain controller and network services.
- **VPN-Client:** Connects remotely to Router1 using IKEv2 VPN and accesses internal resources.

## 3. VMware Virtual Infrastructure

### 3.1 Virtual Machine Deployment

VMware Workstation was used to create and manage five virtual machines for the Windows Server infrastructure lab.

Each virtual machine was configured with the necessary virtual hardware resources and network adapter settings based on its intended role in the lab.

The following virtual machines were deployed:

| Virtual Machine | Operating System | Purpose |
|---|---|---|
| DC1 | Windows Server 2022 | Primary domain controller |
| DC2 | Windows Server 2022 | Additional domain controller |
| Router1 | Windows Server 2022 | Routing, NAT and VPN services |
| Client-PC | Windows 11 Pro | Internal workstation and Group Policy testing |
| VPN-Client | Windows 11 Pro | Remote VPN connectivity and administration testing |

### 3.2 Virtual Machine Hardware Configuration

The following screenshots show the basic virtual machine configurations in VMware Workstation, including the virtual hardware settings for each system.

#### DC1
![DC1 Virtual Machine Configuration](Screenshots/01-Virtual-Infrastructure/DC1-VM-Configuration.png)

#### DC2
![DC2 Virtual Machine Configuration](Screenshots/01-Virtual-Infrastructure/DC2-VM-Configuration.png)

#### Router1
![Router1 Virtual Machine Configuration](Screenshots/01-Virtual-Infrastructure/Router1-VM-Configuration.png)

#### Client-PC
![Client-PC Virtual Machine Configuration](Screenshots/01-Virtual-Infrastructure/Client-PC-VM-Configuration.png)

#### VPN-Client
![VPN-Client Virtual Machine Configuration](Screenshots/01-Virtual-Infrastructure/VPN-Client-VM-Configuration.png)

### 3.3 Virtual Network Design

Two VMware virtual networks were used to separate internal lab communication from external network connectivity.

- **VMnet10:** Internal virtual network used for communication between the domain controllers, Router1 and the internal workstation.
- **VMnet8:** VMware NAT network used for Router1's WAN connectivity and the remote VPN client.

Router1 was configured with two virtual network adapters:
- LAN adapter connected to VMnet10.
- WAN adapter connected to VMnet8.

This network design allowed Router1 to provide routing and NAT services between the internal lab network and external connectivity.

## 4. Active Directory Domain Services

Active Directory Domain Services (AD DS) was deployed to provide centralized identity management, authentication, and administration for the lab environment.

Two Windows Server 2022 virtual machines, DC1 and DC2, were configured as domain controllers within the `ad.lab` domain. DC1 was configured as the first domain controller, followed by the deployment of DC2 as an additional domain controller.

### 4.1 DC1 Initial Configuration

DC1 was prepared for the Active Directory deployment by configuring its computer name and static IPv4 address.

The following settings were configured:

- Computer name: DC1
- Static IP address: `192.168.50.10`
- Subnet mask: `255.255.255.0`
- DNS server: `192.168.50.10`

**DC1 Computer Name Configuration**

![DC1 Computer Rename](Screenshots/02-Active-Directory/DC1-Computer-Rename.png)

**DC1 Static IP Configuration**

![DC1 Static IP Configuration](Screenshots/02-Active-Directory/DC1-Static-IP-Configuration.png)

### 4.2 AD DS Installation and Domain Creation

The Active Directory Domain Services role was installed on DC1 through Server Manager.

DC1 was then promoted to a domain controller by creating a new forest with the domain name `ad.lab`.

This established the first domain controller and the foundation for centralized domain management.

**AD DS Role Installation**

![AD DS Installation on DC1](Screenshots/02-Active-Directory/DC1-ADDS-Installation.png)

**New Forest Creation**

![Active Directory New Forest Creation](Screenshots/02-Active-Directory/AD-New-Forest-Creation.png)

### 4.3 DC2 Domain Controller Deployment

DC2 was configured as an additional domain controller to provide redundancy and improve the availability of Active Directory services.

The deployment process included:

- Preparing DC2 with its initial network configuration.
- Joining DC2 to the existing `ad.lab` domain.
- Installing the Active Directory Domain Services role.
- Promoting DC2 to an additional domain controller.
- Configuring DNS during the domain controller promotion.

**DC2 Initial Configuration**

![DC2 Initial Configuration](Screenshots/02-Active-Directory/DC2-Initial-Configuration.png)

**DC2 Domain Join**

![DC2 Domain Join](Screenshots/02-Active-Directory/DC2-Domain-Join.png)

**DC2 Domain Controller Promotion**

![DC2 Domain Controller Promotion](Screenshots/02-Active-Directory/DC2-Domain-Controller-Promotion.png)

### 4.4 Active Directory Users and Groups

Organizational Units (OUs), user accounts, and security groups were created to organize and manage domain users.

The lab included separate IT and Support organizational units and corresponding security groups.

The following test accounts were created:

- Alex Morgan (`alex.morgan`) — IT user.
- Maria Jones (`maria.jones`) — Support user.

Security groups were used to organize users according to their roles and support future access control and Group Policy administration.

![Active Directory Users and Groups](Screenshots/02-Active-Directory/AD-Users-And-Groups.png)

### 4.5 Active Directory Replication

Active Directory replication was configured between DC1 and DC2 to synchronize directory information across both domain controllers.

Replication was verified to confirm that changes made within the domain could be replicated between the domain controllers.

![Active Directory Replication Configuration](Screenshots/02-Active-Directory/AD-Replication-Configuration.png)

![Active Directory Replication Verification](Screenshots/02-Active-Directory/AD-Replication-Verification.png)

### 4.6 Active Directory Implementation Summary

The Active Directory deployment established a centralized domain environment with two domain controllers.

The completed configuration provided:

- A new Active Directory forest and `ad.lab` domain.
- Two domain controllers, DC1 and DC2.
- Centralized user and group management.
- Organizational Units for IT and Support users.
- Active Directory replication between domain controllers.

These services provided the foundation for subsequent DNS, DHCP, Group Policy, file services, and remote access configurations.
