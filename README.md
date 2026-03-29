# Full-Stack-Enterprise-Cyber-Range
Cybersecurity testing and research lab

# Enterprise Virtualized Network Perimeter & Cyber Range

## 📌 Project Overview
This project documents the design, configuration, and deployment of a segmented enterprise network environment using **pfSense** as the primary perimeter firewall and router. The architecture was built entirely within Oracle VirtualBox to simulate a realistic corporate network, complete with isolated zones for Active Directory, Malware Analysis, Incident Response, and Penetration Testing.

* **Credit/Inspiration:** [Insert Link to the writeup you followed]

## 🛠️ Core Technologies Used
* **Hypervisor:** Oracle VirtualBox
* **Firewall/Router:** pfSense (FreeBSD)
* **Offensive Security:** Kali Linux
* **Defensive/Analysis:** Splunk, FlareVM, REMnux, Windows Server, Tsurugi Linux

## 🗺️ Network Topology & IP Addressing
The environment is segmented into six distinct interfaces managed by the pfSense firewall. Each zone operates on its own dedicated subnet to ensure strict traffic control and isolation, preventing unauthorized lateral movement.

| Interface | Zone Name | IP Subnet | Gateway (pfSense) | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **WAN** (`vtnet0`) | External | DHCP | N/A | Simulated internet connection (NAT to Host). |
| **LAN** (`vtnet1`) | Management | `10.0.0.0/24` | `10.0.0.1` | Firewall administration and core management. |
| **CYBER_RANGE** (`vtnet2`) | Offensive | `10.0.1.0/24` | `10.0.1.1` | Penetration testing tools and attack origination (Kali Linux). |
| **AD_LAB** (`vtnet3`) | Corporate | `10.0.2.0/24` | `10.0.2.1` | Windows Server Active Directory forest and domain-joined endpoints. |
| **ISOLATED** (`vtnet4`) | Sandbox | `10.0.3.0/24` | `10.0.3.1` | Air-gapped malware detonation and reverse engineering (FlareVM, REMnux). |
| **SECURITY** (`vtnet5`) | SIEM / DFIR | `10.0.4.0/24` | `10.0.4.1` | Log ingestion, monitoring, and forensics (Splunk, Tsurugi). |

## 🔒 Access Control & Firewall Methodology
By default, pfSense implicitly denies all inbound and outbound traffic. To construct a functional but secure environment, custom firewall rules were engineered for each interface following a **Zero Trust** model.

*(Detailed rulesets for Egress, Ingress, and Inter-VLAN routing will be documented below).*
