# Service Map

This document maps the major services in my homelab by role, hosting location, purpose, and operational importance.

The goal is to show how the environment is organized and how each service contributes to the overall system.

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

Proxmox VE is the foundation of the homelab.

It provides the virtualization layer and allows me to separate workloads into VMs. This is important because I do not want every service tied directly to one operating system or one application stack.

Proxmox gives me:

- VM hosting
- Resource allocation
- Workload separation
- A central infrastructure management point

### Debian Docker VM

The Debian Docker VM is where most of the self-hosted services live.

I use this VM as the main Docker host because it keeps the Proxmox host clean and gives containerized services their own dedicated Linux environment.

If this VM is down, many services are affected, so it is one of the highest-priority systems in the environment.

### Docker

Docker is the container runtime for most services.

Using Docker makes it easier to deploy, update, organize, and eventually document services. It also lets me group applications by purpose while keeping them separated from the base operating system.

### Portainer

Portainer gives me a management view into Docker.

I use it for visibility into containers, images, networks, and volumes. It is useful for day-to-day management, but Docker itself remains the underlying runtime.

## Visibility and Monitoring Services

### Homepage

Homepage is the central launchpad for the homelab.

This is one of the services that makes the environment feel polished and usable. Instead of remembering every service URL, I can organize everything into sections such as:

- Core Infrastructure
- Home
- Docker Services
- Media Stack

Homepage now includes links and visibility for services like Home Assistant, Donetick, Donetick Docker metrics, and Tailscale Machines.

### Uptime Kuma

Uptime Kuma provides service health monitoring.

I use it to quickly see whether important services are reachable. This gives me a simple status view without having to manually check every service.

It is not a full monitoring platform yet, but it gives the environment a much better operational baseline.

## Network and Access Services

### Pi-hole

Pi-hole provides DNS and ad-blocking capabilities.

In my environment, Pi-hole supports internal name resolution and network-level filtering. It is part of making the homelab feel more integrated instead of just being a group of services accessed by IP address.

### Nginx Proxy Manager

Nginx Proxy Manager provides a reverse proxy management layer.

I use it where cleaner service access patterns are useful. It gives me practice managing reverse proxy concepts without manually writing every Nginx config from scratch.

### Tailscale

Tailscale provides private remote access.

This is one of the most important design choices in the environment. I want to be able to reach my services remotely without exposing administrative dashboards directly to the public internet.

Tailscale gives me a practical private access path for management.

## Home Automation Services

### Home Assistant OS

Home Assistant OS is the household automation platform.

It runs separately from the Docker VM because it has become its own major part of the environment. It supports dashboards, calendar visibility, chore workflows, and future automation ideas.

Current use cases include:

- Family dashboard views
- Google Calendar visibility
- Donetick chore integration
- Daily and weekly chore workflows
- Completed-today chore summaries
- Household status visibility

### Donetick

Donetick provides recurring chore management.

It runs in Docker, while Home Assistant provides the dashboard and automation layer around it. This combination gives me a self-hosted household task system that can grow over time.

### Google Calendar Integration

Google Calendar is integrated with Home Assistant.

This allows household scheduling information to appear alongside other dashboard data. It is one of the pieces that makes Home Assistant useful as a family-facing dashboard rather than just a technical control panel.

## Media Services

### Plex

Plex provides media server functionality.

It is part of the Docker-hosted media stack.

### Seerr

Seerr provides a media request frontend.

It supports a cleaner request workflow for the media stack.

### Radarr

Radarr manages movie automation.

### Sonarr

Sonarr manages TV automation.

### Prowlarr

Prowlarr manages indexer coordination.

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

The Debian Docker VM is the main dependency for most self-hosted services.

If the Debian Docker VM is offline, these services are affected:

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

If Home Assistant OS is offline, household dashboards and automations are affected, but the general Docker service stack can continue running.

If Proxmox is offline, all VM-hosted workloads are affected.

## What This Map Helps With

This service map helps me quickly understand what depends on what.

It also makes the project easier to explain to someone reviewing the repo. Instead of just saying I run a bunch of containers, this document shows the role each service plays in the environment.
