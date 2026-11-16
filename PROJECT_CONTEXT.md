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
| Managed switches | Configured | VLAN transport and access switching |
| UniFi Network Application | Configured | Runs in a Debian LXC |
| UniFi U7 Lite | Configured | Adopted and online |
| Debian Docker VM | Configured | Runs Docker Engine and Docker Compose in VLAN40 |
| NetBox | Configured | Application and initial infrastructure inventory import complete; further firewall/connection records are optional and should be added selectively |
| Immich | Configured | Runs in Docker; local ML service is available on the Docker VM |
| Optional gaming-PC ML host | Present | Can provide Immich ML using its Ubuntu installation when deliberately booted with the hot-swap drive connected |
| Docker container DNS | Configured | Explicit DNS server configured on the Docker host |
| Home Assistant OS | Configured | Runs in a separate VM |
| Mosquitto | Configured | MQTT broker installed as a Home Assistant add-on |
| Internal DNS domain | Configured | `home.arpa` |
| VLANs and baseline firewall policy | Partially configured | See [NETWORK.md](NETWORK.md) for per-VLAN status and open validation |
| Upstream FRITZ!Box topology | Configured | OPNsense WAN uses FRITZ!Box EG home network; FRITZ!Box Büro WAN uses isolated guest network. Switch transport VLAN100 and VLAN200 are configured |
| Upstream switch VLAN300 | Configured | Reserved on both switches for future physical office-LAN access; access connections remain open |
| Upstream-home access policy | Open | Verify implementation of default block and narrowly scoped management exceptions |
| IPv6 | Deferred | Currently blocked/disabled; comprehensive IPv6 design and implementation will be a separate later phase |
| OPNsense mDNS Repeater | Configured | Enabled on VLANs 30, 40, 50, and 90 |
| Cast receivers | Configured | Existing receivers moved to VLAN30 |
| VLAN50-to-Cast discovery and communication | Configured | Tested successfully; corresponding firewall access is in place |
| VLAN90-to-Cast access | Open | Needs separate end-to-end validation |
| Captive Portal | Deferred | Revisit after the public domain, portal URLs, and certificate approach are decided |
| Bitfocus Companion | Planned | LXC; later allow only Companion host to Cast devices if required |
| 5 TB HDD and NAS storage | Planned | Storage and backup design still to be completed |
| Independent backups | Planned | Strategy and implementation open |

## Next steps

1. Review and verify final interface rules for VLAN90 and VLAN95.
2. Perform a functional validation of all seven VLANs, including DHCP, DNS, Internet access, isolation, permitted flows, and management access.
3. Test VLAN90-to-Cast discovery and communication separately.
4. Verify the upstream-home subnet restrictions and narrowly scoped management exceptions.
5. Complete the physical access connections for reserved switch VLAN300 if/when office LAN access from the ground floor and upper floor is required.
6. Deploy further services and add narrowly scoped firewall exceptions where required.
7. Configure VLAN95 Captive Portal after the public domain, portal URLs, and certificate approach are settled.
8. Finalize storage layout and independent backups.
9. Design and implement IPv6 comprehensively; until then, keep IPv6 blocked/disabled.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
