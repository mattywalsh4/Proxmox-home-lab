# Networking & VLAN Architecture

## Virtual Networking Overview

Networking is fully virtualized using **Proxmox Linux bridges**.

- VLAN tagging is used internally
- No VLAN configuration exists on the physical network
- All routing and inter-VLAN traffic passes through the firewall VM

This design provides strong isolation without requiring advanced physical networking hardware.

---

## VLAN Architecture & IP Addressing

Each VLAN uses a `/24` subnet.

| VLAN ID | Purpose       | Subnet          | Gateway      |
|--------|---------------|-----------------|--------------|
| 10     | Management    | 192.168.10.0/24 | 192.168.10.1 |
| 20     | DNS / Pi-hole | 192.168.20.0/24 | 192.168.20.1 |
| 30     | NAS / Storage | 192.168.30.0/24 | 192.168.30.1 |
| 40     | Applications  | 192.168.40.0/24 | 192.168.40.1 |
| 50     | VPN Clients   | 192.168.50.0/24 | 192.168.50.1 |

All VMs use their VLAN gateway as the default route.
