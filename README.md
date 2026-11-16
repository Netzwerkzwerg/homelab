# Homelab

Short, AI-readable documentation for the homelab's current state and target design.

## Documents

- [Project status and decisions](PROJECT_CONTEXT.md) — what is configured, planned, and still open.
- [Network design](NETWORK.md) — VLAN purposes, current firewall policy, and validation work.
- [Architecture](ARCHITECTURE.md) — Proxmox, OPNsense, UniFi, Docker, NetBox, and design principles.
- [AI working context](AI_CONTEXT.md) — how assistants should use and update this repository.
- [Access test](TEST_ACCESS.md) — public test file for checking whether external AI assistants can read the repository.

## Current state

Proxmox and OPNsense are running. UniFi Network Application is deployed in a Debian LXC and the U7 Lite access point is adopted and online. A Debian Docker VM in the Services VLAN hosts NetBox, Immich, and their supporting services. Home Assistant OS runs in a separate VM, with Mosquitto available as its MQTT broker add-on. NetBox's initial infrastructure inventory import is complete; detailed firewall-rule and connection records remain optional follow-up work rather than a prerequisite for using NetBox.

Seven VLANs are defined. The existing Cast receivers have been moved to the Multimedia/Cast VLAN. mDNS discovery and Cast communication from the Trusted Clients VLAN have been verified, so the corresponding OPNsense firewall access is configured. OPNsense also provides DHCP, DNS, inter-VLAN routing, and mDNS repeating. The Docker host has an explicit DNS server configuration for container name resolution. IPv6 remains blocked/disabled pending a complete later implementation.

The optional Immich machine-learning workload can run either on the Docker VM or on the Ubuntu installation on the gaming PC. Using the gaming PC requires booting it into Ubuntu with the hot-swap drive connected.

All documentation is in English. Do not publish real IP assignments, credentials, hostnames, or other sensitive operational details.
