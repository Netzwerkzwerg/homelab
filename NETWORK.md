# Network Design

## Status

Seven VLANs have been created in the network design. VLANs 10, 20, and 30 are operational. VLANs 40 and 50 have recently shown correct gateway assignment and Internet access. The remaining end-to-end checks, and a complete functional review of all seven VLANs, are still open.

Inter-VLAN firewall rules have not yet been fully implemented and validated. The target policy is deny by default, with only explicitly required flows allowed. A cross-VLAN Cast discovery service (for example, a carefully scoped mDNS reflector/repeater) also remains to be selected, configured, and tested.

## VLANs

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Management | Proxmox host and network infrastructure management |
| 20 | Smart Home / IoT | Smart-home and other IoT devices |
| 30 | Multimedia / Cast | Casting and multimedia devices |
| 40 | Services | Self-hosted applications such as Immich |
| 50 | Trusted Clients | Trusted personal client devices |
| 90 | Trusted Guest | Guest devices with access to selected casting functions |
| 95 | QR Guest | Internet-only guest access |

Real IP ranges, prefixes, and assignments are kept in a private inventory and must not be published here.

## Access policy

- **VLAN 10:** management network for infrastructure. Administrative access should remain controlled.
- **VLAN 20:** isolate IoT devices. Permit only required Internet and internal service flows.
- **VLAN 30:** multimedia devices. Permit casting only from intended clients and services.
- **VLAN 40:** isolate hosted services; allow only explicitly required client and service flows. Restrict Internet access where practical.
- **VLAN 50:** trusted clients. Permit narrowly scoped access to selected management devices and ports, not unrestricted access to the management network.
- **VLAN 90:** allow guest devices to reach selected casting devices in VLAN 30. Do not grant general access to other internal networks.
- **VLAN 95:** Internet access only. Block access to all devices and networks inside the home.

These are target policies, not a claim that the corresponding firewall rules are already active. Implement, document, and test the required rules between all seven VLANs. Casting may need mDNS or other service discovery across VLANs; enable only the required discovery paths. Discovery alone must not grant network access.

## DHCP and DNS

- DHCP is provided by Kea DHCPv4 on OPNsense.
- DNS for the management network is provided by dnsmasq; the other VLANs use Unbound.
- DNS access is controlled by firewall rules.
- DHCP subnet definitions for the newly added services and trusted-client networks were added after initial lease/gateway inconsistencies. Correct gateway assignment and Internet access have since been observed, but a router restart and repeat verification remain pending.

## Wi-Fi and switching

- The UniFi U7 Lite is adopted, updated, and online.
- UniFi Network Application runs in a Debian LXC.
- OPNsense is used as a third-party gateway; VLAN networks are represented in UniFi and mapped to WLANs.
- The access point's switch connection uses the management network untagged/native and the other WLAN VLANs tagged.
- The IoT WLAN in VLAN 20 has been set up and tested.
- mDNS proxying in UniFi is disabled. Cross-VLAN Cast discovery is intended to be handled deliberately through a suitable OPNsense-side service or other explicitly chosen component.
- 802.1X is disabled because no RADIUS service is deployed.

## Validation scope

Before treating the network as complete, test all seven VLANs (10, 20, 30, 40, 50, 90, and 95), not just DHCP. Verify as applicable:

- VLAN tagging, client connectivity, DHCP leases, subnet/gateway assignment, and DNS resolution.
- Internet access and intended restrictions.
- Inter-VLAN isolation and each explicitly permitted flow.
- Access to management interfaces only from authorized networks/devices.
- WLAN-to-VLAN mapping and roaming/client behavior.
- Cast device discovery and actual casting across the intended VLAN boundary.
- IPv4/IPv6 behavior, ensuring IPv6 does not bypass the intended segmentation.

## Public-repository boundary

Do not publish real IP addresses or prefixes, public IPs, MAC addresses, device-specific hostnames, VPN endpoints, credentials, Wi-Fi passwords, keys, tokens, or detailed private inventory. Use anonymized examples only.
