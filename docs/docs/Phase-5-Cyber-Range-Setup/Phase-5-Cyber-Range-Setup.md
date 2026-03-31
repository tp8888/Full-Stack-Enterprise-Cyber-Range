# Phase 5: Cyber Range Setup (Vulnerable Targets)

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To populate the `CYBER_RANGE` VLAN with intentionally vulnerable target machines (such as Metasploitable 2) to serve as practice targets for penetration testing and exploit development from the Kali Linux management node.

## ⚙️ Target Provisioning
Vulnerable virtual machines (imported via `.vmdk` and `.ova` formats) were deployed into Oracle VirtualBox and assigned strictly to the isolated offensive network.

* **Network Adapter:** Attached to `Internal Network` -> `CYBER_RANGE` (Maps to pfSense `vtnet2`)

## 🌐 Network Verification
Once the vulnerable VMs were booted, their connectivity was verified using the central pfSense router:
1. **DHCP Leases:** Confirmed via `Status -> DHCP Leases` that the targets successfully pulled IPs in the `10.0.1.x` range.
2. **Ping Sweep:** Verified connectivity by successfully pinging the target IPs directly from the Kali Linux VM.

*(Note: The strict egress firewall rules established in Phase 4 ensure that even if these intentionally vulnerable machines are fully compromised during a lab exercise, malicious actors cannot use them to pivot into the management, corporate, or SIEM networks).*
