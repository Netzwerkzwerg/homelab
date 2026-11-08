# Project Context

This file is the concise status summary. Detailed network policy belongs in [NETWORK.md](NETWORK.md); the overall design and service deployment details belong in [ARCHITECTURE.md](ARCHITECTURE.md).

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
| Debian Docker VM | Configured | Runs Docker Engine and Docker Compose in VLAN40 |
| Docker Compose stack for NetBox | Configured | NetBox web app, worker, PostgreSQL, and Valkey services are healthy |
| NetBox | Configured | Application and stack deployed and healthy; inventory import is pending. Service access details in [ARCHITECTURE.md](ARCHITECTURE.md) |
| Internal DNS domain | Configured | `home.arpa` |
| VLANs and firewall policy | Partially configured | See [NETWORK.md](NETWORK.md) for per-VLAN status and open validation |
| Upstream FRITZ!Box topology | Configured | OPNsense WAN uses FRITZ!Box EG home network; FRITZ!Box Büro WAN uses isolated guest network. Switch transport VLAN100 (home) and VLAN200 (guest) are configured on both upstream switches |
| Upstream switch VLAN300 | Configured | Reserved on both switches for future physical office-LAN access from ground floor and upper floor; access connections remain open |
| Upstream-home access policy | Planned | Block upstream home subnet from all OPNsense VLANs by default; add narrowly scoped management exceptions for FRITZ!Box EG and selected upstream switches |
| IPv6 | Deferred | Currently blocked/disabled; comprehensive IPv6 design and implementation will be a separate later phase |
| OPNsense mDNS Repeater | Configured | Enabled on VLANs 30, 40, 50, and 90 |
| Cross-VLAN Cast control | Open | Cast receivers not deployed; planned access from VLAN50 and VLAN90 to Cast device alias |
| Captive Portal | Deferred | Revisit after the public domain, portal URLs, and certificate approach are decided |
| Immich / Nextcloud | Planned | Intended for a Docker VM in the services network |
| Home Assistant OS | Planned | Separate VM |
| Bitfocus Companion | Planned | LXC; later allow only Companion host to Cast devices |
| 5 TB HDD and NAS storage | Planned | Storage and backup design still to be completed |
| Independent backups | Planned | Strategy and implementation open |

## Next steps

1. Review and verify final interface rules for VLAN90 and VLAN95.
2. Perform a complete functional validation of all seven VLANs, including DHCP, DNS, Internet access, isolation, permitted flows, and management access.
3. Import the network structure, VLANs, prefixes, devices, interfaces, and IP assignments into NetBox. Keep actual inventory private and do not copy it into this public repository.
4. Add and test OPNsense rules that block access from all internal VLANs to the upstream home subnet by default, with narrowly scoped management exceptions for the FRITZ!Box EG and the two upstream switches.
5. Complete the physical access connections for reserved switch VLAN300 if/when office LAN access from the ground floor and upper floor is required.
6. Deploy Cast receivers, then test mDNS discovery and actual casting; add the planned VLAN50/VLAN90 access rules if not already present.
7. Deploy planned services and add narrowly scoped firewall exceptions (Home Assistant to required IoT devices; Companion to Cast devices).
8. Configure VLAN95 Captive Portal after the public domain, portal URLs, and certificate approach are settled.
9. Finalize storage layout and independent backups.
10. Design and implement IPv6 comprehensively; until then, keep IPv6 blocked/disabled.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
