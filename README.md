# Homelab

Short, AI-readable documentation for the homelab's current state and target design.

## Documents

- [Project status and decisions](PROJECT_CONTEXT.md) — what is configured, planned, and still open.
- [Network design](NETWORK.md) — VLAN purposes, current firewall policy, and validation work.
- [Architecture](ARCHITECTURE.md) — Proxmox, OPNsense, UniFi, services, and design principles.
- [AI working context](AI_CONTEXT.md) — how assistants should use and update this repository.
- [Access test](TEST_ACCESS.md) — public test file for checking whether external AI assistants can read the repository.

## Current state

Proxmox and OPNsense are running. UniFi Network Application is deployed in a Debian LXC and the U7 Lite access point is adopted and online. Seven VLANs are defined. Baseline firewall rules are configured for VLANs 20, 30, and 40; VLAN50 administrator access is restricted by an administrator-client alias; VLAN10 remains a privileged infrastructure network. An OPNsense mDNS Repeater is enabled for VLANs 30, 40, 50, and 90. Full validation, guest-network policy review, and real Cast testing remain open.

All documentation is in English. Do not publish real IP assignments, credentials, or other sensitive operational details.
