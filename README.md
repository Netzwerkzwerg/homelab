# Homelab

Short, AI-readable documentation for the homelab's current state and target design.

## Documents

- [Project status and decisions](PROJECT_CONTEXT.md) — what is configured, planned, and still open.
- [Network design](NETWORK.md) — VLAN purposes, deployment status, and intended access policy.
- [Architecture](ARCHITECTURE.md) — Proxmox, OPNsense, UniFi, services, and design principles.
- [AI working context](AI_CONTEXT.md) — how assistants should use and update this repository.
- [Access test](TEST_ACCESS.md) — public test file for checking whether external AI assistants can read the repository.

## Current state

Proxmox and OPNsense are running. UniFi Network Application is deployed in a Debian LXC and the U7 Lite access point is adopted and online. Seven VLANs have been created; the management, IoT, and multimedia networks are operational, while full functional validation of all seven remains in progress. Inter-VLAN firewall rules and cross-VLAN Cast device discovery still need to be implemented and tested.

All documentation is in English. Do not publish real IP assignments, credentials, or other sensitive operational details.
