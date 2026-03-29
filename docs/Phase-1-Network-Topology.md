# Phase 1: Network Topology & Hypervisor Setup

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To establish the foundational hypervisor environment and map out the virtualized network segmentation required for a Zero Trust enterprise architecture.

## ⚙️ Hypervisor Configuration (Oracle VirtualBox)
The host machine was provisioned with adequate resources to support multiple simultaneous Virtual Machines (VMs). VirtualBox was configured with custom internal networks to act as isolated virtual switches.

### Virtual Network Assignments
To ensure strict traffic control, the following isolated networks were created within VirtualBox. All traffic between these zones is forced through the pfSense perimeter firewall.

| Interface Name | VirtualBox Network Type | pfSense Assignment | Zone Purpose |
| :--- | :--- | :--- | :--- |
| **Adapter 1** | NAT | `WAN` (`vtnet0`) | External Internet Access |
| **Adapter 2** | Internal Network (`LAN`) | `LAN` (`vtnet1`) | Primary Management |
| **Adapter 3** | Internal Network (`CYBER_RANGE`) | `CYBER_RANGE` (`vtnet2`) | Offensive Operations (Kali) |
| **Adapter 4** | Internal Network (`AD_LAB`) | `AD_LAB` (`vtnet3`) | Corporate Environment |
| **Adapter 5** | Internal Network (`ISOLATED`) | `ISOLATED` (`vtnet4`) | Malware Analysis Sandbox |
| **Adapter 6** | Internal Network (`SECURITY`) | `SECURITY` (`vtnet5`) | Defensive Monitoring (SIEM) |

## 🚀 Next Step
With the isolated switches created in the hypervisor, the next phase involves installing the pfSense routing engine to bridge and secure these networks.
