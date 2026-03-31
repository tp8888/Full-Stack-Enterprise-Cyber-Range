# Phase 6: Active Directory Deployment

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To build a realistic, simulated corporate network environment within the `AD_LAB` VLAN. This infrastructure serves as the primary identity management layer for the `ad.lab` project, providing a target for practicing Active Directory attacks, privilege escalation, and lateral movement.

## 🏗️ Domain Controller Setup (Windows Server 2019)
A Windows Server 2019 instance was deployed and promoted to the primary Domain Controller (DC) for the simulated corporate environment.

* **Domain Name:** `ad.lab`
* **Network Assignment:** Attached to `Internal Network` -> `AD_LAB` (Maps to pfSense `vtnet3`).
* **Static Routing:** Configured with a static IP (`10.80.80.2`) to route traffic exclusively through the pfSense `AD_LAB` gateway (`10.80.80.1`).

### Core Enterprise Services Configured:
1. **AD DS (Active Directory Domain Services):** Established the core domain structure, including Users, Groups, and Organizational Units (OUs).
2. **DNS & DHCP:** Configured the DC to handle local DNS resolution and assign DHCP leases to all client endpoints.
3. **AD CS (Active Directory Certificate Services):** Installed the Certificate Authority role to replicate an enterprise PKI environment, enabling future AD CS exploitation labs.

## 💻 Client Endpoint Provisioning
Windows 10 Enterprise machines were deployed to the `AD_LAB` subnet and successfully joined to the `ad.lab` domain. 

* **GPO Pipeline Validation:** To verify the AD pipeline, a GPO was implemented to prohibit access to the Control Panel. Success was validated using `gpupdate /force` on the endpoints.
* **Snapshot Strategy:** Clean base snapshots were captured in VirtualBox immediately following domain-joining. This allows the environment to be instantly rolled back after destructive penetration testing or configuration changes.
