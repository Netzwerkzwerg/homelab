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
| VLAN10 — Management | Configured | Privileged infrastructure network; IPv4 pass-to-any retained |
| VLAN20 — Smart Home / IoT | Configured | Internal networks blocked; outbound access allowed |
| VLAN30 — Multimedia / Cast | Configured | Baseline isolation and mDNS rule configured |
| VLAN40 — Services | Configured | Internal networks blocked; outbound access allowed; service-specific exceptions pending |
| VLAN50 — Trusted Clients | Configured | Administrator alias can access internal networks; other VLAN50 clients are blocked from internal networks |
| VLAN90 — Trusted Guest | Open | mDNS configured; final access policy and end-to-end tests pending |
| VLAN95 — QR Guest | Open | Final interface rules and isolation tests pending; Captive Portal deferred |
| Shared DNS/DHCP floating rule | Configured | Applies to VLANs |
| OPNsense mDNS Repeater | Configured | Enabled on VLANs 30, 40, 50, and 90 |
| Cross-VLAN Cast control | Open | Cast receivers not deployed; planned access from VLAN50 and VLAN90 to Cast device alias |
| Captive Portal | Deferred | Revisit after domain, portal URLs, and certificate approach are decided |
| Immich / Nextcloud | Planned | Intended for a Docker VM in the services network |
| Home Assistant OS | Planned | Separate VM |
| Bitfocus Companion | Planned | LXC; later allow only Companion host to Cast devices |
| 5 TB HDD and NAS storage | Planned | Storage and backup design still to be completed |
| NetBox | Planned | Intended replacement for private IP/inventory table |
| Independent backups | Planned | Strategy and implementation open |

## Network intent

- **10 — Management:** privileged network for Proxmox and network infrastructure.
- **20 — Smart Home / IoT:** isolate smart-home and IoT devices.
- **30 — Multimedia / Cast:** casting and multimedia devices.
- **40 — Services:** self-hosted applications; add only service-specific inter-VLAN permissions.
- **50 — Trusted Clients:** designated administrator clients may access internal networks; other clients remain isolated.
- **90 — Trusted Guest:** guest access to selected casting functions only.
- **95 — QR Guest:** Internet-only access, isolated from home devices and networks.

## Next steps

1. Review and verify final interface rules for VLAN90 and VLAN95.
2. Perform a complete functional validation of all seven VLANs, including DHCP, DNS, Internet access, isolation, permitted flows, and management access.
3. Test VLAN50 administrator access and non-admin client isolation.
4. Deploy Cast receivers, then test mDNS discovery and actual casting; add the planned VLAN50/VLAN90 access rules if not already present.
5. Deploy planned services and add narrowly scoped firewall exceptions (Home Assistant to required IoT devices; Companion to Cast devices).
6. Configure VLAN95 Captive Portal after the domain, portal URLs, and certificate approach are settled.
7. Finalize storage layout and independent backups.
8. Migrate the private IP inventory to NetBox; keep real assignments out of this public repository.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
