# Phase 3: Offensive Security Zone (Kali Linux Deployment)

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To deploy and configure Kali Linux within the isolated `CYBER_RANGE` VLAN, establishing a dedicated machine for vulnerability scanning, penetration testing, and offensive security operations.

## ⚙️ Virtual Machine Configuration
A fresh Kali Linux instance was imported into Oracle VirtualBox with the following network configuration to ensure it sits strictly behind the pfSense perimeter:

* **Network Adapter:** Attached to `Internal Network`
* **Name:** `CYBER_RANGE` (Maps to pfSense `vtnet2`)

## 🌐 Network Verification
Upon boot, Kali Linux successfully requested and received a DHCP lease from the pfSense router for the `CYBER_RANGE` subnet (`10.0.1.x`). 

Initial connectivity tests (pinging the `10.0.1.1` gateway and external DNS servers) were conducted to verify routing functionality before proceeding to strict firewall lockdowns in the next phase.

## 🛠️ Baseline Tooling
The system was updated and upgraded (`sudo apt update && sudo apt upgrade -y`) to ensure all native offensive tools (Nmap, Metasploit, Wireshark, Burp Suite) were running their latest stable versions for upcoming lab exercises.
