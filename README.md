# Homelab Infrastructure Portfolio

This repository documents a real-world self-hosted homelab and household infrastructure environment built for practical systems administration, service hosting, monitoring, automation, and security hardening practice.

The goal of this project is to present the architecture, operational decisions, security model, and lessons learned from maintaining a functional home infrastructure stack. This is not a dump of raw production configuration files. All examples in this repository are sanitized and intended to demonstrate design patterns, documentation quality, and technical decision-making without exposing secrets or private operational data.

This repository focuses on infrastructure and operations. Future cybersecurity-specific work, such as SIEM deployment, detection engineering, log analysis, attack simulation, and incident writeups, will be documented separately as a dedicated security lab project built on top of this environment.

## Project Goals

This homelab is designed to demonstrate practical experience across several infrastructure and security domains:

- Virtualization using Proxmox VE
- Linux server administration
- Docker-based service hosting
- Self-hosted monitoring and dashboards
- Private remote access using mesh VPN technology
- DNS and reverse proxy service management
- Home automation integration
- Backup planning and operational maintenance
- Security hardening and risk reduction
- Documentation suitable for professional review

## Current Environment Overview

The environment is built around a Proxmox VE host running on a Dell Precision T7820 workstation. Proxmox provides the virtualization layer for multiple workloads, including a Debian Docker VM used as the primary service host and a Home Assistant OS VM used for household automation.

The Debian Docker VM hosts the majority of self-hosted services, including dashboarding, monitoring, DNS, reverse proxy management, media services, and chore automation tooling.

Home Assistant is used as the household automation layer, integrating with Google Calendar and Donetick to provide family-facing dashboards for calendars, chores, daily tasks, and household operations.

## Core Components

### Virtualization

- **Proxmox VE**
  - Primary virtualization platform
  - Runs VM-based workloads
  - Provides separation between infrastructure services and home automation services

- **Debian Docker VM**
  - Main Docker service host
  - Runs infrastructure, dashboard, monitoring, DNS, reverse proxy, media, and automation services

- **Home Assistant OS VM**
  - Dedicated VM for Home Assistant
  - Used for household dashboarding and automation workflows

### Infrastructure Services

- **Docker**
  - Container runtime for self-hosted services

- **Portainer**
  - Docker management interface

- **Homepage**
  - Central launchpad for homelab services
  - Organized into sections such as Core Infrastructure, Home, Docker Services, and Media Stack

- **Uptime Kuma**
  - Service monitoring
  - Public-style status page for internal service health visibility

- **Pi-hole**
  - DNS and ad-blocking service

- **Nginx Proxy Manager**
  - Reverse proxy management layer

- **Tailscale**
  - Private remote access to infrastructure services
  - Reduces the need for public exposure of management interfaces

### Media Stack

The media stack is documented as an example of Docker-based service orchestration and internal service management.

Current services include:

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

This repository does not include private media data, claim tokens, credentials, indexer credentials, or production configuration files.

### Home Automation Stack

- **Home Assistant OS**
  - Runs in Proxmox as a dedicated VM
  - Provides household dashboarding and automation

- **Google Calendar Integration**
  - Calendar events are surfaced in Home Assistant
  - Used for household scheduling visibility

- **Donetick**
  - Self-hosted recurring chore backend
  - Runs in Docker
  - Integrated with Home Assistant

- **Home Assistant Chore Dashboard**
  - Displays active chores
  - Shows completed-today summary
  - Supports daily and weekly chore creation workflows
  - Uses Donetick-backed completion tracking and notifications

## Architecture Summary

At a high level, the architecture follows this pattern:

```text
Physical Host
└── Proxmox VE
    ├── Debian Docker VM
    │   ├── Docker / Portainer
    │   ├── Homepage
    │   ├── Uptime Kuma
    │   ├── Pi-hole
    │   ├── Nginx Proxy Manager
    │   ├── Tailscale access path
    │   ├── Plex / Arr media stack
    │   └── Donetick
    │
    └── Home Assistant OS VM
        ├── Home Assistant dashboards
        ├── Google Calendar integration
        └── Donetick integration
```

The design separates infrastructure hosting from home automation while still allowing the two layers to communicate through controlled service integrations.

## Security and Privacy Approach

This repository intentionally avoids publishing sensitive production data.

The following are not included:

- API tokens
- Passwords
- Private keys
- `.env` files
- Home Assistant `secrets.yaml`
- Donetick API tokens
- Plex claim tokens
- Tailscale auth keys
- Cloudflare tokens
- Personal calendar data
- Raw production Docker volumes
- Raw production application configs
- Screenshots containing private information
- Unredacted private IP details where unnecessary
- Family-specific personal information

Example files use placeholders such as:

```text
CHANGEME
HOMELAB_HOST
LOCAL_IP
TAILSCALE_IP
DONETICK_HOST
HOME_ASSISTANT_URL
API_TOKEN_HERE
```

The purpose of this repository is to document architecture and operational maturity, not to expose live infrastructure.

## Completed Work

Current completed work includes:

- Proxmox VE host deployed on Dell Precision T7820 hardware
- Debian Docker VM created and used as primary service host
- Docker service stack deployed
- Portainer configured for Docker visibility
- Homepage dashboard deployed and organized
- Uptime Kuma deployed for service monitoring
- Internal status-page style monitoring configured
- Pi-hole deployed for DNS/ad-blocking
- Nginx Proxy Manager deployed as reverse proxy layer
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

## Planned Work

Planned improvements include:

- UFW/firewall hardening
- Restore testing from generated backups
- More formal architecture diagrams
- Improved service monitoring and alerting
- Better documentation of recovery procedures
- House Brain/Hermes local AI assistant layer
- Alexa bridge for voice-assisted household workflows
- Additional Home Assistant automation refinements
- More complete security hardening checklist
- Expanded screenshots with sensitive data removed

UFW/firewall hardening is intentionally documented as planned work and should not be represented as completed until implemented and tested.

## Skills Demonstrated

This project demonstrates hands-on experience with:

- Linux administration
- Virtualization
- Docker and containerized services
- Service monitoring
- Internal DNS
- Reverse proxy management
- Private remote access
- Home automation
- Backup planning
- Operational documentation
- Security hardening
- Infrastructure troubleshooting
- Practical systems design

## Design Principles

The project follows several practical design principles:

1. **Private by default**
   - Management interfaces are intended to remain private and reachable through trusted access paths.

2. **Documented but sanitized**
   - The repository explains how the environment works without publishing raw production secrets.

3. **Service separation**
   - Infrastructure services and home automation services are separated where practical.

4. **Operational visibility**
   - Dashboards and monitoring are used to make service state easy to understand.

5. **Incremental hardening**
   - Security improvements are tracked honestly as completed, in progress, or planned.

6. **Career-useful documentation**
   - The project is written to demonstrate real technical skill, not just show screenshots of running containers.

## Repository Roadmap

Planned documentation sections:

```text
docs/
  architecture/
    overview.md
    service-map.md
    network-map.md
    security-model.md

  operations/
    backup-plan.md
    maintenance-checklist.md
    restore-notes.md

  security/
    hardening-checklist.md
    remote-access.md
    ufw-plan.md

  career/
    resume-summary.md
    project-bullets.md

examples/
  homepage/
    services.example.yaml
    docker.example.yaml

  donetick/
    docker-compose.example.yml

  home-assistant/
    donetick-dashboard.example.yaml

  backups/
    homelab-backup.example.sh

diagrams/
  README.md

screenshots/
  README.md
```

## Screenshot Policy

Screenshots may be included only after review and redaction.

Safe screenshots may include:

- Homepage dashboard with private URLs, IPs, and personal labels blurred
- Uptime Kuma status page with sensitive hostnames blurred
- Home Assistant dashboard with personal names and calendar events blurred
- Donetick dashboard with names and private task details blurred
- Portainer container list with private ports, private IPs, and sensitive names reviewed
- Proxmox VM list with private IPs and node names reviewed

Screenshots should not include:

- Tokens
- Passwords
- Email addresses
- Calendar event details
- Family names where avoidable
- API keys
- Public IP addresses
- Private keys
- Raw configuration files containing secrets

## Notes for Reviewers

This project is a living infrastructure lab. Some services are fully implemented, while others are documented as planned work. The repository is structured to show not only what was built, but also how the environment is operated, secured, monitored, and improved over time.

The most important part of this project is not that it uses a specific tool. The value is in the integration of virtualization, Linux administration, containers, monitoring, remote access, automation, security thinking, and operational documentation into one working environment.

The design separates infrastructure hosting from home automation while still allowing the two layers to communicate through controlled service integrations.

Security and Privacy Approach

This repository intentionally avoids publishing sensitive production data.

The following are not included:

API tokens
Passwords
Private keys
.env files
Home Assistant secrets.yaml
Donetick API tokens
Plex claim tokens
Tailscale auth keys
Cloudflare tokens
Personal calendar data
Raw production Docker volumes
Raw production application configs
Screenshots containing private information
Unredacted private IP details where unnecessary
Family-specific personal information

Example files use placeholders such as:

CHANGEME
HOMELAB_HOST
LOCAL_IP
TAILSCALE_IP
DONETICK_HOST
HOME_ASSISTANT_URL
API_TOKEN_HERE

The purpose of this repository is to document architecture and operational maturity, not to expose live infrastructure.

Completed Work

Current completed work includes:

Proxmox VE host deployed on Dell Precision T7820 hardware
Debian Docker VM created and used as primary service host
Docker service stack deployed
Portainer configured for Docker visibility
Homepage dashboard deployed and organized
Uptime Kuma deployed for service monitoring
Internal status-page style monitoring configured
Pi-hole deployed for DNS/ad-blocking
Nginx Proxy Manager deployed as reverse proxy layer
Tailscale configured for private remote access
Plex/Arr media stack deployed
Home Assistant OS deployed in Proxmox
Google Calendar integrated with Home Assistant
Home Assistant family dashboard created
Donetick deployed in Docker
Donetick integrated with Home Assistant
Chore dashboard created in Home Assistant
Daily and weekly chore creation workflows implemented
Donetick-backed chore completion notifications configured
Homepage expanded with Home Assistant, Donetick, Docker metrics, Tailscale Machines, and organized service sections
Backup script created for selected Docker and infrastructure configuration paths
SSH hardening completed
Planned Work

Planned improvements include:

UFW/firewall hardening
Restore testing from generated backups
More formal architecture diagrams
Improved service monitoring and alerting
Better documentation of recovery procedures
House Brain/Hermes local AI assistant layer
Alexa bridge for voice-assisted household workflows
Additional Home Assistant automation refinements
More complete security hardening checklist
Expanded screenshots with sensitive data removed

UFW/firewall hardening is intentionally documented as planned work and should not be represented as completed until implemented and tested.

Skills Demonstrated

This project demonstrates hands-on experience with:

Linux administration
Virtualization
Docker and containerized services
Service monitoring
Internal DNS
Reverse proxy management
Private remote access
Home automation
Backup planning
Operational documentation
Security hardening
Infrastructure troubleshooting
Practical systems design
Design Principles

The project follows several practical design principles:

Private by default
Management interfaces are intended to remain private and reachable through trusted access paths.
Documented but sanitized
The repository explains how the environment works without publishing raw production secrets.
Service separation
Infrastructure services and home automation services are separated where practical.
Operational visibility
Dashboards and monitoring are used to make service state easy to understand.
Incremental hardening
Security improvements are tracked honestly as completed, in progress, or planned.
Career-useful documentation
The project is written to demonstrate real technical skill, not just show screenshots of running containers.
Repository Roadmap

Planned documentation sections:

docs/
  architecture/
    overview.md
    service-map.md
    network-map.md
    security-model.md

  operations/
    backup-plan.md
    maintenance-checklist.md
    restore-notes.md

  security/
    hardening-checklist.md
    remote-access.md
    ufw-plan.md

  career/
    resume-summary.md
    project-bullets.md

examples/
  homepage/
    services.example.yaml
    docker.example.yaml

  donetick/
    docker-compose.example.yml

  home-assistant/
    donetick-dashboard.example.yaml

  backups/
    homelab-backup.example.sh

diagrams/
  README.md

screenshots/
  README.md
Screenshot Policy

Screenshots may be included only after review and redaction.

Safe screenshots may include:

Homepage dashboard with private URLs, IPs, and personal labels blurred
Uptime Kuma status page with sensitive hostnames blurred
Home Assistant dashboard with personal names and calendar events blurred
Donetick dashboard with names and private task details blurred
Portainer container list with private ports, private IPs, and sensitive names reviewed
Proxmox VM list with private IPs and node names reviewed

Screenshots should not include:

Tokens
Passwords
Email addresses
Calendar event details
Family names where avoidable
API keys
Public IP addresses
Private keys
Raw configuration files containing secrets
Notes for Reviewers

This project is a living infrastructure lab. Some services are fully implemented, while others are documented as planned work. The repository is structured to show not only what was built, but also how the environment is operated, secured, monitored, and improved over time.

The most important part of this project is not that it uses a specific tool. The value is in the integration of virtualization, Linux administration, containers, monitoring, remote access, automation, security thinking, and operational documentation into one working environment.
