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
