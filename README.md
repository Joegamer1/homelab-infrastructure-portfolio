# Homelab Infrastructure Project

This repository documents my personal homelab and household infrastructure environment.

What started as a practical home server project has become a working infrastructure platform for self-hosted services, monitoring, private remote access, home automation, backups, and day-to-day household workflows. The environment supports real services that I use, maintain, troubleshoot, and improve over time.

The goal of this repository is to present the project in a professional, portfolio-ready way while still keeping it grounded in the reality of my own homelab. This is not a generic best-practices checklist and it is not a dump of raw production configuration files. It is documentation of what I built, why I built it this way, what works well, and what I plan to improve next.

This repository focuses on infrastructure and operations. Future cybersecurity-specific work, such as SIEM deployment, detection engineering, log analysis, attack simulation, and incident writeups, will be documented separately as a dedicated security lab project built on top of this environment.

## Project Purpose

The purpose of this homelab is to build and operate a real infrastructure environment that is useful at home and valuable for career development.

This project demonstrates hands-on experience with:

- Proxmox VE virtualization
- Linux server administration
- Docker-based service hosting
- Internal service monitoring
- Private remote access
- DNS and reverse proxy management
- Home automation
- Backup planning
- Infrastructure hardening
- Operational documentation
- Practical troubleshooting

The most important part of this project is that it is real. These are not isolated lab exercises. The services documented here support actual household workflows and infrastructure needs.

## Environment Overview

The environment is built around a Dell Precision T7820 workstation running Proxmox VE.

Proxmox provides the virtualization layer. It hosts multiple virtual machines, including:

- A Debian Docker VM used as the main service host
- A Home Assistant OS VM used for household automation and dashboarding

The Debian Docker VM runs most of the self-hosted services in the environment, including dashboards, monitoring, DNS, reverse proxy management, media services, and Donetick.

Home Assistant provides the household automation layer. It integrates with Google Calendar and Donetick to support family dashboard views, recurring chores, task visibility, and household coordination.

## Core Infrastructure

### Proxmox VE

Proxmox VE is the foundation of the environment.

I use it to separate major workloads into virtual machines instead of running everything directly on one operating system. This gives the homelab cleaner boundaries and makes it easier to expand over time.

### Debian Docker VM

The Debian Docker VM is the primary service host.

It runs the majority of the Docker-based applications in the environment, including infrastructure services, dashboards, monitoring tools, media services, and Donetick.

I chose this approach because it keeps the Proxmox host focused on virtualization while giving Docker services their own dedicated Linux environment.

### Home Assistant OS VM

Home Assistant OS runs as its own VM.

I chose to keep Home Assistant separate from the general Docker stack because it functions more like a household operations platform than a standard containerized service. Running it in its own VM keeps home automation isolated, simplifies maintenance, and reduces the chance that changes to other Docker services will affect household dashboards and automations.

## Current Services

### Infrastructure and Operations

- Docker
- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Tailscale

### Media Stack

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

### Home Automation and Household Operations

- Home Assistant OS
- Google Calendar integration
- Donetick
- Home Assistant chore dashboard
- Donetick-backed chore completion notifications

## Home Assistant and Household Workflows

One of the most useful parts of this environment is the Home Assistant dashboard work.

The current setup includes:

- Family dashboard views
- Google Calendar visibility
- Donetick integration
- Active chore tracking
- Completed-today chore summaries
- Daily and weekly chore creation workflows
- Donetick completion notifications

This part of the project matters to me because it turns the homelab into something useful beyond the server rack. It is not just infrastructure for infrastructure’s sake. It supports real household coordination and gives me a platform to keep improving automation over time.

## Architecture Summary

At a high level, the environment is organized into four layers:

1. **Physical and virtualization layer**
   - Dell Precision T7820
   - Proxmox VE
   - VM separation for major workloads

2. **Service hosting layer**
   - Debian Docker VM
   - Docker and Portainer
   - Infrastructure, monitoring, DNS, reverse proxy, media, and household services

3. **Home automation layer**
   - Home Assistant OS VM
   - Google Calendar integration
   - Donetick integration
   - Household dashboard workflows

4. **Access and visibility layer**
   - Tailscale for private remote access
   - Homepage for centralized service navigation
   - Uptime Kuma for service health visibility

This structure keeps the environment understandable as it grows.

## Completed Work

Completed work includes:

- Proxmox VE deployed on Dell Precision T7820 hardware
- Debian Docker VM created as the primary service host
- Docker service stack deployed
- Portainer configured for Docker visibility
- Homepage deployed and organized into useful service sections
- Uptime Kuma deployed for service monitoring
- Pi-hole deployed for DNS and ad-blocking
- Nginx Proxy Manager deployed as a reverse proxy layer
- Tailscale configured for private remote access
- Plex/Arr media stack deployed
- Home Assistant OS deployed in Proxmox
- Google Calendar integrated with Home Assistant
- Home Assistant family dashboard created
- Donetick deployed in Docker
- Donetick integrated with Home Assistant
- Chore dashboard created in Home Assistant
- Daily and weekly chore creation workflows implemented
- Donetick-backed chore completion notifications configured
- Homepage expanded with Home Assistant, Donetick, Docker metrics, Tailscale Machines, and organized service sections
- Backup script created for selected Docker and infrastructure configuration paths
- SSH hardening completed

## Planned Improvements

Planned work includes:

- UFW/firewall hardening
- Restore testing from backups
- More formal architecture diagrams
- Improved monitoring and alerting
- Better documentation of recovery procedures
- House Brain/Hermes local AI assistant layer
- Alexa bridge for voice-assisted household workflows
- Additional Home Assistant automation refinements
- Dedicated future cybersecurity lab repository built on top of this infrastructure

UFW/firewall hardening is documented as planned work and should not be represented as completed until implemented and tested.

## Security and Privacy

This repository uses sanitized documentation and example files.

It does not include production secrets, raw configuration files, private keys, API tokens, passwords, personal calendar data, private household details, or sensitive screenshots.

Example files use placeholder values such as:

- `CHANGEME`
- `LOCAL_IP`
- `SERVICE_URL`
- `API_TOKEN_HERE`
- `HOMELAB_HOST`
- `HOME_ASSISTANT_URL`
- `DONETICK_HOST`

## Skills Demonstrated

This project demonstrates practical experience with:

- Linux administration
- Virtualization
- Docker and containerized services
- Service monitoring
- Internal DNS
- Reverse proxy management
- Private remote access
- Home automation
- Backup planning
- Infrastructure troubleshooting
- Operational documentation
- Security-conscious system design

## Documentation

Current documentation includes:

- `docs/architecture/overview.md`
- `docs/architecture/service-map.md`
- `docs/security/security-model.md`
- `docs/operations/backup-plan.md`
- `examples/README.md`

Planned documentation includes:

- `docs/operations/maintenance-notes.md`
- `docs/operations/restore-notes.md`
- `docs/security/remote-access.md`
- `docs/security/hardening-roadmap.md`
- `docs/career/project-summary.md`

## Notes for Reviewers

This project is a living homelab. Some areas are complete and actively used, while others are planned improvements.

The value of this project is not simply that it runs a list of tools. The value is in how the pieces fit together: virtualization, Linux administration, Docker operations, monitoring, private remote access, home automation, backup planning, and practical documentation.

This is the infrastructure foundation I plan to keep building on.
