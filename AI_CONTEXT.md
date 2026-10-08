# Homelab – AI Context

## Purpose

This repository documents the architecture, configuration concepts and important decisions of my homelab.

The documentation is intended to be understandable and useful for AI assistants such as ChatGPT, Gemini and Perplexity.

## Current situation

I am rebuilding my homelab from scratch.

The current main hardware is an N100 mini PC running Proxmox.

The planned system will include:

- Proxmox as the virtualization platform
- Router and firewall functionality
- VLAN-based network segmentation
- Home Assistant
- Various containers and services
- A structured infrastructure documentation
- NetBox as a possible source of truth for infrastructure inventory

## How I want AI assistants to help

I am a beginner with GitHub and Linux.

I want AI assistants to:

- explain what commands and configuration changes do
- explain the reasoning behind architectural decisions
- point out risks and trade-offs
- suggest better approaches when appropriate
- help me understand the system rather than simply doing everything automatically
- keep the architecture consistent with the existing documentation

I do not need an AI agent to autonomously type commands or operate my system.

I prefer to execute commands myself after understanding what they do.

## Important principle

Before making significant architectural changes, explain:

1. What is being changed
2. Why it is being changed
3. What alternatives exist
4. What the advantages and disadvantages are
5. What impact the change has on the rest of the homelab

## Sources of truth

The planned documentation model is:

- GitHub: architecture, documentation, configuration concepts and decisions
- NetBox: actual infrastructure inventory such as devices, interfaces, IP addresses, VLANs and prefixes
- Official documentation: current software-specific technical information
- AI memory: personal preferences and working style, but not authoritative infrastructure data

## Security

This repository is public.

Therefore it must never contain:

-
