# AI Context

## Purpose

This public repository is the concise, English-language reference for homelab architecture, intended design, and important decisions. It should remain readable by people and external AI assistants.

## Current state

- Proxmox runs on the NiPoGi mini PC; OPNsense runs as a Proxmox VM and provides routing, firewalling, DHCP, and DNS.
- UniFi Network Application runs in a Debian LXC; the UniFi U7 Lite is adopted, updated, and online.
- A Debian VM in the Services VLAN runs Docker Engine and Docker Compose for self-hosted applications.
- NetBox is deployed on that Docker VM; its web application, background worker, PostgreSQL database, and Valkey services are running and healthy.
- The internal DNS domain is `home.arpa`.
- Full end-to-end validation of the network and firewall flows remains open. Cast receivers are not yet deployed, so discovery and casting have not been tested.
- See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise status summary, [ARCHITECTURE.md](ARCHITECTURE.md) for the system design, and [NETWORK.md](NETWORK.md) for network policy and validation status.

## Working with the user

The user is a beginner with Linux and networking. Explain commands, reasons, risks, and trade-offs before significant changes. Do not assume planned designs are implemented. Prefer simple, maintainable solutions and ask before consequential or disruptive changes.

When proposing a significant architectural change, briefly cover what changes, why, alternatives, trade-offs, and impact on the rest of the system.

## Sources of truth

- GitHub: concise architecture, design intent, and decisions.
- NetBox: actual IP assignments and detailed infrastructure inventory.
- Official vendor documentation: current technical behavior and configuration guidance.

Do not duplicate detailed inventory or full configuration tables here. Keep real IP addresses, prefixes, credentials, keys, tokens, MAC addresses, VPN endpoints, device-specific hostnames, and other sensitive details out of this public repository. Use anonymized examples only.

## Documentation rules

Use these status values consistently:
- **Present** — physically or logically exists.
- **Configured** — set up and verified for its stated role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Rejected** — explicitly not part of the target design.

“Present” does not mean “configured.” Keep this repository short, remove duplication when information is preserved, and put each detail in its natural home: network policy in `NETWORK.md`, current status and open decisions in `PROJECT_CONTEXT.md`, and overall design and service deployment details in `ARCHITECTURE.md`.
