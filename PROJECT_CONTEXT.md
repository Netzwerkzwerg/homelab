# Homelab Project Context

## Purpose

This document captures the current project status, hardware, network design, storage concept and unresolved architectural decisions.

It supplements `AI_CONTEXT.md`, `ARCHITECTURE.md` and `NETWORK.md`.

**Status labels**
- **Existing:** currently present or confirmed
- **Planned:** intended design, not necessarily implemented
- **Open:** requires confirmation or a design decision

Do not treat planned configurations as implemented facts.

## 1. Hardware and Physical Topology

### Main server — Existing

- Model: NiPoGi AK1 Plus
- CPU: Intel N100
- RAM: 16 GB
- Internal storage: 512 GB NVMe SSD
- Additional storage: Seagate BarraCuda 5 TB HDD, reportedly installed internally using a modified enclosure and a short SATA cable

The physical HDD installation and its current operational status should be confirmed.

### Network equipment — Reported existing

- Core switch: Netgear ProSafe GS108PE, 8-port Gigabit Ethernet, with PoE on selected ports
- Multimedia switch: Ubiquiti UniFi USW Flex Mini, 5-port Gigabit Ethernet
- Wireless access point: TP-Link EAP220
- Additional access point: TP-Link Archer C6 v2, intended to operate without routing functionality

Exact firmware versions, VLAN capabilities and current configurations remain to be verified.

### WAN and LAN separation — Planned / reported design

The external Internet connection is intended to connect directly to a dedicated USB-to-Gigabit Ethernet adapter on the Proxmox server.

The WAN connection should not be connected directly to internal LAN switches.

The intended goal is to separate the external uplink physically from the internal network and terminate it at the virtual firewall.

The physical installation and firewall configuration must be verified before considering this design complete.

## 2. Virtualization and Firewall

### Proxmox

**Existing:** Proxmox runs on the Intel N100 server.

### OPNsense

**Planned / reported design:** OPNsense will act as the central virtual router and firewall.

It is intended to provide:
- Inter-VLAN routing
- Firewall enforcement
- Internet access control
- Selected multicast and service-discovery support

The exact virtual NIC mapping, WAN/LAN interfaces, VLAN trunking and recovery procedure remain to be documented.

The consequences of running the firewall as a VM on the same host as other services must be considered, especially during host maintenance or failure.

## 3. Network Design

The intended design uses one IP subnet per VLAN, with routing and inter-network filtering performed by the firewall.

Concrete IP addresses and internal subnet assignments are intentionally omitted from this public document.

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | Hauptnetz_Privat | Trusted personal computers, MacBooks, consoles and primary WLAN |
| 20 | SmartHome_IoT | Smart-home and IoT devices, including Hue, Homematic, EcoFlow and Thermomix |
| 25 | Multimedia_Cast | Chromecasts, Google Minis, AV receiver, projector and other multimedia devices |
| 90 | Gastnetz_Trusted | Visitor network with access to selected casting functionality |
| 95 | Gastnetz_QR | Isolated guest Internet access with bandwidth limitation |
| TBD | Direktlink_2.5G | Dedicated point-to-point connection; endpoints and addressing still need confirmation |

The intended IP addressing scheme may use the VLAN ID as the third IPv4 octet. This remains a design convention to confirm, not a requirement.

### IP Address Management

A structured allocation scheme is being considered for gateways, network hardware, hosts, services and DHCP clients.

Static DHCP reservations may be used for devices requiring stable addresses.

NetBox is being considered as the authoritative inventory for devices, interfaces, VLANs, prefixes and IP assignments.

## 4. Firewall Policy

The intended baseline is **deny inter-VLAN traffic by default**, with explicit rules for required communication.

### Intended policies

- VLAN 10: trusted client network with administrative access to selected services
- VLAN 20: restricted IoT network; Internet access only as required for cloud functionality and updates
- VLAN 25: multimedia network with required Internet access and controlled casting access
- VLAN 90: guest access to selected casting functionality, without unrestricted access to internal networks
- VLAN 95: Internet-only guest network, isolated from internal networks and subject to bandwidth restrictions

These are policy intentions, not a complete or implemented firewall ruleset.

Administrative access to every network should not automatically imply unrestricted access from all devices in VLAN 10. The required management devices and permitted destinations still need to be defined.

## 5. Multicast and Service Discovery

Casting and smart-home use cases may require mDNS or other multicast-based discovery mechanisms.

An mDNS repeater such as Avahi on OPNsense is being considered.

Potential use cases include:
- Trusted clients discovering Chromecast and other casting devices
- Selected guest clients discovering permitted multimedia devices

The exact permitted VLAN combinations and required traffic flows are unresolved.

One previous proposal referenced VLAN 91, but the currently documented trusted guest network is VLAN 90. Do not implement VLAN 91 without explicit confirmation.

Service discovery must not be treated as equivalent to authorization. Firewall rules must still restrict which devices can communicate.

## 6. Storage and Applications

### Internal NVMe SSD — Planned allocation

The internal SSD is intended to host:
- Proxmox
- OPNsense
- Home Assistant
- Application databases and other performance-sensitive data

Immich is being considered for photo management. Its database and thumbnail-generation workload are intended to use SSD storage for performance.

The exact deployment and resource allocation are not yet confirmed.

### 5 TB HDD — Planned allocation

The additional HDD is intended primarily for:
- Original photo files
- Historical backups or archival data

A possible implementation is an ext4 filesystem managed by Proxmox and mounted into selected containers.

The final filesystem, permissions, backup arrangement and recovery procedure remain undecided.

**Important:** A second disk inside the same physical host is not an independent backup against host failure, theft, electrical damage or other physical incidents.

### Future NAS — Possible expansion

A separate NAS may be introduced if storage requirements grow.

The potential future design is:
- Compute-intensive photo processing remains on the N100 host
- Bulk storage moves to a dedicated storage system
- A dedicated 2.5 Gbit/s network connection provides connectivity

ZFS and a separate SATA-equipped enclosure are possible options, not final decisions.

## 7. Smart Home

Home Assistant is intended to become the central smart-home platform.

The existing smart-home setup is fragmented and is planned for gradual migration.

IoT devices are intended to reside primarily in VLAN 20, while multimedia and casting devices reside in VLAN 25.

Required communication between Home Assistant, IoT devices and multimedia services must be identified before restrictive firewall rules are finalized.

## 8. Security and Public Documentation

This repository is public.

Do not store:
- Passwords, API keys or access tokens
- SSH or WireGuard private keys
- Wi-Fi credentials
- Public IP addresses or VPN endpoints
- MAC addresses or sensitive device-specific identifiers
- Exact internal addressing or detailed operational configuration unless there is a deliberate reason to publish it

Use placeholders in public examples. Maintain sensitive inventory and configuration separately.

## 9. Open Decisions

The following questions need resolution before the design is considered final:

1. What are the exact WAN and LAN interface assignments on the Proxmox host?
2. Which switch ports carry tagged VLANs, and which are access ports?
3. Does the current switch and access-point configuration support every required VLAN and SSID?
4. Which devices require communication between VLANs, especially Home Assistant and IoT devices?
5. Which casting flows are needed for VLANs 10, 25 and 90?
6. Is the 2.5 Gbit/s point-to-point link between two PCs, or between a PC and a NAS?
7. Is the HDD currently installed and operational, and how will its data be backed up independently?
8. Which services will run in VMs versus containers?
9. How will the system recover if Proxmox or the virtual firewall fails?
10. Will NetBox become the authoritative source for network inventory and addressing?

## 10. Instructions for AI Assistants

- Distinguish existing facts, planned designs and unresolved questions.
- Do not invent missing configuration details.
- Identify contradictions before proposing implementation steps.
- Explain technical trade-offs and security implications.
- Prefer simple, maintainable solutions.
- Update the documentation when an architectural decision is confirmed.
