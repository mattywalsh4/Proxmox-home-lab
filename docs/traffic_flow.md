# Traffic Flow Examples

This document illustrates how traffic moves through the environment and how
network segmentation and firewall enforcement are applied in practice.

All traffic between VLANs is explicitly routed and filtered by the firewall VM.
There is no direct Layer 2 or Layer 3 communication between virtual machines.

---

## Application → NAS Access

### Scenario
An application running in the Applications VLAN requires access to shared storage
hosted on the NAS VLAN.

### Traffic Flow

Application VM (192.168.40.x)
→ Firewall (VLAN 40 interface)
→ Firewall policy evaluation
→ NAS VM (192.168.30.2)


### Enforcement
- Traffic originates in VLAN 40
- Inter-VLAN routing occurs only at the firewall
- Firewall rules restrict access to required protocols and ports only
- No direct VM-to-VM connectivity exists

### Security Outcome
- Storage access is tightly controlled
- Lateral movement between application workloads is prevented
- NAS services are not exposed outside approved flows

---

## VPN Client → Infrastructure Access

### Scenario
An administrator connects remotely to manage the Proxmox environment.

### Traffic Flow

Admin PC
→ VPN tunnel
→ VLAN 50 (VPN Clients)
→ Firewall policy evaluation
→ Admin VM (192.168.10.10)
→ Proxmox / AD / Internal Services


### Enforcement
- VPN clients are placed into a dedicated VLAN
- VPN clients cannot directly access infrastructure services
- Access is restricted to the admin jump host only
- Additional authentication and logging occurs on the admin VM

### Security Outcome
- Infrastructure services are never exposed directly to VPN clients
- Administrative access is centralized and auditable
- Compromise of a VPN client does not grant lateral access

---

## Design Principles Demonstrated

These traffic flows demonstrate the core security principles of the lab:

- Default-deny networking
- Explicit allow rules
- Centralized inspection and routing
- No implicit trust between workloads

Every permitted flow is intentional, and enforced by policy.
