# Automation & Resilience

Automation is a core objective of this lab. All infrastructure is designed to be
**reprovisioned, rebuilt, or recovered** with minimal manual intervention.

Automation is treated as a first-class component of the architecture.

---

## Ansible

Ansible is used as the primary configuration and provisioning tool.

Ansible is responsible for:

- Virtual machine creation and rebuilds
- Proxmox API interaction
- Firewall and system configuration
- Post-install configuration tasks

Playbooks are written to be:
- Idempotent
- Repeatable
- Safe to re-run

All infrastructure definitions are stored in Git and treated as the source of truth.

---

## Jenkins (Automation Orchestration)

Jenkins is used to **orchestrate Ansible workflows** and provide controlled,
repeatable execution of automation tasks.

Jenkins is responsible for:

- Triggering Ansible playbooks
- Running scheduled or manual rebuild jobs
- Providing execution history and visibility
- Acting as a controlled automation entry point

### Jenkins Execution Model

- Jenkins runs on a dedicated management system
- Jobs execute Ansible playbooks
- Credentials and secrets are injected securely at runtime
- No direct infrastructure changes occur outside automation pipelines

---

## Example Jenkins Use Cases

- Scheduled validation of infrastructure state
- One-click rebuild of a failed VM
- Controlled execution of firewall rule updates
- Automated recovery testing

This approach ensures that **all changes are intentional, repeatable, and auditable**.

---

## Proxmox High Availability

Proxmox HA is enabled for critical virtual machines:

- Firewall
- Management VM
- Active Directory / identity services

On node failure, Proxmox automatically restarts services on available cluster nodes,
reducing downtime without manual intervention.

---

## Automation Philosophy

The automation design follows these principles:

- Infrastructure as Code
- Minimal manual configuration
- Clear separation between design and execution
- Recovery-first mindset

