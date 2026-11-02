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

## 5. DNS and DHCP Services

### 5.1 DNS Configuration

DNS was configured on both domain controllers to provide name resolution for the `ad.lab` domain.

- DC1 (`192.168.50.10`) hosts the primary DNS service.
- DC2 (`192.168.50.11`) also hosts DNS to provide redundancy.
- A forward lookup zone was configured for `ad.lab`.
- Reverse lookup zones were configured to support IP address-to-hostname resolution.

**DNS configuration on DC1:**

![DC1 DNS Forward Lookup Zone](Screenshots/03-DNS-DHCP/DC1-DNS-Forward-Lookup-Zone.png)

![DC1 DNS Reverse Lookup Zone](Screenshots/03-DNS-DHCP/DC1-DNS-Reverse-Lookup-Zone.png)

**DNS configuration on DC2:**

![DC2 DNS Forward Lookup Zone](Screenshots/03-DNS-DHCP/DC2-DNS-Forward-Lookup-Zone.png)

![DC2 DNS Reverse Lookup Zone](Screenshots/03-DNS-DHCP/DC2-DNS-Reverse-Lookup-Zone.png)

DNS redundancy was verified by confirming that DNS records were available through both domain controllers.

![DNS Redundancy Verification](Screenshots/03-DNS-DHCP/DNS-Redundancy-Verification.png)

### 5.2 DHCP Scope Configuration

The DHCP role was configured to automatically assign IPv4 addresses to devices on the internal network.

The DHCP scope was configured with the following address range:

| Setting | Configuration |
|---|---|
| Network | `192.168.50.0/24` |
| Subnet Mask | `255.255.255.0` |
| Address Pool | `192.168.50.100–192.168.50.200` |
| Default Gateway | `192.168.50.1` |
| DNS Servers | `192.168.50.10`, `192.168.50.11` |
| DNS Domain | `ad.lab` |

![DHCP Address Pool](Screenshots/03-DNS-DHCP/DHCP-Address-Pool.png)

### 5.3 DHCP Scope Options

DHCP scope options were configured to provide clients with the network settings required to communicate with the domain and access other network resources.

- **Option 003 – Router:** `192.168.50.1`
- **DNS Servers:** `192.168.50.10` and `192.168.50.11`
- **DNS Domain Name:** `ad.lab`

![DHCP Option 003 Default Gateway](Screenshots/03-DNS-DHCP/DHCP-Option-003-Default-Gateway.png)

![DHCP DNS Server Configuration](Screenshots/03-DNS-DHCP/DHCP-DNS-Server-Configuration.png)

### 5.4 DHCP Client Address Assignment

A Windows client was configured to obtain its IPv4 configuration automatically through DHCP.

The assigned address and DNS settings were verified on the client.

![DHCP Client Address Assignment](Screenshots/03-DNS-DHCP/DHCP-Client-Address-Assignment.png)

![Client DNS Server Settings](Screenshots/03-DNS-DHCP/Client-DNS-Server-Settings.png)

### 5.5 DHCP Failover Configuration

DHCP failover was configured between DC1 and DC2 to provide continued DHCP service if one domain controller becomes unavailable.

The failover configuration was verified in the DHCP management console.

![DHCP Failover Configuration](Screenshots/03-DNS-DHCP/DHCP-Failover-Configuration.png)

### 5.6 Router1 DNS Configuration

Router1 was configured to use the domain controllers for DNS name resolution on the internal network.

![Router1 DNS Server Configuration](Screenshots/03-DNS-DHCP/Router1-DNS-Server-Configuration.png)

## 6. Group Policy Management

Group Policy was used to centrally manage user restrictions, workstation security settings, password requirements, and network drive mapping across the `ad.lab` domain.

### 6.1 Support User Restrictions

A Group Policy Object named `Support-Users` was created and linked to the Support organizational unit (OU).

The policy was configured to apply the following restrictions to Support users:

- Prevent access to Control Panel and PC settings.
- Prevent access to the Command Prompt.
- Restrict access to Windows PowerShell.

These settings demonstrate how Group Policy can be used to apply role-based restrictions to domain user accounts.

**Control Panel and Settings restriction:**

![Support GPO Control Panel Restriction](Screenshots/04-Group-Policy/Support-GPO-Control-Panel-Restriction.png)

**Command Prompt restriction:**

![Support GPO Command Prompt Restriction](Screenshots/04-Group-Policy/Support-GPO-Command-Prompt-Restriction.png)

**PowerShell restriction:**

![Support GPO PowerShell Restriction](Screenshots/04-Group-Policy/Support-GPO-PowerShell-Restriction.png)

**Verification of Support user restrictions:**

![Support GPO Restrictions Verification](Screenshots/04-Group-Policy/Support-GPO-Restrictions-Verification.png)

### 6.2 Workstation Security Policy

A workstation security GPO was configured to improve security on domain-connected computers.

The policy included:

- Automatically locking inactive workstations after 15 minutes.
- Displaying a legal notice before users sign in.

The logon notice was configured and verified on the Windows client.

![Workstation GPO Logon Notice Configuration](Screenshots/04-Group-Policy/Workstation-GPO-Logon-Notice-Configuration.png)

![Workstation GPO Logon Notice Verification](Screenshots/04-Group-Policy/Workstation-GPO-Logon-Notice-Verification.png)

### 6.3 Domain Password Policy

The domain password policy was configured through Group Policy to establish password requirements for domain user accounts.

![Domain Password Policy Configuration](Screenshots/04-Group-Policy/Domain-Password-Policy-Configuration.png)

### 6.4 Support Network Drive Mapping

A drive-mapping policy was created to provide Support users with access to a shared network folder.

The initial mapping used the DC1 file-share path:

`\\DC1\Support`

The drive was mapped to the `S:` drive on the client computer.

![Support Drive Mapping GPO Configuration](Screenshots/04-Group-Policy/Support-Drive-Mapping-GPO-Configuration.png)

![Support Drive Mapping Client Verification](Screenshots/04-Group-Policy/Support-Drive-Mapping-Client-Verification.png)

As the lab infrastructure was expanded with DFS, the drive-mapping configuration was updated to use the domain-based DFS namespace:

`\\ad.lab\Support`

This provides users with a consistent domain-based path rather than directly referencing one file server.

**Note:** A separate screenshot of the final GPO path change was not captured.

### 6.5 Group Policy Verification

Group Policy settings were tested using domain user accounts and a Windows client to confirm that the configured restrictions and workstation policies were applied.

The lab demonstrates centralized policy management through Active Directory and Group Policy.

## 7. File Services and Distributed File System (DFS)

File services were configured to provide shared network storage for domain users. Distributed File System (DFS) was then implemented to provide a domain-based namespace and replicate shared folder data between DC1 and DC2.

### 7.1 Shared Folder Creation on DC1

A shared folder named `Support` was created on DC1 and made available over the network using the following UNC path:

`\\DC1\Support`

This share was used as the initial network location for Support department files.

![DC1 Support Shared Folder](Screenshots/05-File-Services-DFS/DC1-Support-Shared-Folder.png)

### 7.2 Share and NTFS Permissions

Share permissions and NTFS permissions were configured to control access to the Support folder.

Access was assigned to the relevant Active Directory groups, including `IT` and `Support`, while administrative access was retained for management.

This demonstrates how Windows file sharing uses both share-level and NTFS permissions to control access to network resources.

**Share permissions:**

![Support Share Permissions](Screenshots/05-File-Services-DFS/Support-Share-Permissions.png)

**NTFS permissions:**

![Support NTFS Permissions](Screenshots/05-File-Services-DFS/Support-NTFS-Permissions.png)

### 7.3 DFS Role Installation

The DFS role services were installed on both DC1 and DC2 to support the domain-based namespace and folder replication.

![DFS Role Installation on DC1 and DC2](Screenshots/05-File-Services-DFS/DFS-Role-Installation-DC1-DC2.png)

### 7.4 Support Folder Preparation on DC2

A corresponding Support folder was prepared on DC2 as the second folder target for DFS replication.

![DC2 Support Folder Preparation](Screenshots/05-File-Services-DFS/DC2-Support-Folder-Preparation.png)

### 7.5 DFS Namespace Configuration

A domain-based DFS namespace was created using the following path:

`\\ad.lab\Support`

This namespace provides users with a consistent domain-based path to access the shared Support folder without having to specify an individual server name.

![DFS Namespace Creation](Screenshots/05-File-Services-DFS/DFS-Namespace-Creation.png)

### 7.6 DFS Replication Configuration

DFS Replication (DFSR) was configured to replicate the Support folder between DC1 and DC2.

The replication configuration established the folder membership and replication relationship between the two servers.

![DFS Replication Configuration](Screenshots/05-File-Services-DFS/DFS-Replication-Configuration.png)

![DFS Replication Folder Membership](Screenshots/05-File-Services-DFS/DFS-Replication-Folder-Membership.png)

### 7.7 DFS Replication Testing and Verification

A test file was created through the client to verify access to the shared folder.

Replication was then checked on both domain controllers to confirm that the test file was available on DC1 and DC2.

![DFS Client File Creation](Screenshots/05-File-Services-DFS/DFS-Client-File-Creation.png)

**Replication verification on DC1:**

![DFS Replication Verification DC1](Screenshots/05-File-Services-DFS/DFS-Replication-Verification-DC1.png)

**Replication verification on DC2:**

![DFS Replication Verification DC2](Screenshots/05-File-Services-DFS/DFS-Replication-Verification-DC2.png)

### 7.8 Outcome

The lab demonstrates Windows file sharing, access control through share and NTFS permissions, domain-based DFS namespace configuration, and folder replication between two domain controllers.

The DFS namespace provides a consistent access path, while DFS Replication maintains copies of the configured folder data across the participating servers.

## 8. Routing and Remote Access (RRAS) and NAT

Routing and Remote Access Service (RRAS) was configured on Router1 to provide routing and Network Address Translation (NAT) between the internal lab network and the external network.

### 8.1 Router1 Network Configuration

Router1 was deployed with two network adapters:

- **LAN adapter:** Connected to the internal VMware network, VMnet10.
- **WAN adapter:** Connected to the VMware NAT network, VMnet8.

The LAN adapter was assigned the static IP address `192.168.50.1`, which serves as the default gateway for devices on the internal network.

The WAN adapter obtained its IP configuration through DHCP from the VMware NAT network.

![Router1 WAN and LAN Network Adapters](Screenshots/06-RRAS-NAT/Router1-WAN-LAN-Network-Adapters.png)

### 8.2 Router1 Hostname and IP Configuration

Router1 was configured with its hostname and network settings before RRAS was installed.

![Router1 Hostname and IP Configuration](Screenshots/06-RRAS-NAT/Router1-Hostname-IP-Configuration.png)

### 8.3 RRAS Installation and Configuration

The Remote Access server role was installed on Router1. RRAS was configured to provide LAN routing and NAT functionality.

![Router1 RRAS Installation](Screenshots/06-RRAS-NAT/Router1-RRAS-Installation.png)

### 8.4 NAT Configuration

NAT was configured in RRAS to allow internal network devices to access external networks through Router1's WAN connection.

The internal LAN interface was configured as the private network interface, while the WAN interface was configured as the public interface with NAT enabled.

![Router1 RRAS NAT Interface Configuration](Screenshots/06-RRAS-NAT/Router1-RRAS-NAT-Interface-Configuration.png)

### 8.5 Default Gateway Configuration

The domain controllers were configured to use Router1 (`192.168.50.1`) as their default gateway.

This allows DC1 and DC2 to route traffic destined for external networks through Router1.

![Domain Controllers Router1 Default Gateway](Screenshots/06-RRAS-NAT/Domain-Controllers-Router1-Default-Gateway.png)

### 8.6 Internet Connectivity Verification

Internet connectivity was tested from an internal Windows client after RRAS and NAT configuration.

Successful connectivity confirmed that internal network traffic could reach external networks through Router1.

![Client Internet Connectivity Verification](Screenshots/06-RRAS-NAT/Client-Internet-Connectivity-Verification.png)

### 8.7 Outcome

The lab demonstrates how a Windows Server can function as a router and NAT gateway, allowing devices on an isolated internal network to access external networks through a separate WAN connection.

## 9. VPN and Network Policy Server (NPS)

A remote-access VPN solution was configured using Windows Server RRAS and Network Policy Server (NPS). RADIUS authentication was implemented to authenticate domain users and control VPN access based on Active Directory group membership.

### 9.1 Router1 Domain Membership

Router1 was joined to the `ad.lab` domain so that it could communicate with the domain controllers and use domain-based authentication services.

![Router1 Domain Join](Screenshots/07-VPN-NPS/Router1-Domain-Join.png)

### 9.2 NPS Role Installation

The Network Policy Server (NPS) role was installed on both DC1 and DC2.

NPS was registered in Active Directory so that the servers could access domain user account and group information during authentication.

![NPS Installation on DC1 and DC2](Screenshots/07-VPN-NPS/NPS-Installation-DC1-DC2.png)

### 9.3 VPN Security Group

An Active Directory security group named `IT-VPN-Users` was created to manage which domain users were authorized to connect to the VPN.

This allows VPN access to be controlled through group membership rather than configuring access individually for each user.

![IT VPN Users Group](Screenshots/07-VPN-NPS/AD-IT-VPN-Users-Group.png)

### 9.4 RADIUS Client Configuration

Router1 was configured as a RADIUS client on both NPS servers.

The RADIUS client configuration allows Router1 to forward VPN authentication requests to DC1 and DC2.

![DC1 NPS Router1 RADIUS Client](Screenshots/07-VPN-NPS/DC1-NPS-Router1-RADIUS-Client.png)

![DC2 NPS Router1 RADIUS Client](Screenshots/07-VPN-NPS/DC2-NPS-Router1-RADIUS-Client.png)

### 9.5 VPN Network Policy

A network policy named `Allow-IT-VPN-Users` was configured on both NPS servers.

The policy uses Active Directory group membership to authorize VPN access for users belonging to `IT-VPN-Users`.

![DC1 NPS VPN Network Policy](Screenshots/07-VPN-NPS/DC1-NPS-VPN-Network-Policy.png)

![DC2 NPS VPN Network Policy](Screenshots/07-VPN-NPS/DC2-NPS-VPN-Network-Policy.png)

### 9.6 Router1 RADIUS Authentication Configuration

Router1 was configured to use the NPS servers as its RADIUS authentication servers.

This enables VPN authentication requests to be processed through the configured NPS policies.

![Router1 RADIUS Authentication Servers](Screenshots/07-VPN-NPS/Router1-RADIUS-Authentication-Servers.png)

### 9.7 VPN Address Pool

A dedicated IP address pool was configured for remote VPN clients.

The configured address range was:

`192.168.60.10–192.168.60.20`

This separates VPN client addresses from the internal LAN DHCP scope.

![Router1 VPN Address Pool](Screenshots/07-VPN-NPS/Router1-VPN-Address-Pool.png)

### 9.8 RRAS Remote Access Configuration

RRAS was configured to support remote-access VPN connections.

The IPv4 remote-access configuration and IKEv2 VPN protocol settings were configured on Router1.

![Router1 RRAS IPv4 Remote Access](Screenshots/07-VPN-NPS/Router1-RRAS-IPv4-Remote-Access.png)

![Router1 IKEv2 Miniport Configuration](Screenshots/07-VPN-NPS/Router1-IKEv2-Miniport-Configuration.png)

The IKEv2 service status was checked to verify that the required VPN service was running.

![Router1 IKEv2 Service Status](Screenshots/07-VPN-NPS/Router1-IKEv2-Service-Status.png)

### 9.9 NPS Authentication Method

The VPN network policy was configured to use EAP-MSCHAPv2 for user authentication.

This configuration enables domain credentials to be used during VPN authentication.

![DC1 NPS EAP MSCHAPv2 Configuration](Screenshots/07-VPN-NPS/DC1-NPS-EAP-MSCHAPv2-Configuration.png)

### 9.10 Outcome

The lab demonstrates remote-access VPN configuration using Windows Server RRAS, RADIUS authentication through NPS, and Active Directory group-based authorization.

The next section documents the certificate infrastructure, VPN client configuration, successful connection, and internal resource access.

## 10. Active Directory Certificate Services (AD CS) and VPN Client Connectivity

Active Directory Certificate Services (AD CS) was implemented to provide an internal certificate authority for the lab environment. A certificate was issued to Router1 for the IKEv2 VPN configuration, and the VPN client was configured to trust the lab's root CA certificate.

### 10.1 AD CS Role Installation

The Active Directory Certificate Services role was installed on DC1.

![AD CS Role Installation on DC1](Screenshots/08-Certificates/ADCS-Role-Installation-DC1.png)

### 10.2 Enterprise Certificate Authority Configuration

An Enterprise Root Certificate Authority (CA) named `AD-LAB-CA` was configured on DC1.

The Enterprise CA integrates with Active Directory to publish certificate information and make certificate templates available to domain members.

![Enterprise CA Configuration](Screenshots/08-Certificates/Enterprise-CA-Configuration.png)

### 10.3 Certificate Template Configuration

A certificate template named `Router1-IKEv2` was configured and published through the Certificate Authority.

The template was prepared to allow Router1 to enroll for a certificate suitable for its VPN configuration.

![Certificate Template Management](Screenshots/08-Certificates/Certificate-Template-Management.png)

![Certificate Template Publishing](Screenshots/08-Certificates/Certificate-Template-Publishing.png)

### 10.4 Router1 Certificate Enrollment

Router1 enrolled for a certificate using the `Router1-IKEv2` certificate template.

The issued certificate was used for the Router1 IKEv2 VPN configuration.

![Router1 IKEv2 Certificate Enrollment](Screenshots/08-Certificates/Router1-IKEv2-Certificate-Enrollment.png)

### 10.5 RRAS Certificate Binding

The appropriate certificate was selected and bound to the RRAS VPN configuration on Router1.

![Router1 RRAS SSL Certificate Binding](Screenshots/08-Certificates/Router1-RRAS-SSL-Certificate-Binding.png)

### 10.6 VPN Client Root CA Trust Configuration

The public certificate of the internal root CA was downloaded and installed on the Windows 11 VPN client.

This allows the client to establish trust in the certificate chain presented by Router1 during VPN authentication.

![VPN Client CA Certificate Downloaded](Screenshots/08-Certificates/VPN-Client-CA-Certificate-Downloaded.png)

![VPN Client Trusted Root CA Installation](Screenshots/08-Certificates/VPN-Client-Trusted-Root-CA-Installation.png)

### 10.7 VPN Client Configuration

The Windows 11 VPN client was configured to connect to Router1 using the IKEv2 VPN protocol.

The VPN connection was configured to use the hostname:

`Router1.ad.lab`

A hosts-file entry was added on the VPN client to resolve the hostname to Router1's WAN address:

`192.168.28.184 Router1.ad.lab`

The VPN client used the domain account `AD\alex.morgan` for authentication.

![VPN Client Connection Configuration](Screenshots/07-VPN-NPS/VPN-Client-Connection-Configuration.png)

![VPN Client Hosts File Configuration](Screenshots/07-VPN-NPS/VPN-Client-Hosts-File-Configuration.png)

### 10.8 VPN Authentication and Connection Verification

The VPN connection was tested using the domain user account.

Successful authentication was verified, and the VPN client established an IKEv2 connection to Router1.

![VPN Client Authentication](Screenshots/07-VPN-NPS/VPN-Client-Authentication.png)

![VPN Client Connected](Screenshots/07-VPN-NPS/VPN-Client-Connected.png)

After connecting, the VPN client received an address from the configured VPN address pool.

The assigned VPN address was:

`192.168.60.11`

Connectivity to internal domain resources was tested, including communication with DC1 and access to internal services.

### 10.9 Outcome

The lab demonstrates the deployment of an internal Enterprise Root CA, certificate template configuration, certificate enrollment, root CA trust installation, and IKEv2 VPN connectivity.

It also demonstrates how RRAS, NPS/RADIUS, Active Directory security groups, and certificate-based trust work together to provide authenticated remote access to an internal Windows Server environment.
