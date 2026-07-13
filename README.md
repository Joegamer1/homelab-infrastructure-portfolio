# Homelab Infrastructure Portfolio

This repository documents my personal homelab and household infrastructure environment.

What started as a traditional home server and media project has grown into a working infrastructure and household operations platform. It now supports self-hosted services, monitoring, private remote access, media delivery, family scheduling, recurring chores, dashboards, backups, and an AI-assisted household coordination project.

The purpose of this repository is not to present a perfect lab or a generic installation guide. It documents what I built, why the architecture changed, where early approaches failed, how I isolated problems, how I validated fixes, and what remains incomplete.

## Project Story

The environment developed in stages:

1. Establish Proxmox as a clean virtualization foundation.
2. Build a Debian VM for Docker-based services.
3. Add DNS, monitoring, reverse proxy management, service navigation, and media applications.
4. Configure private remote administration through Tailscale.
5. Add Plex GPU passthrough and hardware transcoding.
6. Deploy Home Assistant as a separate household operations platform.
7. Integrate family calendars, work schedules, personal lists, shopping, and Donetick chores.
8. Refine mobile layouts and workflows based on real household feedback.
9. Build a safer shift-entry workflow with separate detailed and display calendars.
10. Define Hermes as a separate AI coordination layer that reads from Home Assistant through a dedicated, restricted account.
11. Add backup planning and deeper operational documentation.

The full progression is documented in [`docs/operations/project-evolution.md`](docs/operations/project-evolution.md).

## Environment Overview

The environment runs on a Dell Precision T7820 with Proxmox VE.

### Proxmox VE

Proxmox provides the virtualization layer and remains focused on hosting and managing workloads.

### Debian Docker VM

The Debian VM is the primary container host. It runs infrastructure, monitoring, DNS, dashboard, media, household support services, and will host Hermes separately under `/srv/docker/hermes`.

### Home Assistant OS VM

Home Assistant runs separately because it has become the family-facing household operations layer. Keeping it isolated reduces the chance that media-stack, Docker, or AI experimentation will affect shared dashboards and workflows.

## Current Services

### Infrastructure and Operations

- Docker and Docker Compose
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

### Household Operations

- Home Assistant OS
- Google Calendar integration
- Family dashboard
- Daylight Calendar Card
- Work Week schedule workflow
- Detailed and simplified work calendars
- Donetick
- Rolling daily and weekly chore creation
- Chore assignment and completion visibility
- Personal todo lists
- Shopping dashboard

### AI Coordination Project

- Hermes planned as a separate Docker application
- Home Assistant retained as the source of truth
- Dedicated Home Assistant user and long-lived token
- Read-only integration as the first deployment phase
- Curated data access rather than exposing every Home Assistant service
- First visible feature: a calm, cozy household hero summary on the Home dashboard

## Selected Projects

### [Plex Hardware Transcoding](docs/projects/plex-hardware-transcoding/)

This project covers Intel IOMMU, VFIO binding, PCI passthrough, NVIDIA container access, and Plex playback validation.

### [Home Assistant Family Dashboard](docs/projects/home-assistant-family-dashboard/)

The family dashboard is designed as a household application landing page rather than a default administration interface. It now links calendars, chores, work schedules, personal lists, shopping, and the future Hermes-generated hero summary.

### [Work Week and Calendar Design](docs/projects/home-assistant-work-week/)

The schedule system separates accurate data from simplified presentation.

- Detailed calendars retain exact shift start and end times.
- Overnight shifts correctly end on the following date.
- Display calendars create one all-day entry on the date the shift begins.
- Browser Mod provides the shift-entry popup.
- Shift inputs default to midnight instead of the current time.
- A date-picker month-label defect remains an upstream interface limitation.

### [Home Assistant Chores](docs/projects/home-assistant-chores/)

Donetick remains the task source of truth while Home Assistant provides the household interface.

The workflow includes rolling recurrence, assignment, REST-based task creation, helper reset, completion feedback, and targeted Card Mod styling. The request payload now explicitly sets rolling behavior so recurring chores advance from completion instead of behaving like fixed-date tasks.

### [House Brain / Hermes](docs/projects/house-brain-hermes/)

Hermes is the reasoning and coordination layer planned for the Debian Docker VM. Home Assistant remains authoritative for calendars, chores, shopping lists, people, devices, and services.

The first implementation phase is intentionally read-only. Hermes will use a dedicated Home Assistant account, keep secrets in an untracked `.env` file, and consume a curated household context rather than receiving unrestricted service access.

### [Homepage Dashboard](docs/projects/homepage-dashboard/)

Homepage provides navigation and selected status visibility. Uptime Kuma remains responsible for availability monitoring.

## Real User Feedback

Some of the most important changes came after the systems were used by other people.

Examples include:

- Redesigning overnight work entries after they appeared to occupy two days.
- Simplifying the Home Assistant landing page because the first version felt like a configuration screen.
- Making secondary buttons smaller so they did not compete with the schedule.
- Hiding the default todo creation path because it bypassed recurrence and assignment logic.
- Resetting helper inputs so old values were not carried into the next chore.
- Reverting mobile calendar margin experiments that improved portrait mode but harmed landscape layout.
- Defaulting new shift times to `00:00` instead of the current clock time.
- Keeping fragile custom-card controls unchanged when CSS overrides proved unreliable.

These changes demonstrate a recurring principle in the project: technically valid behavior is not always good operational behavior.

## Validation Practices

Changes are validated through the layer closest to the behavior being tested.

Examples:

- `docker ps` and health state after container recreation
- Live mount inspection before backup or configuration work
- Direct DNS resolution testing
- Tailscale machine and address verification
- Plex playback details read separately for video and audio
- GPU ownership and visibility checked across host, VM, container, and application
- Same-day and overnight calendar test cases
- Browser Mod popup and script execution tested independently
- Daily and weekly rolling chore creation tests
- Visible Card Mod tests before narrowing selectors
- Portrait and landscape dashboard regression testing
- Backup archive size and content review

## Troubleshooting Narrative

The repository includes both recent and historical troubleshooting writeups:

- [`docs/operations/historical-build-and-troubleshooting.md`](docs/operations/historical-build-and-troubleshooting.md)
- [`docs/operations/recent-project-troubleshooting.md`](docs/operations/recent-project-troubleshooting.md)

These documents cover failed assumptions, investigation steps, final resolutions, and lessons learned across Proxmox, Docker, networking, Plex, backups, Home Assistant, calendars, chores, and the Hermes design.

## Known Limitations

The environment is functional, but several areas remain intentionally marked incomplete:

- Backup restore testing has not yet been completed.
- Off-host backup handling should become more formal.
- Some storage paths reflect earlier deployments rather than one perfect convention.
- Monthly and quarterly recurring chore creation are not implemented.
- The simplified work display calendar omits exact shift times by design.
- Individual shift deletion from the Work Week card is still planned.
- The date picker can display a stale month label after month navigation.
- Shopping List Card integration is not yet fully documented.
- Hermes has been architected but is not yet deployed.
- Monitoring is availability-focused rather than full observability.
- Current-state screenshots still need to be sanitized and added.

## Security and Privacy

The repository uses sanitized documentation and examples.

It does not include passwords, API tokens, private keys, personal calendar data, sensitive household details, raw production configuration, or unnecessary internal addresses.

Hermes-specific secrets will be stored in an `.env` file excluded from Git. The initial Home Assistant integration will use a dedicated account and read-only access wherever possible.

## Skills Demonstrated

- Proxmox virtualization
- Linux administration
- Docker and Docker Compose
- PCI passthrough and VFIO
- NVIDIA container integration
- Plex playback diagnostics
- Private remote access
- DNS and service networking
- Monitoring and operational visibility
- Home Assistant automation
- REST API integration
- Calendar and time-range modeling
- Browser Mod workflow integration
- User-focused dashboard design
- Responsive UI troubleshooting
- AI integration architecture and least-privilege planning
- Backup planning
- Cross-layer troubleshooting
- Operational documentation

## Documentation

Start with:

- [`docs/README.md`](docs/README.md)
- [`docs/operations/project-evolution.md`](docs/operations/project-evolution.md)
- [`docs/projects/README.md`](docs/projects/README.md)

The value of this project is not simply that it runs a list of tools. The value is in how the pieces fit together, how problems were narrowed, and how the environment changed after real use exposed better requirements.