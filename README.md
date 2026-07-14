# Homelab Infrastructure Portfolio

This repository documents the homelab and household infrastructure I operate on a Dell Precision T7820 running Proxmox VE.

The environment includes a Debian Docker host, Home Assistant OS, private remote access, monitoring, DNS, media services, recurring household workflows, backups, and a planned AI coordination layer. The documentation focuses on design decisions, troubleshooting, validation, and lessons from operating systems that other people rely on.

## Architecture

- **Proxmox VE** provides virtualization and workload isolation.
- **Debian** hosts Docker-based infrastructure, media, monitoring, and household services.
- **Home Assistant OS** runs separately as the household operations platform.
- **Tailscale** provides private administrative access.
- **Hermes** is planned as a separate, initially read-only AI coordination layer.

See [`docs/architecture/overview.md`](docs/architecture/overview.md) and [`docs/architecture/service-map.md`](docs/architecture/service-map.md).

## Projects

- [Plex hardware transcoding](docs/projects/plex-hardware-transcoding/)
- [Home Assistant family dashboard](docs/projects/home-assistant-family-dashboard/)
- [Work Week calendar workflow](docs/projects/home-assistant-work-week/)
- [Donetick chore workflow](docs/projects/home-assistant-chores/)
- [Homepage dashboard](docs/projects/homepage-dashboard/)
- [Hermes house brain](docs/projects/house-brain-hermes/)

Each project folder contains the implementation notes and sanitized configuration examples specific to that system.

## Operations and Security

- [Project evolution](docs/operations/project-evolution.md)
- [Historical troubleshooting](docs/operations/historical-build-and-troubleshooting.md)
- [Recent troubleshooting](docs/operations/recent-project-troubleshooting.md)
- [Backup plan](docs/operations/backup-plan.md)
- [Restore notes](docs/operations/restore-notes.md)
- [Security model](docs/security/security-model.md)

## Current State

The core virtualization, Docker, media, monitoring, DNS, remote-access, Home Assistant, calendar, and chore workflows are operational. Current priorities are restore testing, stronger firewall and monitoring controls, per-shift calendar deletion, additional chore recurrence options, and the read-only Hermes integration.

## Documentation Boundaries

Public examples are sanitized. They exclude credentials, private keys, tokens, personal calendar data, sensitive household details, and unnecessary internal addressing.

Start with the [documentation index](docs/README.md).