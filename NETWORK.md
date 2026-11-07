# Network Design

## Status

Seven VLANs are defined. The baseline firewall configuration is in place for VLANs 20, 30, and 40, and the administrator access policy for VLAN50 is configured. VLAN10 intentionally remains a privileged infrastructure network with an IPv4 pass-to-any rule. Full end-to-end validation of all seven VLANs and all intended flows remains open.

A shared floating rule provides DNS and DHCP access for the VLANs. IPv6 is currently disabled.

## VLANs

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Management | Proxmox host and network infrastructure management |
| 20 | Smart Home / IoT | Smart-home and other IoT devices |
| 30 | Multimedia / Cast | Casting and multimedia devices |
| 40 | Services | Self-hosted applications such as NetBox |
| 50 | Trusted Clients | Trusted personal client devices |
| 90 | Trusted Guest | Guest devices with access to selected casting functions |
| 95 | QR Guest | Internet-only guest access |

Real IP ranges, prefixes, and assignments are kept in NetBox and must not be published here.

## Current firewall policy

The common floating DNS/DHCP rule is evaluated before the interface rules. IPv6 is disabled.

| VLAN | Current interface policy |
|---:|---|
| 10 — Management | IPv4 pass-to-any retained intentionally. Treat as privileged infrastructure; restrict access from other VLANs on their source interfaces. |
| 20 — Smart Home / IoT | Block source VLAN network to INTERNAL_NETS, then allow VLAN20 to any. |
| 30 — Multimedia / Cast | Allow mDNS to 224.0.0.251 UDP/5353, block source VLAN network to INTERNAL_NETS, then allow VLAN30 to any. |
| 40 — Services | Block source VLAN network to INTERNAL_NETS, then allow VLAN40 to any. No broad Cast-device access is granted. |
| 50 — Trusted Clients | Allow TRUSTED_ADMIN_CLIENTS to INTERNAL_NETS; block the remaining VLAN50 network from INTERNAL_NETS; then allow VLAN50 to any. |
| 90 — Trusted Guest | mDNS rule is present. Final general access policy and end-to-end behavior still need review and testing. |
| 95 — QR Guest | Final interface rules and end-to-end isolation still need review and testing. Captive Portal is deferred. |

The VLAN50 administrator alias contains only the designated administrator client addresses. Actual addresses are stored privately and are intentionally omitted from this public repository.

## Aliases

The following policy aliases are used in OPNsense:

- `INTERNAL_NETS`: internal VLAN networks.
- `TRUSTED_ADMIN_CLIENTS`: designated administrator clients.
- `MANAGEMENT_HOSTS`: infrastructure management endpoints.
- `MDNS_NETWORKS`: VLANs 30, 40, 50, and 90.
- `CAST_DEVICES`: reserved for Cast receiver addresses; actual device addresses are kept private.

Do not add actual addresses or detailed device inventory to this public document.

## mDNS and Cast

- OPNsense mDNS Repeater plugin is installed and enabled.
- Listen interfaces: VLANs 30, 40, 50, and 90.
- CARP failover is disabled; the blocklist is empty.
- An IPv4 UDP rule to destination 224.0.0.251, destination port 5353, using `MDNS_NETWORKS` as source, is present on each of the four participating interfaces.
- UniFi mDNS proxy remains disabled.
- mDNS only provides service discovery. It does not authorize Cast control or media traffic by itself.
- No Cast receivers are currently deployed, so discovery and actual casting have not yet been tested.
- Planned access: allow VLAN50 and VLAN90 clients to reach the `CAST_DEVICES` alias in VLAN30. Do not grant VLAN40 broad access; when Companion is deployed, add a source-host-specific rule from Companion to `CAST_DEVICES`.

## DHCP and DNS

- DHCP is provided by Kea DHCPv4 on OPNsense.
- DNS for the management network is provided by dnsmasq; the other VLANs use Unbound.
- The internal DNS domain is `home.arpa`.
- DNS and DHCP access is handled by the shared floating firewall rule.

## Wi-Fi and switching

- The UniFi U7 Lite is adopted, updated, and online.
- UniFi Network Application runs in a Debian LXC.
- OPNsense is used as a third-party gateway; VLAN networks are represented in UniFi and mapped to WLANs.
- The access point's switch connection uses the management network untagged/native and the other WLAN VLANs tagged.
- mDNS proxying in UniFi is disabled; cross-VLAN discovery is handled by the OPNsense mDNS Repeater.

## Open work and validation

- Review and verify the final interface rules for VLAN90 and VLAN95.
- Verify VLAN10 management access is limited from other VLANs as intended.
- Add and test the planned VLAN50/VLAN90-to-Cast rules when Cast receivers are deployed.
- Add narrowly scoped service-specific rules as applications are deployed (for example, Home Assistant to required IoT devices and Companion to Cast devices).
- Configure VLAN95 Captive Portal only after the public domain, portal URLs, and certificate approach are settled.
- Test all seven VLANs: VLAN tagging, DHCP, DNS, Internet access, internal isolation, permitted flows, and management access.
- Test mDNS discovery and actual casting once receivers are available.
- Revisit IPv6 policy if IPv6 is enabled in the future.

## Public-repository boundary

Do not publish real IP addresses or prefixes, public IPs, MAC addresses, device-specific hostnames, VPN endpoints, credentials, Wi-Fi passwords, keys, tokens, or detailed private inventory. Use anonymized examples only.
