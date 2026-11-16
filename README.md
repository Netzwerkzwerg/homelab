# Homelab

Concise, public documentation of the homelab's current architecture, network design, and durable decisions.

## Documentation

- [Project status](PROJECT_CONTEXT.md) — configured services, decisions, and remaining work.
- [Network design](NETWORK.md) — VLANs, firewall policy, discovery, and validation.
- [Architecture](ARCHITECTURE.md) — virtualization, infrastructure services, and application hosting.
- [AI working context](AI_CONTEXT.md) — documentation conventions and collaboration guidance.
- [Access test](TEST_ACCESS.md) — public test file for checking repository access.

## Current overview

Proxmox hosts OPNsense, infrastructure management services, and application workloads. OPNsense provides routing, firewalling, DHCP, and DNS for the managed VLANs. UniFi manages the wireless access point. A Debian Docker VM hosts NetBox and application services including Immich. Home Assistant runs as a separate VM.

The NetBox baseline inventory import is complete. Detailed firewall-rule and network-connection records remain optional follow-up work, not a prerequisite for a useful home-network inventory.

All existing Cast devices have moved to the Multimedia/Cast VLAN. Discovery and casting communication from the primary Trusted Clients VLAN have been verified, so the required firewall path is configured.

The Docker host uses an explicitly configured DNS resolver for container name resolution. Immich normally uses its local machine-learning service; an optional GPU-assisted ML setup on the gaming PC remains available by booting Ubuntu with the hot-swap drive installed.

Keep this repository focused on durable architecture and decisions. Do not publish credentials, real IP assignments, MAC addresses, VPN endpoints, or other sensitive operational details.
