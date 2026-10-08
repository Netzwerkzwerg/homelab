# Homelab Architecture

## Overview

The homelab is being rebuilt with a focus on:

- simplicity
- reliability
- security
- clear separation of responsibilities
- easy maintenance
- good documentation
- future expandability

The system is based on a Proxmox host running on an N100 mini PC.

## High-level architecture

```text
Internet
   |
   v
[ Router / Firewall ]
   |
   v
[ VLANs / Network Segmentation ]
   |
   +-------------------+
   |                   |
   v                   v
[ Trusted LAN ]    [ IoT / Smart Home ]
   |                   |
   v                   v
[ Services ]      [ Home Assistant ]
   |
   v
[ Other Containers / VMs ]
```

This diagram is conceptual and will be refined as the infrastructure is designed.

## Virtualization

Proxmox is the virtualization platform.

The Proxmox host will provide virtual machines and/or containers for the different services.

Services should be separated where there is a meaningful benefit in terms of:

- security
- reliability
- maintenance
- resource management
- independent upgrades

Services should not be separated into individual VMs or containers without a clear reason.

## Network

The network will use VLANs to separate different classes of devices and services.

The exact VLAN structure will be documented separately in `NETWORK.md`.

The router/firewall is responsible for routing between VLANs and enforcing firewall rules.

## Smart Home

Home Assistant will be the central platform for the smart home.

The existing smart-home setup is currently fragmented and will be migrated gradually.

The migration should avoid unnecessary disruption to the existing system.

## Infrastructure Inventory

NetBox is being considered as the source of truth for infrastructure inventory.

This may include:

- physical devices
- virtual machines
- interfaces
- IP addresses
- VLANs
- networks/prefixes

The exact relationship between GitHub documentation and NetBox will be defined as the infrastructure develops.

## Design Principles

### Keep it simple

Prefer simple solutions that are easy to understand and maintain.

### Separate concerns

Networking, virtualization, services and documentation should have clearly defined responsibilities.

### Document important decisions

Architectural decisions that have meaningful long-term consequences should be documented.

### Avoid unnecessary complexity

Do not introduce additional software, services or abstractions without a clear benefit.

### Security by default

Services should only be exposed where necessary.

Network segmentation and firewall rules should be used to limit unnecessary communication between systems.
