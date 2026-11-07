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
   |                                           +-- VLAN-specific clients
   |
   +-- [ Debian Docker VM ] -- NetBox and other self-hosted services
```

The UniFi Network Application runs in a Debian LXC. The U7 Lite is adopted, updated, and online. A separate Debian VM in the Services VLAN runs Docker Engine and Docker Compose. NetBox is deployed there.

## Responsibilities

- **Proxmox:** virtualization host for VMs and containers.
- **OPNsense:** routing, inter-VLAN firewalling, Internet access control, DHCP, DNS, and mDNS repeating.
- **UniFi Network Application:** management of the UniFi access point and WLAN configuration.
- **Docker VM:** runtime for containerized self-hosted applications.
- **NetBox:** infrastructure documentation and IP address management (IPAM); detailed inventory belongs here rather than in this public repository.
- **Home Assistant (planned):** central smart-home platform.

## NetBox deployment

- NetBox runs as a Docker Compose stack on a Debian VM in VLAN40 (Services).
- The web UI is published on TCP port **8080** of the Docker host.
- The stack consists of the NetBox web application, a background worker, PostgreSQL, and Valkey services for queues and caching.
- All stack containers were verified healthy after initial database migrations.
- The internal DNS domain is `home.arpa`.
- Exact host addresses, hostnames, credentials, and detailed inventory are intentionally not documented in this public repository.

## Network security and discovery

OPNsense provides inter-VLAN routing and firewalling. The detailed VLAN purposes, current firewall rules, mDNS configuration, planned access exceptions, and outstanding validation are maintained in [NETWORK.md](NETWORK.md), which is the single reference for network policy.

mDNS discovery does not itself authorize Cast control or media traffic. Actual cross-VLAN casting remains untested because Cast receivers have not yet been deployed.

See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise project status.
