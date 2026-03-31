# Phase 1: Network Topology & Hypervisor Setup

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To establish the foundational hypervisor environment and map out the virtualized network segmentation required for a Zero Trust enterprise architecture.

## 🗺️ Lab Topology Diagram
The following diagram illustrates the logical separation of the lab zones, all orchestrated by the pfSense firewall.

```mermaid
graph TD
    subgraph External_Network
        WAN[WAN / Internet]
    end

    subgraph pfSense_Appliance [pfSense Firewall]
        FW[Routing & Filtering]
    end

    subgraph AD_Zone [AD_LAB - 10.80.80.1/24]
        DC[Windows Server 2019 DC]
        W10_1[Win10 Enterprise VM1]
        W10_2[Win10 Enterprise VM2]
    end

    subgraph Cyber_Zone [CYBER_RANGE - 10.0.2.1/24]
        KALI[Kali Linux / Offensive Box]
    end

    subgraph Malware_Zone [ISOLATED - vtnet4]
        DET[Malware Analysis Sandbox]
    end

    subgraph Mgmt_Zone [LAN - 10.0.1.1/24]
        MGMT[Management Console]
    end

    %% Connectivity
    WAN <--> FW
    FW <--> AD_Zone
    FW <--> Cyber_Zone
    FW <--> Mgmt_Zone
    FW -.-> |Restricted| Malware_Zone
