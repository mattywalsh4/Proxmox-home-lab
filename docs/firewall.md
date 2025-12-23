# Firewall Design

## Firewall VM

| Property        | Value                          |
|-----------------|--------------------------------|
| OS              | pfSense / OPNsense             |
| VLAN Interfaces | 10, 20, 30, 40, 50             |
| Role            | Routing, firewalling, NAT, VPN |

Firewall interface IPs:

- 192.168.10.1/24
- 192.168.20.1/24
- 192.168.30.1/24
- 192.168.40.1/24
- 192.168.50.1/24

---

## Firewall Model

- Default-deny policy
- Stateful inspection
- All inter-VLAN traffic flows through the firewall VM
- No direct VM-to-VM routing

---

## Firewall Rules (Summary)

### VLAN 10 – Management
- Allow: Management → all VLANs
- Allow: VPN → Management

### VLAN 20 – DNS
- Allow: Any VLAN → DNS (TCP/UDP 53)
- Allow: DNS → Internet

### VLAN 30 – NAS
- Allow: Management → NAS
- Allow: Applications → NAS (restricted ports)

### VLAN 40 – Applications
- Allow: Apps → DNS
- Allow: Apps → Internet
- Allow: Apps → NAS (restricted)

### VLAN 50 – VPN Clients
- Allow: VPN → Admin VM only
- Allow: VPN → Applications
