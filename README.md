# My homelab

I run my homelab on a Dell Precision T7820 with Proxmox VE. Most services live in a Debian Docker VM, with Home Assistant OS in its own VM.

It hosts our media server, DNS, monitoring, calendars, chores, and other household tools. This repo is where I keep the setup notes, configuration examples, fixes, and unfinished work.

## Setup

- **Proxmox VE:** runs the VMs.
- **Debian:** hosts Docker services for media, DNS, monitoring, and household tools.
- **Home Assistant OS:** runs the dashboards and automations in a separate VM.
- **Tailscale:** gives me private remote access for administration.
- **Hermes:** a planned AI assistant for household summaries, starting with read-only access.

See the [architecture overview](docs/architecture/overview.md) and [service map](docs/architecture/service-map.md).

## Projects

- [Plex server](docs/projects/plex-server/): [media automation](docs/projects/plex-server/media-automation/), [subtitle processing](docs/projects/plex-server/subtitle-processing/), and [hardware transcoding](docs/projects/plex-server/hardware-transcoding/)
- [Home Assistant family dashboard](docs/projects/home-assistant-family-dashboard/)
- [Work Week calendar](docs/projects/home-assistant-work-week/)
- [Donetick chores](docs/projects/home-assistant-chores/)
- [Homepage dashboard](docs/projects/homepage-dashboard/)
- [Hermes plans](docs/projects/house-brain-hermes/)

## Running and fixing it

- [How the setup changed over time](docs/operations/project-evolution.md)
- [Earlier troubleshooting](docs/operations/historical-build-and-troubleshooting.md)
- [Recent troubleshooting](docs/operations/recent-project-troubleshooting.md)
- [Backup plan](docs/operations/backup-plan.md)
- [Restore notes](docs/operations/restore-notes.md)
- [Security settings and remaining work](docs/security/security-model.md)

The core services, calendars, and chore workflows are running. Restore testing, firewall work, better monitoring, per-shift calendar deletion, more recurrence options for chores, and Hermes are still on the list.

## Public examples

The examples use placeholders for credentials, addresses, and household data. They're here to explain the setup without publishing my private configuration.

The [documentation index](docs/README.md) lists the rest of the notes.
