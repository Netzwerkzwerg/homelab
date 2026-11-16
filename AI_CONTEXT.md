# AI Context

## Purpose

This public repository is the concise, English-language reference for homelab architecture, durable configuration, and important decisions.

## Current state

- Proxmox runs on the NiPoGi mini PC. OPNsense runs as a VM and provides routing, inter-VLAN firewalling, DHCP, and DNS.
- UniFi Network Application runs in a Debian LXC and manages the adopted UniFi access point.
- A Debian VM in the Services VLAN runs Docker Engine and Docker Compose for self-hosted applications, including NetBox and Immich.
- NetBox is deployed and healthy. The baseline infrastructure inventory import is complete. More detailed firewall-rule and connection records can be added selectively if they provide operational value; exhaustive modelling is not required for this home network.
- Immich runs in Docker. Its machine-learning service runs locally on the Docker VM by default. It can optionally run on the gaming PC's Ubuntu installation when that PC is deliberately booted with its hot-swap drive connected.
- Docker on the services VM uses an explicit DNS server configuration for reliable container DNS resolution.
- Home Assistant OS runs in a separate VM. Mosquitto is installed as a Home Assistant add-on and provides the MQTT broker.
- All existing Cast receivers have moved to VLAN30. mDNS discovery and Cast communication from VLAN50 Trusted Clients have been verified; the required firewall access is configured.
- The internal DNS domain is `home.arpa`.
- The selected upstream design keeps OPNsense WAN in the normal FRITZ!Box EG home network and connects FRITZ!Box Büro WAN to the isolated guest network. Upstream switch transport VLANs 100 and 200 are configured; VLAN300 remains reserved for future physical office-LAN access.
- IPv6 is currently blocked/disabled; a comprehensive IPv6 design and implementation is deferred.
- Office remote access and home remote access are separate VPN systems. The office server's outbound VPN is only for the office network; a separate OPNsense VPN may provide access to the home VLANs.
- See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise status summary, [ARCHITECTURE.md](ARCHITECTURE.md) for the system design, and [NETWORK.md](NETWORK.md) for network policy.

## Working with the user

The user is a beginner with Linux and networking. Explain commands, reasons, risks, and trade-offs before significant changes. Do not assume planned designs are implemented. Prefer simple, maintainable solutions and ask before consequential or disruptive changes.

## Sources of truth

- GitHub: concise architecture, design intent, and durable decisions.
- NetBox: actual IP assignments and detailed infrastructure inventory.
- OPNsense: actual firewall rules and network enforcement.
- Official vendor documentation: current technical behavior and configuration guidance.

Do not duplicate detailed inventory or full configuration tables here. Keep real IP addresses, prefixes, credentials, keys, tokens, MAC addresses, VPN endpoints, device-specific hostnames, and other sensitive details out of this public repository.

## Documentation rules

Use these status values consistently:
- **Present** — physically or logically exists.
- **Configured** — set up and verified for its stated role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Deferred** — intentionally postponed.

Keep this repository short, remove duplication, and put each detail in its natural home: network policy in `NETWORK.md`, current status and open decisions in `PROJECT_CONTEXT.md`, and system/service placement in `ARCHITECTURE.md`. Record durable decisions, not every troubleshooting step or temporary maintenance action.
