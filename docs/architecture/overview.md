# Architecture Overview

The homelab is organized around clear workload boundaries rather than placing every service on one host.

## Platform Layout

### Proxmox VE

The Dell Precision T7820 runs Proxmox VE and remains focused on virtualization. General applications run inside dedicated workloads.

### Debian Docker VM

The Debian VM hosts containerized infrastructure, monitoring, media, and household services, including:

- Portainer, Homepage, Uptime Kuma, Pi-hole, and Nginx Proxy Manager
- Plex, Seerr, Radarr, Sonarr, Prowlarr, and SABnzbd
- Donetick
- The planned Hermes application

### Home Assistant OS VM

Home Assistant runs separately because it is the household operations platform. It owns dashboard presentation, calendars, chores, lists, devices, people, automations, and service execution.

This isolation reduces the chance that Docker, media, or AI experimentation disrupts family-facing workflows.

## Responsibility Boundaries

| Component | Responsibility |
|---|---|
| Proxmox | Virtualization and workload isolation |
| Debian VM | Docker service hosting |
| Home Assistant | Household state, automation, and dashboards |
| Donetick | Chore recurrence, assignment, and history |
| Tailscale | Private remote administration |
| Uptime Kuma | Availability monitoring |
| Homepage | Navigation and selected status visibility |
| Hermes | Planned interpretation, summaries, and approved coordination |

Hermes will not become a second household database. Home Assistant remains authoritative, while Hermes receives a curated subset of context through a dedicated account and token. The first phase is read-only, with secrets stored outside Git.

## Primary Data Flows

### Chores

```text
Home Assistant form -> REST command -> Donetick API -> Home Assistant integration -> dashboard
```

### Work schedules

```text
Dashboard inputs -> Browser Mod confirmation -> Home Assistant script
-> detailed calendar + all-day display calendar -> dashboard views
```

### Hermes summary

```text
Home Assistant entities -> curated adapter -> Hermes
-> summary entity/helper -> Home dashboard
```

The Hermes flow is one-way during the initial phase.

## Access, Monitoring, and Backup

Administrative access uses Tailscale rather than public management endpoints. Plex has the limited public exposure needed for remote playback.

Uptime Kuma monitors availability; Homepage provides navigation. A backup script protects selected Docker configuration and metadata, but restore testing remains incomplete.

## Current Priorities

- Validate restores and improve off-host backup handling.
- Add firewall hardening on the Debian VM.
- Improve monitoring beyond reachability checks.
- Deploy Hermes with read-only Home Assistant access.
- Add controlled authorization before any Hermes write actions.

Service-specific details belong in the project folders and [`service-map.md`](service-map.md).