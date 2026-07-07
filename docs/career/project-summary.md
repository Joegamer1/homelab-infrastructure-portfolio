# Career Project Summary

This document summarizes the homelab infrastructure project in a way that is useful for resumes, interviews, portfolio reviews, and technical conversations.

## One-Sentence Summary

Built and documented a real home infrastructure environment using Proxmox, Debian Linux, Docker, Home Assistant, Tailscale, monitoring, DNS, backups, and household automation workflows.

## Project Overview

This project documents my personal homelab as a practical infrastructure and operations platform. The environment is built around a Dell Precision T7820 running Proxmox VE, with separate virtual machines for Docker-based services and Home Assistant OS.

The project is intentionally documented as a living system rather than a generic lab. It includes what is running, why design choices were made, how services are separated, what operational work has been completed, and what improvements are planned next.

## What This Demonstrates

This project demonstrates hands-on ability in:

- Virtualization with Proxmox VE
- Linux server administration
- Docker service hosting
- Internal DNS and service organization
- Private remote access using Tailscale
- Service monitoring with Uptime Kuma
- Backup planning and infrastructure recovery thinking
- Home Assistant dashboarding and household automation
- Documentation of real infrastructure decisions
- Security-conscious planning and incremental hardening

## Key Design Decisions

### Proxmox as the Virtualization Layer

Proxmox is used as the base platform so major workloads can be separated into virtual machines. This keeps the environment easier to understand, troubleshoot, and expand.

### Debian Docker VM for Services

Most containerized services run inside a dedicated Debian VM instead of directly on the Proxmox host. This keeps the host focused on virtualization and gives Docker services their own operational boundary.

### Home Assistant as a Separate VM

Home Assistant runs separately because it acts more like a household operations platform than a normal containerized service. Isolating it makes maintenance cleaner and reduces the chance that unrelated Docker changes affect household dashboards or automations.

### Private Remote Access First

Tailscale is used for private remote access so administrative services do not need to be exposed directly to the public internet.

## Current Services and Workflows

The environment currently includes:

- Proxmox VE virtualization
- Debian Docker service host
- Portainer for Docker visibility
- Homepage for service navigation
- Uptime Kuma for monitoring
- Pi-hole for DNS and filtering
- Nginx Proxy Manager for reverse proxy management
- Tailscale for private access
- Plex and media automation services
- Home Assistant OS for household dashboards and automation
- Google Calendar integration
- Donetick-backed chore workflows

## Resume-Friendly Bullet Options

- Built and maintained a Proxmox-based homelab hosting Docker services, Home Assistant, monitoring, DNS, remote access, and backup workflows.
- Designed a segmented home infrastructure environment using separate VMs for Docker services and Home Assistant to improve maintainability and reduce service coupling.
- Deployed and documented self-hosted services including Portainer, Homepage, Uptime Kuma, Pi-hole, Nginx Proxy Manager, Plex, Donetick, and Tailscale.
- Integrated Home Assistant with Google Calendar and Donetick to support household scheduling, dashboarding, and recurring chore workflows.
- Created portfolio-grade documentation covering architecture, service roles, backup planning, security posture, maintenance notes, and planned hardening work.

## Interview Talking Points

Good topics to discuss from this project:

- Why I separated Home Assistant from the general Docker stack
- How Tailscale changes the remote access threat model
- What I chose to document publicly versus keep private
- How I think about backup planning and restore testing
- How monitoring helps operate even a small home environment
- How household automation turned the lab into something people actually use
- What security hardening is complete and what still needs work

## Current Improvement Roadmap

Planned improvements include:

- UFW/firewall hardening
- Restore testing from backups
- More architecture diagrams
- Improved monitoring and alerting
- Better recovery documentation
- Continued Home Assistant workflow refinement
- House Brain/Hermes local AI assistant layer
- Future dedicated cybersecurity lab built on top of this infrastructure

## Positioning

This project is strongest when presented as practical infrastructure ownership. It is not just a list of tools. It shows that I can build, operate, document, troubleshoot, and improve systems that have real users and real workflows.
