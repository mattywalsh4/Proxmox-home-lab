# Physical Hardware Design

## Hardware Components

| Device           | Role                                    |
|------------------|------------------------------------------|
| Raspberry Pi 5   | Primary Proxmox compute node              |
| Raspberry Pi 4   | Secondary Proxmox compute / backup node   |
| Raspberry Pi 400 | Management / lightweight workload node    |
| Home Router      | Internet gateway                          |
| Admin PC         | External administration via VPN           |

Each Raspberry Pi runs Proxmox VE and participates in a single Proxmox cluster.

All devices connect to the home router using standard Ethernet or Wi-Fi.
No managed switch is required — VLANs are implemented **entirely within Proxmox**.

This keeps the physical network simple while allowing strong logical segmentation.
