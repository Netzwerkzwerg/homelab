# Architecture

## Current topology

A NiPoGi mini PC runs Proxmox. OPNsense runs as a virtual machine and acts as the router and firewall. Its LAN connection carries the management network natively and the other networks as tagged VLANs. A managed switch connects the Proxmox host and UniFi access point.

```text
Internet
   |
[ Proxmox host ]
   |-- [ OPNsense VM ] ---- VLAN trunk ---- [ Managed switch ] ---- [ UniFi AP ]
   |
   |-- [ UniFi Network Application LXC ]
   |
   |-- [ Debian Docker VM ]
   |       |-- NetBox
   |       |-- Immich
   |       +-- Supporting containers
   |
   +-- [ Home Assistant OS VM ]
           +-- Mosquitto MQTT add-on
```

The UniFi Network Application runs in a Debian LXC. The access point is adopted and online. A separate Debian VM in VLAN40 (Services) runs Docker Engine and Docker Compose.

## Responsibilities

- **Proxmox:** virtualization host for VMs and containers.
- **OPNsense:** routing, inter-VLAN firewalling, Internet access control, DHCP, DNS, and mDNS repeating.
- **UniFi Network Application:** management of the UniFi access point and WLAN configuration.
- **Docker VM:** runtime for containerized self-hosted applications.
- **NetBox:** infrastructure documentation and IP address management (IPAM).
- **Immich:** photo and video library, with a local machine-learning service for features such as Smart Search.
- **Home Assistant OS:** smart-home platform in a separate VM.
- **Mosquitto:** MQTT broker provided as a Home Assistant add-on.

## Docker services

- NetBox runs as a Docker Compose stack on the Debian Docker VM. Its stack includes the web application, background worker, PostgreSQL, and Valkey.
- Immich runs as a Docker Compose stack on the same VM, with its server, PostgreSQL, Redis/Valkey, and machine-learning service.
- Docker uses an explicit DNS server configuration on the host to provide reliable DNS resolution from containers.
- Immich machine learning normally runs locally on the Docker VM. An optional alternative is the Ubuntu installation on the gaming PC; that machine must be deliberately booted into Ubuntu with its hot-swap drive connected before it can provide the ML service.
- Document durable service placement and dependencies, not transient troubleshooting steps, password rotations, or private environment values.

## NetBox scope

The initial infrastructure inventory import is complete. NetBox is the source of truth for detailed devices, interfaces, VLANs, prefixes, and IP assignments. Detailed firewall-rule and physical/logical connection records are optional follow-up work: model them where they materially help operations or troubleshooting, but do not pursue exhaustive documentation for its own sake. OPNsense remains the source of truth for the actual firewall configuration.

## Network security and discovery

OPNsense provides inter-VLAN routing and firewalling. All existing Cast receivers are in VLAN30. Discovery and Cast communication from VLAN50 Trusted Clients have been verified, and the required OPNsense firewall access is configured. mDNS discovery alone does not authorize Cast control or media traffic; both discovery and communication must be allowed.

IPv6 is currently blocked/disabled. A complete IPv6 design—including addressing, routing, firewall policy, and validation—is deferred until a later phase.

See [NETWORK.md](NETWORK.md) for VLAN purposes and network policy, and [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for current open work.
