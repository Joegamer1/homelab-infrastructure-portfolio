# Architecture Overview

This document explains how my homelab is structured and why I organized it this way.

The environment supports real services used at home while providing a platform for learning infrastructure, operations, automation, AI integration, and eventually more advanced security lab work.

## Architecture Goals

- Keep the Proxmox host focused on virtualization.
- Use a dedicated Debian VM for Docker-based services.
- Keep Home Assistant separate from the general Docker stack.
- Keep Home Assistant authoritative for household data and automation.
- Run Hermes separately as an interpretation and coordination layer.
- Use private remote access instead of exposing management interfaces publicly.
- Make service health visible through monitoring.
- Document enough context to troubleshoot and improve the environment over time.
- Build a foundation that can later support a dedicated cybersecurity lab.

This is not an enterprise design copied into a home environment. It is a practical infrastructure design for a real homelab.

## Physical and Virtualization Layer

The environment runs on a Dell Precision T7820 workstation with Proxmox VE.

Current major virtual machines include:

- Debian Docker VM
- Home Assistant OS VM

Proxmox remains focused on hosting and separating workloads rather than running general applications directly on the hypervisor.

## Debian Docker VM

The Debian VM is the main container host.

Current services include:

- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd
- Donetick

Hermes will also run here as a separate Docker application under `/srv/docker/hermes`.

Keeping Docker workloads off the Proxmox host provides one managed service layer without turning the hypervisor into an application server.

## Home Assistant OS VM

Home Assistant OS runs as a dedicated VM because it has become the household operations platform rather than an ordinary containerized service.

Current Home Assistant responsibilities include:

- Family dashboard views
- Google Calendar integration
- Work Week schedule entry and display
- Detailed and simplified work calendar entities
- Donetick chore integration
- Rolling recurring chore creation
- Active and completed chore visibility
- Personal todo lists
- Shopping dashboard
- Household notifications
- Future automation execution for Hermes-approved actions

Keeping Home Assistant separate reduces coupling between household workflows and Docker, media, or AI experimentation.

## Hermes AI Coordination Layer

Hermes is planned as a separate application on the Debian VM.

Its responsibility is interpretation and coordination, not ownership of household data.

### Home Assistant remains authoritative for:

- Calendars and events
- Chores and completion state
- Shopping and todo lists
- People and presence
- Devices and entities
- Automations and services
- Dashboard presentation

### Hermes will handle:

- Natural-language interpretation
- Context selection
- Household summaries
- Reasoning across multiple Home Assistant domains
- Coordination of individually approved higher-level actions

The first deployment phase will be read-only. Hermes will use a dedicated Home Assistant user and long-lived token. Secrets will be stored in an untracked `.env` file.

The model will not receive unrestricted access to every Home Assistant service. A curated adapter will expose only the data and actions required for each supported workflow.

## Logical Service Layers

### Physical and Virtualization

- Dell Precision T7820
- Proxmox VE
- Debian Docker VM
- Home Assistant OS VM

### Service Hosting

- Debian Linux
- Docker
- Portainer

### Network and Access

- Pi-hole
- Nginx Proxy Manager
- Tailscale

### Visibility

- Homepage
- Uptime Kuma

### Household Operations

- Home Assistant
- Donetick
- Google Calendar
- Work Week and display calendars
- Todo and shopping entities

### AI Interpretation and Coordination

- Hermes
- Curated Home Assistant API adapter
- Future summary and action policies

### Media Services

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

## Primary Data Flows

### Household dashboard flow

```text
Household user -> Home Assistant dashboard -> Home Assistant entities/services
```

### Chore flow

```text
Home Assistant form -> REST command -> Donetick API -> Donetick integration -> Home Assistant dashboard
```

### Work schedule flow

```text
Browser Mod popup -> Home Assistant script -> detailed calendar + display calendar -> dashboard views
```

### Hermes summary flow

```text
Home Assistant entities -> curated context adapter -> Hermes -> summary entity/helper -> Home dashboard hero card
```

The summary path is intentionally one-way during the first phase.

## Remote Access Model

Remote access is handled through Tailscale. Administrative dashboards are not intentionally exposed directly to the public internet.

## Monitoring Model

Uptime Kuma provides service availability monitoring. Homepage provides a central launchpad and selected service information.

This is practical operational visibility rather than a full observability stack.

## Backup Approach

A backup script exists for selected Docker and infrastructure configuration paths. Restore testing remains planned, so the backup process is implemented but not yet fully proven.

Hermes configuration should follow the same backup approach while excluding tokens, API keys, and other secrets from both Git and unsafe archives.

## Security Approach

Current controls and decisions include:

- SSH hardening
- Tailscale private remote access
- Avoiding public exposure for administrative services
- Workload separation between Proxmox, Debian, and Home Assistant
- Sanitized public documentation
- Dedicated Hermes identity and token
- Read-only AI integration as the first phase
- Secrets kept outside Git
- No unrestricted Home Assistant service catalog exposed to the model

Planned improvements include:

- UFW or equivalent firewall hardening
- Restore testing
- Better monitoring and alerting
- Formal action authorization for Hermes
- A separate future cybersecurity lab project

## Why This Design Works

Proxmox handles virtualization. Debian handles containerized services. Home Assistant handles household state and automation. Hermes will interpret and coordinate without becoming a second source of truth. Tailscale handles private remote access. Uptime Kuma handles availability visibility. Homepage ties infrastructure services together as a launchpad.

That separation makes failures easier to isolate and allows the environment to grow without giving every new component unrestricted control.

## Current Status

The core infrastructure and household workflows are functional and actively maintained. Hermes has a defined deployment, trust, and responsibility model but is not yet deployed.

The next major improvements are Hermes read-only integration, the Home dashboard hero summary, firewall hardening, restore testing, stronger monitoring, and sanitized architecture diagrams.