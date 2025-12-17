# 🖥️ Proxmox Raspberry Pi Cluster Home Lab

An enterprise‑style **Proxmox VE home lab** built on Raspberry Pi hardware, focusing on **security, network segmentation, resilience, and automation**.

This project documents the full architecture, design decisions, and operational model for a small but robust virtualization environment.

---

## 🎯 Purpose & Goals

This home lab is designed to:

* Run **virtualized infrastructure services** on low‑power hardware
* Enforce **strong network isolation** using VLANs
* Centralize routing, firewalling, and VPN access
* Support **automated rebuilds and recovery**
* Use a dedicated **admin jump host** for management access

The overall design mirrors **enterprise best practices**, scaled appropriately for Raspberry Pi hardware.

---

## 🧱 Physical Hardware Design

### Hardware Components

| Device           | Role                                    |
| ---------------- | --------------------------------------- |
| Raspberry Pi 5   | Primary Proxmox compute node            |
| Raspberry Pi 4   | Secondary Proxmox compute / backup node |
| Raspberry Pi 400 | Management / lightweight workload node  |
| Home Router      | Internet gateway                        |
| Admin PC         | External administration via VPN         |

All Raspberry Pis connect to the home router via **eth0 or wlan0**.
No managed switch is required — VLANs exist **only virtually**.

---

## ⚙️ Virtualization Layer

Each Raspberry Pi runs **Proxmox VE 8.1** and participates in a single Proxmox cluster.

### Proxmox Cluster Roles

| Node   | Primary Role                    |
| ------ | ------------------------------- |
| Pi 5   | Firewall, Active Directory, DNS |
| Pi 4   | NAS, Application workloads      |
| Pi 400 | Management VM, monitoring       |

**Proxmox HA** is enabled for critical VMs:

* Firewall
* Management VM
* Active Directory

---

## 🌐 Virtual Networking Overview

* Networking is fully virtualized using **Proxmox Linux bridges**
* **VLAN tagging** is used internally
* A **dedicated firewall VM** handles all routing and security
* No VLAN configuration exists on the physical network

---

## 🧩 VLAN Architecture & IP Addressing

Each VLAN uses a `/24` subnet.

| VLAN ID | Purpose       | Subnet          | Gateway      |
| ------- | ------------- | --------------- | ------------ |
| 10      | Management    | 192.168.10.0/24 | 192.168.10.1 |
| 20      | DNS / Pi‑hole | 192.168.20.0/24 | 192.168.20.1 |
| 30      | NAS / Storage | 192.168.30.0/24 | 192.168.30.1 |
| 40      | Applications  | 192.168.40.0/24 | 192.168.40.1 |
| 50      | VPN Clients   | 192.168.50.0/24 | 192.168.50.1 |

All VMs use their VLAN gateway as the default route.

---

## 🖥️ Core Virtual Machines

### Firewall / Router VM

| Property        | Value                          |
| --------------- | ------------------------------ |
| OS              | pfSense / OPNsense             |
| VLAN Interfaces | 10, 20, 30, 40, 50             |
| Role            | Routing, firewalling, NAT, VPN |

Firewall interface IPs:

* 192.168.10.1/24
* 192.168.20.1/24
* 192.168.30.1/24
* 192.168.40.1/24
* 192.168.50.1/24

---

### Management / Admin VM

| Property | Value                             |
| -------- | --------------------------------- |
| VLAN     | 10                                |
| IP       | 192.168.10.10/24                  |
| Purpose  | Admin jump host, SSH, Proxmox GUI |

This is the **only VM directly accessed from VPN**.

---

### Active Directory VM

| Property | Value                     |
| -------- | ------------------------- |
| VLAN     | 10                        |
| IP       | 192.168.10.2/24           |
| Role     | Identity & authentication |

---

### Pi‑hole / DNS VM

| Property | Value               |
| -------- | ------------------- |
| VLAN     | 20                  |
| IP       | 192.168.20.2/24     |
| Role     | DNS (optional DHCP) |

---

### NAS VM

| Property | Value           |
| -------- | --------------- |
| VLAN     | 30              |
| IP       | 192.168.30.2/24 |
| Role     | File storage    |

---

### Application VMs

| Property | Value                         |
| -------- | ----------------------------- |
| VLAN     | 40                            |
| IP Range | 192.168.40.10–50/24           |
| Role     | Docker & application services |

---

## 🔥 Firewall Design

### Firewall Model

* Default‑deny policy
* Stateful inspection
* All inter‑VLAN traffic flows through firewall VM
* No direct VM‑to‑VM routing

### Firewall Rules (Summary)

**VLAN 10 – Management**

* Allow: Management → all VLANs
* Allow: VPN → Management

**VLAN 20 – DNS**

* Allow: Any VLAN → DNS (TCP/UDP 53)
* Allow: DNS → Internet

**VLAN 30 – NAS**

* Allow: Management → NAS
* Allow: Applications → NAS (restricted ports)

**VLAN 40 – Applications**

* Allow: Apps → DNS
* Allow: Apps → Internet
* Allow: Apps → NAS (restricted)

**VLAN 50 – VPN Clients**

* Allow: VPN → Admin VM only
* Allow: VPN → Applications

---

## 🔐 VPN Design

* VPN runs on the firewall VM
* VPN clients receive IPs from `192.168.50.0/24`
* VPN clients **cannot directly access infrastructure VMs**

### Admin Access Flow

```
Admin PC
  ↓ VPN
VLAN 50
  ↓ Firewall
Admin VM
  ↓
Proxmox / AD / Internal Systems
```

---

## 🚦 Traffic Flow Examples

**Application → NAS**

```
App (192.168.40.x)
 → Firewall
 → NAS (192.168.30.2)
```

**VPN → Proxmox**

```
Admin PC
 → VPN (192.168.50.x)
 → Admin VM (192.168.10.10)
```

---

## 🤖 Automation & Resilience

* **Ansible** used for:

  * VM rebuilds
  * Firewall rule reapplication
  * Configuration restores

* **Proxmox HA**:

  * Automatically restarts Firewall, DNS, and AD on node failure

* All configuration stored in **Git**

---

## 🛡️ Security Principles Applied

* Least privilege
* Strong network segmentation
* No flat networks
* No direct admin access
* Default‑deny firewalling
* Defense in depth

---

## ✅ Final Architecture Summary

This home lab provides:

* Fully virtualized VLAN architecture
* Centralized firewall and VPN
* Secure admin jump host model
* Proxmox HA with automation
* Enterprise‑grade security on Raspberry Pi hardware

---

> **Status:** Design complete · Implementation in progress
