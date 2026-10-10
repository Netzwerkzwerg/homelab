# Architecture

## Target design

A NiPoGi N100 mini PC runs Proxmox as the virtualization host. OPNsense is planned as a virtual router/firewall; VLANs will separate trusted clients, IoT, multimedia, and guest devices.

```text
Internet
   |
Dedicated WAN interface
   |
[ Proxmox: OPNsense VM ] ---- VLAN trunk ---- [ Switches / UniFi U7 Lite ]
   |                                             |
   +-- planned services and VMs/containers       +-- VLAN-specific clients
```

This is a target concept, not an implemented topology. Physical interface mapping, switch ports, and recovery procedures remain open.

## Responsibilities

- **Proxmox:** host for virtual machines and containers.
- **OPNsense (planned):** routing, inter-VLAN firewalling, Internet access control, and narrowly scoped service discovery if needed.
- **Home Assistant (planned):** central smart-home platform.
- **NetBox (planned):** infrastructure inventory and IPAM after migration from the current private table.

Do not split services into separate VMs or containers without a clear security, reliability, maintenance, or resource-management benefit.

## Design principles

- Deny inter-VLAN traffic by default; allow only documented requirements.
- Keep IoT and guest devices separated from trusted clients.
- Treat service discovery (such as mDNS) separately from permission to communicate.
- Prefer simple, maintainable designs and document consequential decisions.
- Plan for the fact that a virtual firewall depends on the Proxmox host being operational.

See [NETWORK.md](NETWORK.md) for VLAN intent and [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for status and open decisions.
