# Architecture

## Current topology

A NiPoGi mini PC runs Proxmox. OPNsense runs as a virtual machine and acts as the router and firewall. Its LAN connection carries the management network natively and the other networks as tagged VLANs. A managed switch connects the Proxmox host and a UniFi U7 Lite access point.

```text
Internet
   |
Dedicated WAN interface
   |
[ Proxmox host ]
   |-- [ OPNsense VM ] ---- VLAN trunk ---- [ Managed switch ]
   |                                           |
   |-- [ UniFi Network Application LXC ]       +-- [ UniFi U7 Lite ]
                                               +-- VLAN-specific clients
```

The UniFi Network Application runs in a Debian LXC. The U7 Lite is adopted, updated, and online.

## Responsibilities

- **Proxmox:** virtualization host for VMs and containers.
- **OPNsense:** routing, inter-VLAN firewalling, Internet access control, DHCP, and DNS.
- **UniFi Network Application:** management of the UniFi access point and WLAN configuration.
- **Home Assistant (planned):** central smart-home platform.
- **NetBox (planned):** infrastructure inventory and IPAM after migration from the current private table.

## Design principles

- Deny inter-VLAN traffic by default; allow only documented requirements.
- Implement and verify explicit firewall rules for all required inter-VLAN flows before considering segmentation complete.
- Keep IoT and guest devices separated from trusted clients.
- Treat Cast device discovery (mDNS/service discovery) separately from permission to communicate. A narrowly scoped cross-VLAN discovery service is still to be selected and configured.
- Prefer simple, maintainable designs and document consequential decisions.
- Plan for the fact that a virtual firewall depends on the Proxmox host being operational.

See [NETWORK.md](NETWORK.md) for VLAN intent and [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for status and open decisions.
