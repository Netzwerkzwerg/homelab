# Project Context

This file is the concise status summary. Network policy belongs in [NETWORK.md](NETWORK.md); the overall design belongs in [ARCHITECTURE.md](ARCHITECTURE.md).

## Status values

- **Present** — exists, not necessarily configured.
- **Configured** — configured and verified for its intended role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Rejected** — not part of the target design.

## Current status

| Item | Status | Notes |
|---|---|---|
| NiPoGi mini PC and Proxmox | Configured | Virtualization host |
| OPNsense | Configured | Router/firewall, Kea DHCPv4, dnsmasq and Unbound DNS |
| Managed switch | Configured | Connected to Proxmox VLAN trunk and access point |
| UniFi Network Application | Configured | Runs in a Debian LXC and is reachable |
| UniFi U7 Lite | Configured | Adopted, updated, and online |
| VLAN 10 — Management | Configured | Operational; full policy validation pending |
| VLAN 20 — Smart Home / IoT | Configured | Operational; IoT WLAN tested |
| VLAN 30 — Multimedia / Cast | Configured | Operational |
| VLAN 40 — Services | Open | Created; correct gateway and Internet access observed; router restart and full validation pending |
| VLAN 50 — Trusted Clients | Open | Created; correct gateway and Internet access observed; router restart and full validation pending |
| VLAN 90 — Trusted Guest | Open | Created in UniFi; end-to-end connectivity and isolation need validation |
| VLAN 95 — QR Guest | Open | Created in UniFi; end-to-end connectivity and isolation need validation |
| Inter-VLAN firewall policy | Open | Rules between all networks still need to be implemented and tested |
| Cross-VLAN Cast discovery | Open | Select and configure a service for discovery (likely scoped mDNS reflection/repeater), then test actual casting |
| Immich / Nextcloud | Planned | Intended for a Docker VM in the services network |
| Home Assistant OS | Planned | Separate VM |
| Bitfocus Companion | Planned | LXC |
| 5 TB HDD and NAS storage | Planned | Storage and backup design still to be completed |
| NetBox | Planned | Intended replacement for private IP/inventory table |
| Independent backups | Planned | Strategy and implementation open |

## Network intent

- **10 — Management:** Proxmox and network infrastructure.
- **20 — Smart Home / IoT:** smart-home and IoT devices.
- **30 — Multimedia / Cast:** casting and multimedia devices.
- **40 — Services:** self-hosted applications.
- **50 — Trusted Clients:** trusted personal clients.
- **90 — Trusted Guest:** guest access to selected casting functions only.
- **95 — QR Guest:** Internet-only access, isolated from home devices and networks.

Inter-VLAN traffic is intended to be denied by default. The firewall rules implementing this policy are not yet complete.

## Next steps

1. Restart OPNsense, then verify DHCP, gateway assignment, DNS, and connectivity in the affected networks.
2. Perform a complete functional validation of all seven VLANs (10, 20, 30, 40, 50, 90, and 95), including WLAN mapping, DNS, Internet access, isolation, permitted flows, and IPv4/IPv6 behavior.
3. Implement and test the inter-VLAN firewall rules according to the documented least-privilege policy.
4. Select, configure, and test a cross-VLAN Cast discovery service (such as a scoped mDNS reflector/repeater); verify both device discovery and actual casting.
5. Deploy planned services: Immich/Nextcloud, Home Assistant OS, and Bitfocus Companion.
6. Finalize storage layout and independent backups for local and NAS storage.
7. Migrate the private IP inventory to NetBox; keep real assignments out of this public repository.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
