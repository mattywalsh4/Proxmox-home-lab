# 🖥️ Proxmox Raspberry Pi Cluster Home Lab

An enterprise-style **Proxmox VE home lab** built on Raspberry Pi hardware, focused on
**security, network segmentation, resilience, and automation**.

This project documents the **architecture, design decisions, and operational model**
for a small but robust virtualized environment, scaled appropriately for low-power hardware.

---

## 🎯 Project Goals

- Virtualize infrastructure services on Raspberry Pi hardware
- Enforce strong network isolation using VLANs
- Centralize routing, firewalling, and VPN access
- Use a dedicated admin jump host for secure management
- Support automated rebuilds and recovery using Ansible
- Apply enterprise security principles in a homelab context

---

## 🧱 High-Level Architecture

- Proxmox VE cluster across multiple Raspberry Pi nodes
- Fully virtualized networking using Linux bridges and VLANs
- Firewall-first design using pfSense / OPNsense
- No flat networks — all inter-VLAN traffic routed and filtered
- Admin access only via VPN + jump host

> No physical VLAN configuration is required — all segmentation is virtual.

---

## 📚 Documentation

Detailed design documentation is split into focused sections:

- 📐 [Architecture Overview](docs/architecture.md)
- 🧱 [Hardware Design](docs/hardware.md)
- 🌐 [Networking & VLANs](docs/networking.md)
- 🔥 [Firewall Design](docs/firewall.md)
- 🔐 [Security Model](docs/security.md)
- 🔑 [VPN Design](docs/vpn.md)
- 🤖 [Automation & Resilience](docs/automation.md)
- 🚦 [Traffic Flows](docs/traffic-flows.md)

---

## 🤖 Automation & Status

- Ansible is used for VM provisioning, rebuilds, and configuration recovery
- Proxmox HA is enabled for critical services
- Design is complete; implementation is in progress

> **Status:** Design complete · Implementation in progress 

---

## ⚠️ Disclaimer

This repository documents a **personal learning lab**.
Configurations are simplified for educational purposes and should not be used directly in production.
