# Phase 2: pfSense Firewall Installation & Baseline Configuration

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
Deploy pfSense (FreeBSD) as the central router and perimeter firewall for the enterprise environment. Map the VirtualBox internal networks to physical pfSense interfaces and assign static IP gateways.

## ⚙️ Interface Assignments
During the initial pfSense boot, the virtual MAC addresses from VirtualBox were mapped to the corresponding pfSense `vtnet` interfaces.

### Subnet Allocations
Each isolated network was assigned a dedicated IPv4 subnet space, and pfSense was configured to act as the `.1` Gateway for each zone.

* **WAN:** DHCP (Leased from Host)
* **LAN:** `10.0.0.1/24`
* **CYBER_RANGE:** `10.0.1.1/24`
* **AD_LAB:** `10.0.2.1/24`
* **ISOLATED:** `10.0.3.1/24`
* **SECURITY:** `10.0.4.1/24`

## 🔒 Baseline Ruleset
To allow initial configuration via the web GUI from a management machine, the following baseline rules were established:

1. **Anti-Lockout Rule:** Port 80/443 access granted to the WebGUI on the `LAN` interface.
2. **Temporary Egress:** A temporary "Allow All to Anywhere" IPv4 rule was placed on the internal subnets to verify baseline connectivity and DNS resolution before enforcing strict Zero Trust egress filtering.

*(Note: In future phases, this temporary "Allow All" rule will be removed and replaced with explicit Allow/Deny parameters).*
