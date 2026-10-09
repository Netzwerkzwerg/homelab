# Network Architecture

## Status

This document describes the currently planned network architecture.

The architecture is not yet considered final. Design decisions may change during implementation and testing.

## Network Segmentation

The network is segmented using VLANs according to device type, trust level and required communication.

| VLAN | Name | Intended devices / purpose |
|---:|---|---|
| 10 | Hauptnetz_Privat | PCs, MacBooks, consoles and primary WLAN |
| 20 | SmartHome_IoT | Hue, Homematic, EcoFlow, Thermomix and other IoT devices |
| 30 | Multimedia_Cast | Chromecasts, Google Minis, AVR, projector and other casting/multimedia devices |
| 90 | Gastnetz_Trusted | Visitor network with access to selected casting functionality |
| 95 | Gastnetz_QR | Isolated visitor network with Internet-only access and bandwidth limitation |
| — | Direktlink_2.5G | Dedicated point-to-point 2.5 Gbit/s connection between PCs |

## VLAN 10 — Private Network

VLAN 10 is the primary trusted network.

It is intended for:

- personal computers
- MacBooks
- game consoles
- the primary trusted WLAN

Devices in this network are considered trusted compared with IoT and guest devices.

## VLAN 20 — Smart Home / IoT

VLAN 20 contains smart-home and IoT devices.

Examples include:

- Philips Hue
- Homematic
- EcoFlow
- Thermomix
- other devices with limited trust requirements

The purpose of this VLAN is to isolate IoT devices from the primary private network.

Communication from IoT devices to other networks should be restricted by firewall rules to only what is actually required.

## VLAN 30 — Multimedia / Cast

VLAN 30 contains multimedia and casting devices.

Examples include:

- Chromecast devices
- Google Minis
- AV receiver
- projector / beamer

Casting between trusted clients and devices in this VLAN must be supported where required.

The exact firewall and multicast/mDNS requirements still need to be determined during implementation.

## VLAN 90 — Trusted Guest Network

VLAN 90 is intended for visitors who require more functionality than the isolated guest network.

The primary planned use case is:

- visitor devices
- Internet access
- selected access to casting functionality

Access to the private network and IoT network should remain restricted.

The exact rules for accessing VLAN 25 will be defined during implementation.

## VLAN 95 — QR Guest Network

VLAN 95 is intended as a simple, isolated guest network.

Characteristics:

- Internet access only
- no access to private networks
- no access to IoT networks
- bandwidth limitation
- intended for easy access via a QR code

The purpose is to provide visitors with a convenient network without granting access to internal infrastructure.

## Dedicated 2.5 Gbit/s Point-to-Point Link

A dedicated 2.5 Gbit/s connection is planned between PCs.

This is a direct point-to-point connection and is therefore not part of the VLAN structure.

Its purpose is high-speed communication between the connected systems without routing this traffic through the normal network architecture.

The exact addressing and physical implementation will be documented separately once finalized.

## Inter-VLAN Communication

Inter-VLAN communication should be denied by default and explicitly allowed only where required.

Examples of communication that may require dedicated firewall rules include:

- Home Assistant communicating with IoT devices
- trusted clients communicating with casting devices
- guest devices accessing selected casting functionality
- infrastructure services communicating with required networks

The exact firewall policy will be designed after the service architecture has been defined.

## Multicast and Service Discovery

Casting and smart-home functionality may require protocols such as mDNS and other multicast-based service discovery mechanisms.

These requirements must be considered when designing the firewall and VLAN architecture.

Rather than allowing unrestricted communication between VLANs, service discovery should be enabled only where necessary.

## Security Principles

The network follows these principles:

1. Networks are separated according to trust and purpose.
2. Inter-VLAN communication is denied by default.
3. Required communication is explicitly permitted.
4. IoT devices are isolated from trusted personal devices where practical.
5. Guest networks do not receive access to internal infrastructure.
6. Guest bandwidth can be restricted independently.
7. Network changes should be documented before or alongside implementation.

## Sensitive Information

This public repository intentionally does not contain:

- internal IP address ranges
- public IP addresses
- MAC addresses
- device-specific hostnames
- VPN endpoints
- firewall credentials
- Wi-Fi passwords
- API keys or tokens
- other authentication information

Detailed infrastructure inventory and sensitive configuration may be maintained separately, for example using NetBox or a private repository.
