# AI Context

## Purpose

This public repository is the concise, English-language reference for homelab architecture, intended design, and important decisions. It should remain readable by people and external AI assistants.

## Current state

- Proxmox runs on the NiPoGi mini PC; OPNsense runs as a Proxmox VM and provides routing, firewalling, DHCP, and DNS.
- UniFi Network Application runs in a Debian LXC; the UniFi U7 Lite is adopted, updated, and online.
- A Debian VM in the Services VLAN runs Docker Engine and Docker Compose for self-hosted applications.
- NetBox is deployed on that Docker VM and is reachable through the host's published TCP port 8000. Its web application, background worker, PostgreSQL database, and Valkey services are running and healthy.
- The internal DNS domain is `home.arpa`.
- Seven VLANs are defined: 10 Management, 20 Smart Home / IoT, 30 Multimedia / Cast, 40 Services, 50 Trusted Clients, 90 Trusted Guest, and 95 QR Guest.
- A common floating firewall rule provides DNS and DHCP access for the VLANs. IPv6 is currently disabled.
- Baseline firewall policies are in place for VLANs 20, 30, and 40: allow the shared DNS/DHCP rule, block access to internal networks, then allow outbound access to any destination (Internet).
- VLAN10 intentionally retains an IPv4 pass-to-any rule as a privileged infrastructure network. Access to it from other VLANs is controlled on the source interfaces.
- VLAN50 has an administrator-client alias allowed to reach internal networks, followed by a block from the VLAN50 network to internal networks and a final allow-to-any rule. This isolates non-admin clients while preserving Internet access.
- The OPNsense mDNS Repeater plugin is installed and enabled for VLANs 30, 40, 50, and 90. Matching UDP/IPv4 rules to 224.0.0.251:5353 are present on those interfaces, using the MDNS_NETWORKS alias.
- Full end-to-end validation of all VLANs and firewall flows remains open. Cast receivers are not yet deployed, so discovery and casting have not been tested.
- See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise status summary, [ARCHITECTURE.md](ARCHITECTURE.md) for the system design, and [NETWORK.md](NETWORK.md) for the current policy.

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

“Present” does not mean “configured.” Keep this repository short, remove duplication when information is preserved, and put each detail in its natural home: network policy in `NETWORK.md`, current status and open decisions in `PROJECT_CONTEXT.md`, and overall design in `ARCHITECTURE.md`.
