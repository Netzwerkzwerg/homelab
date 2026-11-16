# Architecture

## Current topology

A NiPoGi mini PC runs Proxmox. OPNsense runs as a virtual machine and acts as the router and firewall. Its LAN connection carries the management network natively and the other networks as tagged VLANs. A managed switch connects the Proxmox host and a UniFi U7 Lite access point.

```text
Internet
   |
Dedicated WAN interface
   |
[ Proxmox host ]
   |-- [ OPNsense VM ] ---- VLAN trunk ---- [ Managed switch ]
   |                                           |
   |-- [ UniFi Network Application LXC ]       +-- [ UniFi U7 Lite ]
   |                                           +-- VLAN-specific clients
   |
   |-- [ Debian Docker VM ] -- NetBox, Immich, and supporting containers
   |
   +-- [ Home Assistant OS VM ] -- Mosquitto add-on (MQTT broker)
```

The UniFi Network Application runs in a Debian LXC. The U7 Lite is adopted and online. A separate Debian VM in the Services VLAN runs Docker Engine and Docker Compose. NetBox and Immich are deployed on this VM. Home Assistant OS runs in a separate VM.

The selected upstream-router design keeps OPNsense WAN in the normal FRITZ!Box EG home network. The separate FRITZ!Box Büro WAN is connected to the isolated LAN guest network of FRITZ!Box EG. The VLAN-capable switch connected to FRITZ!Box EG and the basement distribution switch are installed and configured: switch VLAN 100 carries the normal FRITZ!Box EG home segment (including OPNsense WAN and additional upper-floor home clients), VLAN 200 carries the FRITZ!Box EG guest segment to the FRITZ!Box Büro WAN, and VLAN 300 is reserved for future physical access to the office LAN from the ground floor and upper floor without VPN. VLAN300's physical access connections remain future work. These switch transport VLANs are distinct from the OPNsense internal VLANs.

## Responsibilities

- **Proxmox:** virtualization host for VMs and containers.
- **OPNsense:** routing, inter-VLAN firewalling, Internet access control, DHCP, DNS, and mDNS repeating.
- **UniFi Network Application:** management of the UniFi access point and WLAN configuration.
- **Docker VM:** runtime for containerized self-hosted applications.
- **NetBox:** infrastructure documentation and IP address management (IPAM); detailed inventory belongs here rather than in this public repository.
- **Immich:** photo and video library, with a local machine-learning service for features such as Smart Search.
- **Home Assistant OS:** central smart-home platform, running in a separate VM.
- **Mosquitto:** MQTT broker provided as a Home Assistant add-on.

## Docker services

- NetBox runs as a Docker Compose stack on the Debian Docker VM. Its stack includes the web application, background worker, PostgreSQL, and Valkey.
- Immich runs as a Docker Compose stack on the same VM, with its server, PostgreSQL, Redis/Valkey, and machine-learning service.
- Docker uses an explicit DNS server configuration on the host to provide reliable DNS resolution from containers.
- Immich machine learning normally runs locally on the Docker VM. An optional alternative is the Ubuntu installation on the gaming PC; that machine must be deliberately booted into Ubuntu with its hot-swap drive connected before it can provide the ML service.
- Keep service deployment details and durable dependencies here, but do not document transient troubleshooting steps, password rotations, or private environment values.

## NetBox scope

The initial infrastructure inventory import is complete. NetBox is the source of truth for detailed devices, interfaces, VLANs, prefixes, and IP assignments. Detailed firewall-rule and physical/logical connection records are optional follow-up work: model them where they materially help operations or troubleshooting, but do not pursue exhaustive documentation for its own sake. OPNsense remains the source of truth for the actual firewall configuration.

## Network security and discovery

OPNsense provides inter-VLAN routing and firewalling. Its WAN remains on the normal FRITZ!Box EG home network. Access from OPNsense VLANs to the upstream home subnet is intended to be blocked by default, with narrowly scoped management exceptions for the FRITZ!Box EG and the two upstream switches. The separate office router uses the FRITZ!Box EG LAN guest network, isolating the office from upstream home devices. Detailed policy and implementation status are maintained in [NETWORK.md](NETWORK.md).

The existing Cast receivers are in VLAN30. Discovery and Cast communication from VLAN50 Trusted Clients have been verified, and the required OPNsense firewall access is configured. mDNS discovery does not itself authorize Cast control or media traffic; both discovery and communication must be allowed.

IPv6 is currently blocked/disabled. A complete, deliberate IPv6 design—including addressing, routing, firewall policy, and validation—is deferred until a later phase; IPv6 must not be treated as implicitly secured by the current IPv4 policy.

See [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) for the concise project status.
