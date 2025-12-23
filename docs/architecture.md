# Architecture Overview

This home lab is designed to mirror **enterprise infrastructure principles** while remaining
practical for Raspberry Pi hardware and a home environment.

The architecture prioritizes:

- Security first
- Strong network segmentation
- Centralized control and visibility
- Automated rebuild and recovery
- Minimal physical complexity

All infrastructure services are virtualized using **Proxmox VE**, with routing and security
centralized through a dedicated firewall virtual machine.

The design intentionally avoids flat networks and direct administrative access, instead
using a **VPN + admin jump host model** similar to enterprise environments.

While the scale is small, the architectural patterns reflect real-world production designs.
