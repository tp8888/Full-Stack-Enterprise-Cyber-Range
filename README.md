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
* **Enterprise Environment:** Windows Server 2019, Active Directory

## 📂 Project Documentation & Build Phases
The deployment of this cyber range is documented in phases. Click on any phase below to view the detailed configuration guides, firewall rulesets, and deployment steps.

* 📝 **Phase 1:** [Network Topology & Hypervisor Setup](docs/Phase-1-Network-Topology/Phase-1-Network-Topology.md)
* 🛡️ **Phase 2:** [pfSense Firewall Installation](docs/Phase-2-pfSense-Setup/Phase-2-pfSense-Setup.md)
* ⚔️ **Phase 3:** [Offensive Security Zone (Kali Linux)](docs/Phase-3-Kali-Linux-Setup/Phase-3-Kali-Linux-Setup.md)
* 🧱 **Phase 4:** [Egress Filtering & Network Segmentation](docs/Phase-4-Firewall-Configuration/Phase-4-Firewall-Configuration.md)
* 🎯 **Phase 5:** [Cyber Range Setup (Vulnerable Targets)](docs/Phase-5-Cyber-Range-Setup/Phase-5-Cyber-Range-Setup.md)
* 🏢 **Phase 6:** [Active Directory Forest Deployment](docs/Phase-6-Active-Directory/Phase-6-Active-Directory-Deployment.md)
  * ↳ *Sub-Task:* [GPO Validation: Control Panel Restriction](docs/Phase-6-Active-Directory/AD-GPO-Validation-Control-Panel.md)
* 🔓 **Phase 7:** [Vulnerability Modeling & Lab Hardening](docs/Phase-7-Vulnerability-Modeling/Phase-7-Vulnerability-Modeling.md)
* 🦠 **Phase 8:** [Malware Analysis Sandbox & Secure Detonation](docs/Phase-8-Malware-Sandbox/Phase-8-Malware-Sandbox.md)
* 🔎 **Phase 9:** [Digital Forensics & Incident Response (DFIR)](docs/Phase-9-DFIR-Environment/Phase-9-DFIR-Environment.md)
* 📊 **Phase 10:** [Splunk SIEM Deployment & Telemetry](docs/Phase-10-SIEM-Deployment/Phase-10-SIEM-Deployment.md)


# Phase 1: Network Topology & Hypervisor Setup

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To establish the foundational hypervisor environment and map out the virtualized network segmentation required for a Zero Trust enterprise architecture.

## 🗺️ Lab Topology Diagram
The following diagram illustrates the logical separation of the lab zones, all orchestrated by the pfSense firewall.

```mermaid
flowchart TD
    %% ===== Internet & Firewall =====
    WAN["🌐 WAN / Internet"]
    FW["🛡️ pfSense Firewall"]

    %% ===== Red Team / Offensive Zone =====
    KALI["🐧 Kali Linux Offensive Box"]

    %% ===== AD Lab / Corporate Environment =====
    DC["🖥️ Windows Server 2019 DC"]
    W10_1["💻 Win10 Enterprise VM1"]
    W10_2["💻 Win10 Enterprise VM2"]

    %% ===== Blue Team / Security Zone =====
    SIEM["📊 Splunk SIEM"]
    MGMT["🖧 Management Console"]

    %% ===== Malware Sandbox / Isolated Zone =====
    DET["☠️ Malware Analysis Sandbox"]

    %% ===== Network Flows =====
    WAN --> FW
    FW --> KALI
    FW --> DC
    FW --> W10_1
    FW --> W10_2
    FW --> SIEM
    FW --> MGMT
    FW -.-> DET

    %% ===== Attack / Monitoring Arrows =====
    KALI -. Attack --> DC
    KALI -. Attack --> W10_1
    DC --> SIEM
    W10_1 --> SIEM
    W10_2 --> SIEM
    DET -. Malware --> SIEM

    %% ===== Styling =====
    classDef external fill:#d1e7ff,stroke:#339,stroke-width:1px;
    classDef firewall fill:#ffdb99,stroke:#b58900,stroke-width:2px;
    classDef redteam fill:#ffe0e0,stroke:#d33,stroke-width:1px;
    classDef adlab fill:#fff0f5,stroke:#d33,stroke-width:1px;
    classDef blueteam fill:#e0ffe0,stroke:#3a3,stroke-width:1px;
    classDef malware fill:#f5f5f5,stroke:#999,stroke-width:1px,dasharray:5 5;

    class WAN external;
    class FW firewall;
    class KALI redteam;
    class DC,W10_1,W10_2 adlab;
    class SIEM,MGMT blueteam;
    class DET malware;
