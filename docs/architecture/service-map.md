# Service Map

This document maps the major services in the homelab environment by role, hosting location, purpose, and operational importance.

The goal of this document is to show how the environment is organized and how each service contributes to the overall infrastructure.

## Service Overview

| Service | Layer | Purpose | Hosted On | Exposure Model | Status |
|---|---|---|---|---|---|
| Proxmox VE | Virtualization | Hosts and manages virtual machines | Physical host | Private management access | Active |
| Debian Docker VM | Service Hosting | Primary Docker service host | Proxmox VE | Private access | Active |
| Home Assistant OS | Home Automation | Household automation and dashboards | Proxmox VE | Private access | Active |
| Docker | Container Runtime | Runs self-hosted containerized services | Debian Docker VM | Internal | Active |
| Portainer | Management | Docker visibility and management | Debian Docker VM | Private access | Active |
| Homepage | Dashboard | Central launchpad for services | Debian Docker VM | Private access | Active |
| Uptime Kuma | Monitoring | Service health checks and status visibility | Debian Docker VM | Private access | Active |
| Pi-hole | DNS | Internal DNS and ad-blocking | Debian Docker VM | Internal network | Active |
| Nginx Proxy Manager | Reverse Proxy | Reverse proxy management layer | Debian Docker VM | Private/internal use | Active |
| Tailscale | Remote Access | Private remote access | Host/VMs | Authenticated private mesh | Active |
| Plex | Media | Media server | Debian Docker VM | Limited service exposure | Active |
| Seerr | Media Requests | Media request frontend | Debian Docker VM | Private/internal use | Active |
| Radarr | Media Automation | Movie management | Debian Docker VM | Private/internal use | Active |
| Sonarr | Media Automation | TV management | Debian Docker VM | Private/internal use | Active |
| Prowlarr | Indexer Management | Indexer coordination | Debian Docker VM | Private/internal use | Active |
| SABnzbd | Download Client | Download processing | Debian Docker VM | Private/internal use | Active |
| Donetick | Household Operations | Recurring chore backend | Debian Docker VM | Private/internal use | Active |
| Google Calendar Integration | Scheduling | Calendar data surfaced in Home Assistant | Home Assistant OS | Cloud integration | Active |

## Core Infrastructure Services

### Proxmox VE

Proxmox VE is the virtualization platform for the environment.

It provides:

- VM hosting
- Resource allocation
- Workload separation
- Infrastructure management

Proxmox is the foundation of the homelab. Most major services run either directly or indirectly on workloads hosted by Proxmox.

### Debian Docker VM

The Debian Docker VM is the primary service host.

It provides a dedicated Linux environment for running Docker-based services without placing those services directly on the Proxmox host.

This improves separation between the virtualization layer and the application hosting layer.

### Docker

Docker is used to run most self-hosted applications.

Containerizing services makes the environment easier to organize, update, back up, and document.

### Portainer

Portainer provides visibility into Docker containers, images, networks, and volumes.

It is used as an operational management tool rather than as the core architecture itself. Docker remains the underlying runtime.

(this is a service I have not built out as much and plan to return to as my Docker architecture grows)

## Visibility and Monitoring Services

### Homepage

Homepage acts as the central launchpad for the environment.

It organizes services into functional sections, including:

- Core Infrastructure
- Home
- Docker Services
- Media Stack

Homepage improves usability by making services easier to find and navigate.

### Uptime Kuma

Uptime Kuma provides service health monitoring.

It is used to track whether important services are reachable and responding as expected.

This provides quick operational visibility without manually checking each service.

## Network and Access Services

### Pi-hole

Pi-hole provides DNS and ad-blocking capabilities.

In this environment, Pi-hole supports internal name resolution and network-level filtering.

### Nginx Proxy Manager

Nginx Proxy Manager provides a reverse proxy management layer.

It is used where proxying or cleaner service access patterns are useful.

### Tailscale

Tailscale provides private remote access to the environment.

The primary goal is to avoid exposing administrative interfaces directly to the public internet.

Tailscale allows access to internal services through an authenticated private mesh network.

## Home Automation Services

### Home Assistant OS

Home Assistant OS runs as a dedicated VM.

It provides household automation, dashboarding, and service integrations.

Current use cases include:

- Family dashboard views
- Google Calendar visibility
- Donetick chore integration
- Daily and weekly chore workflows
- Household status summaries

### Donetick

Donetick runs in Docker and provides recurring chore management.

It acts as the chore backend, while Home Assistant provides the dashboard and automation layer around it.

### Google Calendar Integration

Google Calendar is integrated with Home Assistant to surface household scheduling information.

This allows calendar data to appear alongside other household dashboard information.

## Media Services

### Plex

Plex provides media server functionality.

It is part of the Docker-hosted media stack.

### Seerr

Seerr provides a media request frontend.

It supports a cleaner request workflow for media services.

### Radarr

Radarr manages movie automation.

### Sonarr

Sonarr manages TV automation.

### Prowlarr

Prowlarr manages indexer configuration and coordination.

### SABnzbd

SABnzbd handles download processing.

## Service Grouping

The environment is organized into these main service groups:

| Group | Services |
|---|---|
| Virtualization | Proxmox VE, Debian Docker VM, Home Assistant OS VM |
| Service Management | Docker, Portainer |
| Dashboards and Monitoring | Homepage, Uptime Kuma |
| Network Services | Pi-hole, Nginx Proxy Manager, Tailscale |
| Home Automation | Home Assistant, Donetick, Google Calendar integration |
| Media Stack | Plex, Seerr, Radarr, Sonarr, Prowlarr, SABnzbd |

## Dependency Notes

Several services depend on the Debian Docker VM because it acts as the main service host.

If the Debian Docker VM is offline, the following services are affected:

- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Portainer
- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd
- Donetick

If Home Assistant OS is offline, home automation dashboards and integrations are affected, but the general Docker service stack can continue running.

If Proxmox is offline, all VM-hosted workloads are affected.

## Operational Notes

This service layout creates clear operational boundaries:

- Proxmox handles virtualization
- Debian handles Docker-hosted services
- Home Assistant handles automation
- Tailscale handles private remote access
- Uptime Kuma handles health visibility
- Homepage handles service navigation

This makes the environment easier to troubleshoot because each service has a defined role.
