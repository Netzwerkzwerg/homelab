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
- The web UI is published on TCP port 8000 of the Docker host.
- The stack consists of the NetBox web application, a background worker, PostgreSQL, and Valkey services for queues and caching.
- All stack containers were verified healthy after initial database migrations.
- The internal DNS domain is `home.arpa`.
- Exact host addresses, hostnames, credentials, and detailed inventory are intentionally not documented in this public repository.

## Network security and discovery

- Deny inter-VLAN traffic by default where explicitly configured; permit only documented requirements.
- VLAN10 is intentionally treated as a privileged infrastructure network. Its IPv4 pass-to-any rule remains in place; access from other VLANs is restricted by rules on their source interfaces.
- VLAN50 administrator clients may access internal networks. Other VLAN50 clients are blocked from internal networks but retain Internet access.
- VLANs 20, 30, and 40 have baseline rules for DNS/DHCP, blocking internal destinations, and allowing outbound access.
- An OPNsense mDNS Repeater is enabled for VLANs 30, 40, 50, and 90. Firewall rules permit IPv4 UDP mDNS traffic to 224.0.0.251:5353 on those interfaces.
- mDNS discovery is separate from permission to control or stream to Cast devices. Actual cross-VLAN casting remains untested because Cast receivers have not yet been deployed.
- VLAN40 is not granted a broad Cast-device exception. A future Companion-to-Cast rule should be limited to the Companion host and the Cast device alias.
- Captive Portal for VLAN95 is deferred until the public domain, portal URLs, and certificate approach are decided.

See [NETWORK.md](NETWORK.md) for VLAN intent, current firewall state, and open validation work; see [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise project status.
