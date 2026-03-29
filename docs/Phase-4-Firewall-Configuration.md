# Phase 4: Egress Filtering & Network Segmentation (pfSense)

> **Credit & Inspiration:** This architecture is built based on the "Building a Virtual Security Home Lab" blueprint designed by [David Varghese](https://david-varghese.medium.com/).

## 📌 Objective
To enforce Zero Trust network segmentation by replacing the temporary "Allow All" baseline rules with strict egress filtering. This ensures that compromised or offensive zones cannot pivot into corporate or management subnets.

## 🧱 The RFC1918 Alias
To streamline firewall rule creation, an alias named `RFC1918` was created in pfSense. This alias contains all private IPv4 address spaces:
* `10.0.0.0/8`
* `172.16.0.0/12`
* `192.168.0.0/16`

By using this alias, we can write a single rule that says: *"Block traffic attempting to reach ANY other internal network."*

## 🔒 Zone-Specific Rulesets

### 1. LAN (Management)
* **Rule:** Allow all outbound traffic.
* **Logic:** As the management interface, this zone requires unrestricted access to configure the router and access external resources.

### 2. CYBER_RANGE (Offensive Zone)
* **Rule 1 (Allow DNS):** Allow UDP Port 53 to the `CYBER_RANGE` Gateway (`10.0.1.1`).
* **Rule 2 (Block Internal):** Block all traffic destined for the `RFC1918` alias. *(Prevents Kali from scanning or attacking the AD_LAB or SECURITY zones unless explicitly allowed).*
* **Rule 3 (Allow Internet):** Allow all other traffic outbound to the WAN.

*(Note: Similar restrictive logic will be applied to the AD_LAB, ISOLATED, and SECURITY zones as those environments are brought online in subsequent phases.)*
