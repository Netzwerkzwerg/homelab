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

The selected upstream-router design keeps OPNsense WAN in the normal FRITZ!Box EG home network. The separate FRITZ!Box Büro WAN is to be moved to the isolated LAN guest network of FRITZ!Box EG. A managed/VLAN-capable switch near FRITZ!Box EG and the existing basement switch are intended to carry the home and guest segments separately over the uplink. This cabling and VLAN transport are planned, not yet implemented. The office LAN remains behind FRITZ!Box Büro.

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
- NetBox initial deployment and health checks are complete. Importing the actual infrastructure inventory (sites/locations, devices, interfaces, VLANs, prefixes, and IP assignments) is still pending.

## Network security and discovery

OPNsense provides inter-VLAN routing and firewalling. Its WAN remains on the normal FRITZ!Box EG home network so that selected management clients can reach explicitly allowed upstream infrastructure. Access from OPNsense VLANs to the upstream home subnet is intended to be blocked by default, with narrowly scoped management exceptions. The separate office router is intended to use the FRITZ!Box EG LAN guest network, isolating the office from the upstream home devices. Detailed policy and implementation status are maintained in [NETWORK.md](NETWORK.md).

IPv6 is currently blocked/disabled. A complete, deliberate IPv6 design—including addressing, routing, firewall policy, and validation—is deferred until a later phase; IPv6 must not be treated as implicitly secured by the current IPv4 policy.

mDNS discovery does not itself authorize Cast control or media traffic. Actual cross-VLAN casting remains untested because Cast receivers have not yet been deployed.

See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise project status.
