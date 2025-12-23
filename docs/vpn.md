# VPN Design

The VPN service runs directly on the firewall VM.

- VPN clients receive IPs from `192.168.50.0/24`
- VPN clients cannot directly access infrastructure VMs
- All access is filtered through firewall rules

---

## Admin Access Flow

Admin PC
↓ VPN
VLAN 50
↓ Firewall
Admin VM
↓
Proxmox / AD / Internal Systems
