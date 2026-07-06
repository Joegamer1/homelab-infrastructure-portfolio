# Homelab Infrastructure Portfolio

This repository documents a real-world self-hosted homelab and household infrastructure environment built for practical systems administration, service hosting, monitoring, automation, and infrastructure operations.

The purpose of this project is to present the architecture, design decisions, operational practices, and lessons learned from maintaining a functional home infrastructure stack. This is not a dump of raw production configuration files. All examples are sanitized and written for documentation purposes.

This repository focuses on infrastructure and operations. Future cybersecurity-specific work, such as SIEM deployment, detection engineering, log analysis, attack simulation, and incident writeups, will be documented separately as a dedicated security lab project built on top of this environment.

## Project Goals

This homelab is designed to demonstrate hands-on experience with:

- Virtualization using Proxmox VE
- Linux server administration
- Docker-based service hosting
- Internal service monitoring
- Private remote access
- DNS and reverse proxy management
- Home automation integration
- Backup planning
- Infrastructure hardening
- Operational documentation

The goal is to show not just what services are running, but how the environment is organized, maintained, and improved over time.

## Environment Overview

The environment is built around a Dell Precision T7820 workstation running Proxmox VE.

Proxmox hosts multiple virtual machines, including:

- A Debian Docker VM used as the primary service host
- A Home Assistant OS VM used for household automation and dashboarding

The Debian Docker VM runs most self-hosted services, including dashboards, monitoring, DNS, reverse proxy management, media services, and Donetick.

Home Assistant provides the household automation layer, including dashboard views, Google Calendar integration, and Donetick-backed chore workflows.

## Core Infrastructure

### Virtualization

- **Proxmox VE**
  - Primary virtualization platform
  - Hosts infrastructure and automation workloads
  - Provides separation between service roles

- **Debian Docker VM**
  - Main container host
  - Runs the majority of self-hosted applications
  - Used for infrastructure, monitoring, media, and household service workloads

- **Home Assistant OS VM**
  - Dedicated home automation VM
  - Runs Home Assistant separately from the general Docker service stack

### Infrastructure Services

- **Docker**
  - Container runtime for self-hosted services

- **Portainer**
  - Docker management and service visibility

- **Homepage**
  - Central launchpad for homelab services
  - Organized into Core Infrastructure, Home, Docker Services, and Media Stack sections

- **Uptime Kuma**
  - Service monitoring and status visibility

- **Pi-hole**
  - Internal DNS and ad-blocking

- **Nginx Proxy Manager**
  - Reverse proxy management layer

- **Tailscale**
  - Private remote access to infrastructure services

## Media Stack

The media stack is documented as an example of Docker-based service orchestration and internal service management.

Current services include:

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

This repository does not include private media data, credentials, claim tokens, indexer credentials, or production service configuration.

## Home Automation Stack

Home Assistant is used as the household automation and dashboard layer.

Current functionality includes:

- Home Assistant OS running in a Proxmox VM
- Google Calendar integration
- Family dashboard views
- Donetick integration
- Chore status visibility
- Completed-today chore summaries
- Daily and weekly chore creation workflows
- Donetick-backed completion notifications

Donetick runs as a self-hosted Docker service and acts as the recurring chore backend.

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

The design separates infrastructure hosting from home automation while still allowing the two layers to communicate through controlled service integrations.

## Current Completed Work

Completed work includes:

- Proxmox VE deployed on Dell Precision T7820 hardware
- Debian Docker VM created as the primary service host
- Docker service stack deployed
- Portainer configured for Docker visibility
- Homepage deployed and organized
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

## Repository Structure

```text
docs/
  architecture/
    overview.md
    service-map.md

  operations/
    backup-plan.md
    maintenance-checklist.md
    restore-notes.md

  security/
    security-model.md
    remote-access.md
    hardening-roadmap.md

  career/
    project-summary.md

examples/
  README.md
  homepage/
  donetick/
  home-assistant/
  backups/

diagrams/

screenshots/
Documentation Status

Current documentation:

docs/architecture/overview.md
docs/architecture/service-map.md
examples/README.md

Planned documentation:

docs/operations/backup-plan.md
docs/operations/maintenance-checklist.md
docs/operations/restore-notes.md
docs/security/security-model.md
docs/security/remote-access.md
docs/security/hardening-roadmap.md
docs/career/project-summary.md
Notes for Reviewers

This project is a living infrastructure lab. Some services are fully implemented, while others are documented as planned improvements.

The value of this project is not simply that it runs a list of tools. The value is in the integration of virtualization, Linux administration, Docker operations, monitoring, private remote access, home automation, backup planning, and clear operational documentation into one working environment.
