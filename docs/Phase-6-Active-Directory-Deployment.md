# 🏗️ Phase 6 & 7: Active Directory Forest Deployment & Vulnerability Modeling

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To build a realistic, simulated corporate network environment within the `AD_LAB` VLAN, complete with a Windows Server Domain Controller and Windows 10 Enterprise client endpoints. This zone is designed for practicing Active Directory attacks, privilege escalation, and lateral movement.

---

## 🛠️ Domain Controller Setup (Windows Server 2019)
A Windows Server 2019 evaluation instance was deployed and promoted to the primary Domain Controller (DC) for the simulated corporate environment.

* **Domain Name:** `ad.lab`
* **Network Assignment:** Attached to `Internal Network` -> `AD_LAB` (Maps to pfSense `vtnet3`).
* **Static Routing:** Configured with a static IP (`10.80.80.2`) to route traffic exclusively through the pfSense `AD_LAB` gateway (`10.80.80.1`).

### Core Enterprise Services Configured:
1. **AD DS (Active Directory Domain Services):** Established the core domain structure, users, and organizational units (OUs).
2. **DNS & DHCP:** Configured the DC to handle local DNS forwarding and assign DHCP leases to all client endpoints.
3. **AD CS (Active Directory Certificate Services):** Installed the Certificate Authority role to replicate an enterprise PKI environment, enabling future AD CS exploitation labs.

---

## 💻 Client Endpoint Provisioning
Multiple Windows 10 Enterprise evaluation machines were deployed to the `AD_LAB` subnet and successfully joined to the `ad.lab` domain. 

* **GPO Pipeline Validation:** To verify the AD pipeline, a GPO was implemented to prohibit access to the Control Panel. Success was validated using `gpupdate /force` on the endpoints.
* **Snapshot Strategy:** Clean base snapshots were captured in VirtualBox immediately after domain-joining. This allows the environment to be instantly rolled back after destructive penetration testing.

---

## 🛡️ Vulnerability Modeling & Lab Hardening
To simulate a realistic attack surface, the following "insecure" configurations and maintenance protocols were established:

### 1. Deliberate Weaknesses
* **Weak Password Policies:** Reduced complexity requirements to allow for password spraying and brute-force simulations.
* **LLMNR/NBT-NS Enabled:** Kept active to practice NetBIOS/LLMNR spoofing with tools like **Responder**.
* **Kerberoasting Lab:** Created a dedicated service account with a Service Principal Name (SPN) and a weak password for offline ticket cracking.

### 2. Technical Troubleshooting & Maintenance
During deployment, several infrastructure hurdles were documented and resolved:

* **Trust Relationship Failures:** Resolved "Secure Channel" desynchronization (often caused by snapshot reverts) using the following PowerShell repair command on the client:
  ```powershell
  Test-ComputerSecureChannel -Repair -Credential (Get-Credential)
