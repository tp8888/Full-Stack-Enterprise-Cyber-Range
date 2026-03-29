# Phase 6: Active Directory Forest Deployment

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To build a realistic, simulated corporate network environment within the `AD_LAB` VLAN, complete with a Windows Server Domain Controller and Windows 10 Enterprise client endpoints. This zone is designed for practicing Active Directory attacks, privilege escalation, and lateral movement.

## 🏗️ Domain Controller Setup (Windows Server 2019)
A Windows Server 2019 evaluation instance was deployed and promoted to the primary Domain Controller (DC) for the simulated corporate environment.

* **Network Assignment:** Attached to `Internal Network` -> `AD_LAB` (Maps to pfSense `vtnet3`)
* **Static Routing:** Configured to route traffic exclusively through the pfSense `AD_LAB` gateway (`10.0.2.1`).

### Core Enterprise Services Configured:
1. **Active Directory Domain Services (AD DS):** Deployed to establish the core domain structure, users, and organizational units (OUs).
2. **DNS & DHCP:** Configured the DC to handle local DNS forwarding and assign DHCP leases (with extended 365-day durations) to all client endpoints.
3. **Active Directory Certificate Services (AD CS):** Installed and configured the Certificate Authority role to replicate an enterprise PKI environment (which enables future AD CS exploitation labs).

## 💻 Client Endpoint Provisioning
Multiple Windows 10 Enterprise evaluation machines were deployed to the `AD_LAB` subnet and successfully joined to the domain. 

* **Snapshot Strategy:** Clean base snapshots were captured in VirtualBox immediately after domain-joining. This allows the environment to be instantly rolled back to a clean state after destructive penetration testing or configuration changes.
