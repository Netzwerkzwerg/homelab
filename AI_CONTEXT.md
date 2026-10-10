# AI Context

## Purpose

This public repository is the concise, English-language reference for homelab architecture, intended design, and important decisions. It should remain readable by people and external AI assistants, including ChatGPT, Gemini, and Perplexity.

## Current state

- Proxmox is installed on the NiPoGi N100 host; no VMs or services are deployed.
- The UniFi U7 Lite, Netgear GS108PE, and UniFi USW Flex Mini are present but not configured for the target network. The switches are still integrated into the old network.
- The additional 5 TB HDD is ordered but has not arrived.
- See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise status summary.

## Working with the user

The user is a beginner with Linux and networking. Explain commands, reasons, risks, and trade-offs before significant changes. Do not assume planned designs are implemented. Prefer simple, maintainable solutions and ask before consequential or disruptive changes.

When proposing a significant architectural change, briefly cover what changes, why, alternatives, trade-offs, and impact on the rest of the system.

## Sources of truth

- GitHub: concise architecture, design intent, and decisions.
- Private inventory table, transitioning to NetBox: actual IP assignments and detailed inventory.
- Official vendor documentation: current technical behavior and configuration guidance.

Do not duplicate detailed inventory or full configuration tables here. Keep real IP addresses, prefixes, credentials, keys, tokens, MAC addresses, VPN endpoints, and other sensitive details out of this public repository. Use anonymized examples only.

## Documentation rules

Use these status values consistently:
- **Present** — physically or logically exists.
- **Configured** — set up and verified for its stated role.
- **Planned** — intended but not yet implemented.
- **Open** — needs a decision or verification.
- **Rejected** — explicitly not part of the target design.

“Present” does not mean “configured.” Keep this repository short, remove duplication when information is preserved, and put each detail in its natural home: network policy in `NETWORK.md`, current status and open decisions in `PROJECT_CONTEXT.md`, and overall design in `ARCHITECTURE.md`.
