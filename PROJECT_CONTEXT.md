# Project Context

This file is the concise status summary. Network policy belongs in [NETWORK.md](NETWORK.md); the overall target design belongs in [ARCHITECTURE.md](ARCHITECTURE.md).

## Status values

- **Present** — exists, not necessarily configured.
- **Configured** — configured and verified for its intended role.
- **Planned** — intended, not yet implemented.
- **Open** — needs a decision or verification.
- **Rejected** — not part of the target design.

## Current status

| Item | Status | Notes |
|---|---|---|
| NiPoGi AK1 Plus (Intel N100, 16 GB RAM, 512 GB NVMe) | Present | Proxmox installed; no VMs or services deployed |
| Proxmox | Present | Base host installed; target services not deployed |
| UniFi U7 Lite | Present | Selected AP; not configured |
| Netgear GS108PE | Present | Still integrated into the old network; target VLAN configuration not done |
| UniFi USW Flex Mini | Present | Still integrated into the old network; target VLAN configuration not done |
| Seagate BarraCuda 5 TB HDD | Planned | Ordered; not yet received or installed |
| OPNsense VM | Planned | Intended router/firewall |
| Home Assistant | Planned | Intended smart-home platform |
| Immich | Planned | Photo management under consideration |
| NetBox | Planned | IP/inventory data is currently in a private table; migration is intended |
| 2.5G point-to-point link | Planned | Exact endpoints and addressing are open |
| TP-Link EAP220 | Rejected | Not part of the target design |
| TP-Link Archer C6 v2 | Rejected | Not part of the target design |

## Network intent

VLAN IDs and purposes:

- **10 — Private:** trusted personal clients.
- **20 — Smart Home / IoT:** smart-home and IoT devices.
- **30 — Multimedia / Cast:** casting and multimedia devices.
- **90 — Trusted Guest:** guest devices may access selected casting devices in VLAN 30; narrowly scoped smart-home control may be considered later.
- **95 — QR Guest:** Internet only, with no access to any device or network inside the home.

Inter-VLAN traffic is intended to be denied by default. Exact IP assignments remain private and are not documented here.

## Open decisions

1. Define WAN/LAN interface mapping and a recovery path for the virtual firewall.
2. Verify switch VLAN support and assign tagged trunk/access ports.
3. Configure the U7 Lite SSIDs and map them to the intended VLANs.
4. Define exact VLAN 90 casting flows and the required mDNS/service-discovery paths.
5. Confirm whether VLAN 90 should ever receive narrowly scoped smart-home control permissions; no such permission is currently approved.
6. Define the final firewall policy for VLAN 95 to ensure it cannot reach any home devices or internal networks.
7. Decide storage filesystem, mount strategy, and independent backup for the 5 TB HDD once it arrives.
8. Decide which services run in VMs versus containers.
9. Move the private IP inventory to NetBox and keep real assignments out of this public repository.
10. Confirm endpoints and implementation for the 2.5G direct link.

## Security boundary

This repository is public. Never commit passwords, tokens, private keys, Wi-Fi credentials, real IP assignments, MAC addresses, VPN endpoints, or sensitive operational configuration.
