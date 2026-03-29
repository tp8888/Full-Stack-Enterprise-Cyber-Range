# 🏢 Full-Stack Enterprise Cyber Range

**Cybersecurity testing and research lab**

## 📌 Executive Summary
This repository documents the architecture, deployment, and configuration of a fully virtualized, enterprise-grade Security Operations Center (SOC) and Cyber Range. Built entirely within Oracle VirtualBox, this environment simulates a realistic corporate network with strict segmentation, a dedicated threat hunting SIEM, a vulnerable Active Directory forest, and an air-gapped malware analysis sandbox. 

The primary objective of this project is to provide a safe, segmented environment for advanced penetration testing, digital forensics, incident response (DFIR), and perimeter defense research.

## 🏗️ Network Architecture & Segmentation
To adhere to Zero Trust principles, the network is routed through a central **pfSense** firewall and segmented into distinct Virtual LANs (VLANs), ensuring strict traffic control between trusted and untrusted zones.

* **WAN (`vtnet0`):** External internet connection (NAT).
* **LAN (`vtnet1`):** Primary management network.
* **CYBER_RANGE (`vtnet2`):** Offensive security subnet featuring Kali Linux.
* **AD_LAB (`vtnet3`):** Simulated corporate environment with Windows Server Active Directory.
* **ISOLATED (`vtnet4`):** Air-gapped sandbox for detonation and malware analysis (FlareVM, REMnux).
* **SECURITY (`vtnet5`):** Dedicated defensive zone for security monitoring and SIEM deployment (Splunk).

## 🛠️ Core Technology Stack
* **Infrastructure & Routing:** Oracle VirtualBox, pfSense (FreeBSD)
* **Offensive Security:** Kali Linux
* **Defensive Operations (Blue Team):** Splunk (SIEM)
* **Digital Forensics & Incident Response (DFIR):** Tsurugi Linux
* **Malware Analysis:** FlareVM (Windows), REMnux (Linux)
* **Enterprise Environment:** Windows Server 2022/2019, Active Directory

## 📂 Project Documentation & Build Phases
The deployment of this cyber range is documented in phases. Click on any phase below to view the detailed configuration guides, firewall rulesets, and deployment steps.

* 📝 **Phase 0 & 1:** [Hypervisor Setup & Network Topology](#)
* 🛡️ **Phase 2 & 4:** [pfSense Firewall & Egress Configuration](#)
* ⚔️ **Phase 3 & 5:** [Kali Linux & Cyber Range Deployment](#)
* 🏢 **Phase 6 & 7:** [Active Directory Forest Build](#)
* 🦠 **Phase 8 & 11:** [Malware Analysis Sandbox & Secure File Transfer](#)
* 🔎 **Phase 9:** [Tsurugi Linux DFIR Setup](#)
* 📊 **Phase 10:** [Splunk SIEM Deployment & Telemetry](#)

---
*Disclaimer: This environment is built strictly for educational purposes, research, and defensive security training.*
