# Security Model

Security is a core design principle of this lab.

The environment applies the following principles:

- Least privilege
- Strong network segmentation
- No flat networks
- No direct admin access
- Default-deny firewalling
- Defense in depth

Administrative access is intentionally restricted and audited through a
VPN-based jump host model.

Even internal services must traverse the firewall to communicate, providing
visibility and control over all traffic flows.
