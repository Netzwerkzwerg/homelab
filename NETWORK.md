# Network Design

## Status

This is the intended design, not a deployed configuration. Inter-VLAN traffic will be denied by default and allowed only through explicit firewall rules.

## VLANs

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Private | Trusted PCs, MacBooks, consoles, and primary WLAN |
| 20 | Smart Home / IoT | Hue, Homematic, EcoFlow, Thermomix, and other IoT devices |
| 30 | Multimedia / Cast | Chromecast, Google Minis, AV receiver, projector, and other media devices |
| 90 | Trusted Guest | Guest devices allowed to use selected casting functions |
| 95 | QR Guest | Internet-only guest access; no access to devices or networks inside the home |
| — | 2.5G Direct Link | Planned point-to-point link between PCs; outside the VLAN design |

VLAN names are descriptive; real IP ranges and assignments are kept in a private inventory and must not be published here.

## Access policy

- **VLAN 10:** trusted client network. Administrative access should be limited to designated management devices and required destinations.
- **VLAN 20:** isolate IoT devices. Permit only required Internet and internal service flows.
- **VLAN 30:** multimedia devices. Permit casting only from intended clients and services.
- **VLAN 90:** allow guest devices to reach selected casting devices in VLAN 30. Do not grant general access to the private or IoT networks. Specific permission to control selected smart-home devices may be considered later, but is not currently authorized or defined.
- **VLAN 95:** Internet access only. Block access to all devices and networks inside the home, including VLANs 10, 20, 30, and 90. Apply a bandwidth limit if supported by the final design.

These are policy intentions, not implemented firewall rules. Casting may need mDNS or other service discovery across VLANs; enable only the required discovery paths. Discovery alone must not grant network access.

## Hardware and implementation

The UniFi U7 Lite is the selected access point but is not configured yet. The Netgear GS108PE and UniFi USW Flex Mini are present and still integrated into the old network. VLAN support, switch-port assignments, SSID mapping, management access, and firewall rules must be verified during implementation.

The 2.5 Gbit/s direct link is planned; its exact endpoints and addressing remain open.

## Public-repository boundary

Do not publish real IP addresses or prefixes, public IPs, MAC addresses, device-specific hostnames, VPN endpoints, credentials, Wi-Fi passwords, keys, tokens, or detailed private inventory. Use anonymized examples only.
