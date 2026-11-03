# AI Context

## Purpose

This public repository is the concise, English-language reference for homelab architecture, intended design, and important decisions. It should remain readable by people and external AI assistants.

## Current state

- Proxmox is running on the NiPoGi mini PC.
- OPNsense is deployed as a Proxmox VM and provides routing, firewalling, DHCP, and DNS.
- A managed switch and UniFi U7 Lite are integrated into the VLAN-based network.
- UniFi Network Application runs in a Debian LXC; the U7 Lite is adopted, updated, and online.
- Seven VLANs have been created. VLANs 10, 20, and 30 are operational; VLANs 40 and 50 have recently shown correct gateway assignment and Internet access. Full end-to-end validation of all seven VLANs remains open.
- Inter-VLAN firewall rules still need to be implemented and tested. Cross-VLAN Cast device discovery also needs a suitable service, likely a deliberately scoped mDNS reflector/repeater.
- See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise status summary.

## Working with the user

The user is a beginner with Linux and networking. Explain commands, reasons, risks, and trade-offs before significant changes. Do not assume planned designs are implemented. Prefer simple, maintainable solutions and ask before consequential or disruptive changes.

When proposing a significant architectural change, briefly cover what changes, why, alternatives, trade-offs, and impact on the rest of the system.

## Sources of truth

- GitHub: concise architecture, design intent, and decisions.
- Private inventory table, transitioning to NetBox: actual IP assignments and detailed inventory.
- Official vendor documentation: current technical behavior and configuration guidance.

Do not duplicate detailed inventory or full configuration tables here. Keep real IP addresses, prefixes, credentials, keys, tokens, MAC addresses, VPN endpoints, device-specific hostnames, and other sensitive details out of this public repository. Use anonymized examples only.

## Documentation rules

Use these status values consistently:
- **Present** — physically or logically exists.
- **Configured** — set up and verified for its stated role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Rejected** — explicitly not part of the target design.

“Present” does not mean “configured.” Keep this repository short, remove duplication when information is preserved, and put each detail in its natural home: network policy in `NETWORK.md`, current status and open decisions in `PROJECT_CONTEXT.md`, and overall design in `ARCHITECTURE.md`.
