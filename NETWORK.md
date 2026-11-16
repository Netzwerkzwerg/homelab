# Network Design

## Status

Seven VLANs are defined. The baseline firewall configuration is in place for VLANs 20, 30, and 40, and the administrator access policy for VLAN50 is configured. VLAN10 intentionally remains a privileged infrastructure network with an IPv4 pass-to-any rule. The existing Cast receivers have been moved to VLAN30, and discovery plus Cast communication from VLAN50 Trusted Clients have been verified. The corresponding access rules are therefore configured and operational for this tested path.

A shared floating rule provides DNS and DHCP access for the VLANs. IPv6 is currently blocked/disabled and will be designed and implemented comprehensively in a later phase. Validation of all VLANs and all other intended flows remains open.

## Upstream FRITZ!Box topology and policy

### Selected design (implementation status varies by item)

- FRITZ!Box EG remains the upstream Internet router and provides the normal home network for its private clients and NAS.
- OPNsense WAN remains connected to the normal FRITZ!Box EG home network. OPNsense continues to be the gateway and firewall for all locally managed VLANs.
- FRITZ!Box Büro WAN is connected to the isolated LAN guest network of FRITZ!Box EG. Its office LAN remains behind FRITZ!Box Büro and is separate from OPNsense and its VLANs.
- The VLAN-capable switch connected to FRITZ!Box EG and the basement distribution switch are installed and configured. VLAN 100 carries the normal FRITZ!Box EG home segment (including the OPNsense WAN and additional upper-floor home clients); VLAN 200 carries the FRITZ!Box EG guest segment to the FRITZ!Box Büro WAN. Both VLANs are configured across the two switches.
- VLAN 300 is configured on both switches as a reserved path for future physical access to the office LAN from the ground floor and upper floor, without using the office VPN. The required access ports/physical connections remain future work.
- These switch transport VLANs (100/200/300) are distinct from the OPNsense-managed internal VLANs (10/20/30/40/50/90/95).

### Intended OPNsense policy

- Create an alias for the complete upstream FRITZ!Box EG home subnet (actual prefix kept private in NetBox), plus a separate alias containing only the FRITZ!Box EG and the two upstream switch management addresses.
- On each internal OPNsense VLAN, place the narrowly scoped management pass rule above the upstream-subnet block rule, and place that block above any general Internet/pass rule. This is a destination-network restriction and must not block ordinary Internet access, because Internet destinations are not part of the upstream private subnet and OPNsense forwards them through its WAN gateway.
- VLAN10 Management may access the explicitly listed upstream management hosts. On other VLANs, only designated administrator clients (where applicable) may access those hosts.
- Block all remaining access from each internal VLAN to the complete upstream FRITZ!Box EG home subnet.
- Do not grant broad upstream-subnet access to the entire Management VLAN. Prefer the specific management hosts and required management ports.
- The policy must cover both IPv4 and IPv6. IPv6 is currently blocked/disabled, so no IPv6 access is intended until a complete IPv6 plan is designed and tested.

### Remote access / VPN decisions

- The office VPN is independent: the office server initiates an outbound tunnel for remote access to the office network only. It does not provide a route into OPNsense or the home VLANs. No inbound port forwarding on FRITZ!Box EG is planned for this office VPN.
- A separate remote-access VPN for the home network may run on OPNsense (for example, WireGuard). If the Internet connection has a publicly reachable IPv4 address, FRITZ!Box EG can forward the chosen UDP port to the OPNsense WAN address. If the IPv4 address is dynamic, DDNS can keep a hostname updated. Public reachability and the actual address type still need verification; no port forward is configured yet.

## VLANs

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Management | Proxmox host and network infrastructure management |
| 20 | Smart Home / IoT | Smart-home and other IoT devices |
| 30 | Multimedia / Cast | Casting and multimedia devices |
| 40 | Services | Self-hosted applications such as NetBox and Immich |
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
| 50 — Trusted Clients | Allow TRUSTED_ADMIN_CLIENTS to INTERNAL_NETS; block the remaining VLAN50 network from INTERNAL_NETS; then allow VLAN50 to any. The tested Cast receiver access is permitted by the configured rules/aliases. |
| 90 — Trusted Guest | mDNS rule is present. Final general access policy and end-to-end behavior still need review and testing. |
| 95 — QR Guest | Final interface rules and end-to-end isolation still need review and testing. Captive Portal is deferred. |

The VLAN50 administrator alias contains only the designated administrator client addresses. Actual addresses are stored privately and are intentionally omitted from this public repository.

## Aliases

The following policy aliases are used in OPNsense:

- `INTERNAL_NETS`: internal VLAN networks.
- `TRUSTED_ADMIN_CLIENTS`: designated administrator clients.
- `MANAGEMENT_HOSTS`: infrastructure management endpoints.
- `MDNS_NETWORKS`: VLANs 30, 40, 50, and 90.
- `CAST_DEVICES`: Cast receiver addresses; actual device addresses are kept private.

Do not add actual addresses or detailed device inventory to this public document.

## mDNS and Cast

- OPNsense mDNS Repeater plugin is installed and enabled.
- Listen interfaces: VLANs 30, 40, 50, and 90.
- CARP failover is disabled; the blocklist is empty.
- An IPv4 UDP rule to destination 224.0.0.251, destination port 5353, using `MDNS_NETWORKS` as source, is present on each of the four participating interfaces.
- UniFi mDNS proxy remains disabled; cross-VLAN discovery is handled by OPNsense.
- Existing Cast receivers are deployed in VLAN30.
- Discovery and Cast communication from VLAN50 Trusted Clients have been tested successfully. The corresponding firewall access is configured.
- mDNS only provides service discovery; Cast control and media traffic also require firewall permission.
- VLAN90-to-Cast behavior is not yet confirmed and should be tested separately before considering it operational.

## DHCP and DNS

- DHCP is provided by Kea DHCPv4 on OPNsense.
- DNS for the management network is provided by dnsmasq; the other VLANs use Unbound.
- The internal DNS domain is `home.arpa`.
- DNS and DHCP access is handled by the shared floating firewall rule.
- The Docker host has an explicit DNS server configured for container DNS resolution.

## Wi-Fi and switching

- The UniFi U7 Lite is adopted and online.
- UniFi Network Application runs in a Debian LXC.
- OPNsense is used as a third-party gateway; VLAN networks are represented in UniFi and mapped to WLANs.
- The access point's switch connection uses the management network untagged/native and the other WLAN VLANs tagged.
- mDNS proxying in UniFi is disabled; cross-VLAN discovery is handled by the OPNsense mDNS Repeater.

## Open work and validation

- Review and verify the final interface rules for VLAN90 and VLAN95.
- Verify VLAN10 management access is limited from other VLANs as intended.
- Test VLAN90-to-Cast discovery and communication; do not assume the VLAN50 result applies to guests.
- Test all seven VLANs: VLAN tagging, DHCP, DNS, Internet access, internal isolation, permitted flows, and management access.
- Add narrowly scoped service-specific rules as applications are deployed (for example, Home Assistant to required IoT devices).
- Configure VLAN95 Captive Portal only after the public domain, portal URLs, and certificate approach are settled.
- Keep IPv6 blocked/disabled until a complete IPv6 addressing, routing, firewall, and remote-access design is implemented and validated.
- Implement and test OPNsense upstream-subnet restrictions and the narrowly scoped management exceptions described above.
- Configure and test the reserved VLAN300 physical office-network access when the required ground-floor and upper-floor connections are installed.
- Verify the public IPv4 reachability of the Internet connection before configuring a home WireGuard port forward; use DDNS only if the public IPv4 is dynamic.

## Public-repository boundary

Do not publish real IP addresses or prefixes, public IPs, MAC addresses, device-specific hostnames, VPN endpoints, credentials, Wi-Fi passwords, keys, tokens, or detailed private inventory. Use anonymized examples only.
