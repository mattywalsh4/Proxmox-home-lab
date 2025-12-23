# Automation & Resilience

Automation is a core objective of this lab.

## Ansible

Ansible is used for:

- VM provisioning and rebuilds
- Firewall rule reapplication
- Configuration restoration

The goal is to support **repeatable recovery** and minimize manual intervention.

---

## Proxmox HA

Proxmox High Availability is enabled for critical services:

- Firewall
- Management VM
- Active Directory / identity services

On node failure, services are automatically restarted on available cluster nodes.

All configuration is stored in Git and treated as the source of truth.
