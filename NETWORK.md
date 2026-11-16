# Network Design

## Overview

OPNsense provides routing, inter-VLAN firewalling, DHCP, and DNS. Seven VLANs are defined:

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Management | Proxmox host and network infrastructure management |
| 20 | Smart Home / IoT | Smart-home and other IoT devices |
| 30 | Multimedia / Cast | Casting and multimedia devices |
| 40 | Services | Self-hosted applications |
| 50 | Trusted Clients | Primary trusted personal client devices |
| 90 | Trusted Guest | Guest devices with access to selected casting functions |
| 95 | QR Guest | Internet-only guest access |

Real IP ranges, prefixes, and assignments are maintained in NetBox and must not be published here.

## Current firewall policy

Inter-VLAN access is controlled by OPNsense interface rules and aliases. VLAN10 is a privileged infrastructure network. VLAN20, VLAN30, and VLAN40 have restrictions on access to internal networks. VLAN50 is the primary trusted client network; administrator access to internal networks is controlled through a dedicated alias. VLAN90 and VLAN95 require continued review of their final isolation and guest policies.

The Cast communication path from VLAN50 to Cast devices in VLAN30 is configured and has been functionally verified. All existing Cast receivers have been moved to VLAN30. Do not describe this path as merely planned.

Home Assistant and other application services may require narrowly scoped access to IoT devices or brokers. Add service-specific rules when needed rather than broad inter-VLAN permissions.

## mDNS and Cast

- OPNsense mDNS Repeater is enabled for the relevant networks, including VLANs 30, 40, 50, and 90.
- UniFi mDNS proxy remains disabled; cross-VLAN discovery is handled by OPNsense.
- All existing Cast receivers are deployed in VLAN30.
- Discovery and Cast communication from VLAN50 Trusted Clients have been tested successfully. The corresponding firewall access is configured.
- mDNS provides discovery only; Cast control and media traffic also require firewall permission.
- VLAN90-to-Cast behavior is not yet confirmed and should be tested separately.

## DHCP and DNS

- DHCP is provided by Kea DHCPv4 on OPNsense.
- DNS for the management network is provided by dnsmasq; the other VLANs use Unbound.
- The internal DNS domain is `home.arpa`.
- DNS and DHCP access is handled by the shared floating firewall rule.
- The Docker host has an explicit DNS server configured for container DNS resolution.

## Upstream network

The selected design keeps OPNsense WAN in the normal FRITZ!Box EG home network. FRITZ!Box Büro WAN connects to the isolated guest network of FRITZ!Box EG. The upstream switch transport VLANs are distinct from the OPNsense-managed VLANs:

- VLAN100 carries the normal upstream home segment.
- VLAN200 carries the isolated guest segment to the office router WAN.
- VLAN300 is reserved for future physical office-LAN access from other floors; the required access connections remain future work.

## Wi-Fi and switching

- UniFi Network Application runs in a Debian LXC and manages the UniFi access point.
- OPNsense is used as a third-party gateway; VLAN networks are represented in UniFi and mapped to WLANs.
- The access point's switch connection uses the management network untagged/native and the other WLAN VLANs tagged.
- mDNS proxying in UniFi is disabled.

## Open work

- Review and validate the final interface rules and isolation behavior for VLAN90 and VLAN95.
- Verify VLAN10 management access is limited from other VLANs as intended.
- Test VLAN90-to-Cast discovery and communication; do not assume the VLAN50 result applies to guests.
- Test all seven VLANs: VLAN tagging, DHCP, DNS, Internet access, internal isolation, permitted flows, and management access.
- Add narrowly scoped service-specific rules as applications are deployed (for example, Home Assistant to required IoT devices).
- Configure VLAN95 Captive Portal only after the public domain, portal URLs, and certificate approach are settled.
- Keep IPv6 blocked/disabled until a complete IPv6 addressing, routing, firewall, and remote-access design is implemented and validated.
- Implement and test OPNsense upstream-subnet restrictions and the narrowly scoped management exceptions described in the architecture.
- Configure and test the reserved VLAN300 physical office-network access when the required ground-floor and upper-floor connections are installed.

## Public-repository boundary

Do not publish real IP addresses or prefixes, public IPs, MAC addresses, device-specific hostnames, VPN endpoints, credentials, Wi-Fi passwords, keys, tokens, or detailed private inventory. Use anonymized examples only.
