# Phase 9: Digital Forensics & Incident Response (DFIR)

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To deploy **Tsurugi Linux**, a specialized DFIR distribution, into the dedicated `SECURITY` zone. This machine acts as the primary staging area for malware samples and the workstation for conducting post-incident digital forensics.

## ⚙️ Infrastructure & Routing
The `SECURITY` zone was provisioned as the 6th network adapter via the `VBoxManage` CLI and attached to the pfSense router. 

### Static IP Assignment
To ensure reliable SCP transfers when pushing malware to the `ISOLATED` sandbox, Tsurugi Linux requires a static IP. This was configured centrally via the pfSense DHCP Server:
1. Located the Tsurugi Linux MAC address in `Status -> DHCP Leases`.
2. Created a static DHCP mapping, permanently assigning it the IP address `10.10.10.2`.

## 🛠️ Configuration & Snapshotting
Following a full system update (`sudo apt update && sudo apt full-upgrade`), a clean snapshot of the Tsurugi VM was captured. Because this machine handles live malware before pushing it to the sandbox, having a known-good baseline to revert to is a critical operational security measure.
