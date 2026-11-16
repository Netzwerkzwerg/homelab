# Project Context

This file is the concise status summary. Detailed network policy belongs in [NETWORK.md](NETWORK.md); system and service placement belongs in [ARCHITECTURE.md](ARCHITECTURE.md).

## Status values

- **Present** — exists, not necessarily configured.
- **Configured** — configured and verified for its intended role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Deferred** — intentionally postponed.

## Current status

| Item | Status | Notes |
|---|---|---|
| Proxmox host | Configured | Runs the homelab VMs and containers |
| OPNsense | Configured | Routing, firewall, Kea DHCPv4, dnsmasq and Unbound DNS |
| Managed switches | Configured | VLAN transport and access switching |
| UniFi Network Application and access point | Configured | Controller runs in Debian LXC; AP adopted and online |
| Debian Docker VM | Configured | Docker Engine and Compose host in VLAN40 |
| NetBox | Configured | Application healthy; baseline infrastructure inventory import complete |
| NetBox firewall rules/connections | Open | Not fully modelled; optional detail for this homelab |
| Home Assistant OS | Configured | Separate VM; Mosquitto MQTT add-on installed |
| Immich | Configured | Runs on Docker VM with local machine-learning service |
| Optional gaming-PC ML | Available | Can be used by deliberately booting the gaming PC into Ubuntu with the hot-swap drive installed |
| Docker DNS | Configured | Explicit DNS resolver configured on Docker host |
| VLANs and inter-VLAN policy | Partially configured | Seven VLANs defined; see [NETWORK.md](NETWORK.md) |
| Upstream FRITZ!Box topology | Configured | OPNsense WAN uses FRITZ!Box EG home network; FRITZ!Box Büro WAN uses isolated guest network. Switch transport VLAN100 and VLAN200 are configured |
| Upstream switch VLAN300 | Reserved | Physical office-LAN access remains optional future work |
| Upstream-home access policy | Open | Verify implementation of default block and narrowly scoped management exceptions |
| IPv6 | Deferred | Currently blocked/disabled pending a complete design |
| OPNsense mDNS Repeater | Configured | Enabled on VLANs 30, 40, 50, and 90 |
| Cast receivers | Configured | Existing receivers moved to VLAN30 |
| VLAN50-to-Cast discovery and communication | Configured | Tested successfully; corresponding firewall access is in place |
| VLAN90-to-Cast access | Open | Needs separate end-to-end validation |
| Captive Portal | Deferred | Revisit after the public domain, portal URLs, and certificate approach are decided |
| Bitfocus Companion | Planned | LXC; add narrow Cast access if required |
| Storage and independent backups | Open | Final storage layout and backup strategy remain to be completed |

## Next steps

1. Review and verify final interface rules for VLAN90 and VLAN95.
2. Perform functional validation of all seven VLANs, including DHCP, DNS, Internet access, isolation, permitted flows, and management access.
3. Test VLAN90-to-Cast discovery and communication separately.
4. Verify the upstream-home subnet restrictions and narrowly scoped management exceptions.
5. Complete the physical access connections for reserved switch VLAN300 if/when office LAN access from the ground floor and upper floor is required.
6. Deploy further services and add narrowly scoped firewall exceptions where required.
7. Configure VLAN95 Captive Portal after the public domain, portal URLs, and certificate approach are settled.
8. Finalize storage layout and independent backups.
9. Design and implement IPv6 comprehensively; until then, keep IPv6 blocked/disabled.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
