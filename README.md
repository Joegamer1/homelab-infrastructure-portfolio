# Homelab Infrastructure Portfolio

This repository documents my personal homelab and household infrastructure environment.

What started as a traditional home server and media project has grown into a working infrastructure and household operations platform. It now supports self-hosted services, monitoring, private remote access, media delivery, family scheduling, recurring chores, dashboards, backups, and ongoing automation work.

The purpose of this repository is not to present a perfect lab or a generic installation guide. It documents what I built, why the architecture changed, where early approaches failed, how I isolated problems, how I validated fixes, and what remains incomplete.

## Project Story

The environment developed in stages:

1. Establish Proxmox as a clean virtualization foundation.
2. Build a Debian VM for Docker-based services.
3. Add DNS, monitoring, reverse proxy management, service navigation, and media applications.
4. Configure private remote administration through Tailscale.
5. Add Plex GPU passthrough and hardware transcoding.
6. Deploy Home Assistant as a separate household operations platform.
7. Integrate family calendar, work schedules, and Donetick chores.
8. Refine workflows based on real household feedback.
9. Add backup planning and deeper operational documentation.

The full progression is documented in [`docs/operations/project-evolution.md`](docs/operations/project-evolution.md).

## Environment Overview

The environment runs on a Dell Precision T7820 with Proxmox VE.

### Proxmox VE

Proxmox provides the virtualization layer and remains focused on hosting and managing workloads.

### Debian Docker VM

The Debian VM is the primary container host. It runs infrastructure, monitoring, DNS, dashboard, media, and household support services.

### Home Assistant OS VM

Home Assistant runs separately because it has become the family-facing household operations layer. Keeping it isolated reduces the chance that media-stack or Docker experimentation will affect shared dashboards and workflows.

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
- Work Week schedule workflow
- Detailed and simplified work calendars
- Donetick
- Daily and weekly recurring chore creation
- Chore assignment and completion visibility

## Selected Projects

### [Plex Hardware Transcoding](docs/projects/plex-hardware-transcoding/)

This project covers:

- Intel IOMMU configuration
- NVIDIA GPU identification
- VFIO binding
- PCI passthrough to the Debian VM
- NVIDIA container access
- Plex hardware-transcoding validation
- Direct Play, Direct Stream, and audio/video transcode diagnosis

### [Home Assistant Family Dashboard](docs/projects/home-assistant-family-dashboard/)

The family dashboard is designed as a household application landing page rather than a default administration interface.

The design emphasizes:

- Clear primary destinations
- Compact secondary actions
- Minimal instructional clutter
- Consistent navigation
- Shared-display readability

### [Work Week and Calendar Design](docs/projects/home-assistant-work-week/)

The schedule system separates accurate data from simplified presentation.

- Detailed calendars retain exact shift start and end times.
- Overnight shifts correctly end on the following date.
- Display calendars create one all-day entry on the date the shift begins.
- No second household-facing work entry is created because a shift ends after midnight.

This design came from real user feedback: the first implementation was technically correct but visually misleading.

### [Home Assistant Chores](docs/projects/home-assistant-chores/)

Donetick remains the task source of truth while Home Assistant provides the household interface.

The workflow includes:

- Active chore visibility
- Daily and weekly recurring creation
- Assignee selection
- REST-based task creation
- Helper reset after submission
- Completion feedback
- Removal of controls that bypass recurrence logic
- Targeted Card Mod styling

### [Homepage Dashboard](docs/projects/homepage-dashboard/)

Homepage provides navigation and selected status visibility. Uptime Kuma remains responsible for availability monitoring.

The project includes troubleshooting of host validation, active bind mounts, service grouping, and actual container naming.

## Real User Feedback

Some of the most important changes came after the systems were used by other people.

Examples include:

- Redesigning overnight work entries after they appeared to occupy two days.
- Simplifying the Home Assistant landing page because the first version felt like a configuration screen.
- Making secondary buttons smaller so they did not compete with the schedule.
- Hiding the default todo creation path because it bypassed recurrence and assignment logic.
- Resetting helper inputs so old values were not carried into the next chore.

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
- Daily and weekly chore creation tests
- Visible Card Mod tests before narrowing selectors
- Backup archive size and content review

## Troubleshooting Narrative

The repository includes both recent and historical troubleshooting writeups:

- [`docs/operations/historical-build-and-troubleshooting.md`](docs/operations/historical-build-and-troubleshooting.md)
- [`docs/operations/recent-project-troubleshooting.md`](docs/operations/recent-project-troubleshooting.md)

These documents cover failed assumptions, investigation steps, final resolutions, and lessons learned across Proxmox, Docker, networking, Plex, backups, Home Assistant, calendars, and chores.

## Known Limitations

The environment is functional, but several areas remain intentionally marked incomplete:

- Backup restore testing has not yet been completed.
- Off-host backup handling should become more formal.
- Some storage paths reflect earlier deployments rather than one perfect convention.
- Monthly recurring chore creation is not implemented.
- The simplified work display calendar omits exact shift times by design.
- Monitoring is availability-focused rather than full observability.
- Current-state screenshots still need to be sanitized and added.

## What I Would Do Differently

If rebuilding from the beginning, I would:

- Standardize `/srv/docker/<service>` earlier.
- Define container naming conventions before deployment.
- Inventory bind mounts and named volumes before writing backups.
- Capture sanitized screenshots at major milestones.
- Separate detailed and display calendar entities earlier.
- Record validation steps during implementation rather than reconstructing them later.

## Security and Privacy

The repository uses sanitized documentation and examples.

It does not include:

- Passwords
- API tokens
- Private keys
- Personal calendar data
- Sensitive household details
- Raw production configuration
- Unnecessary internal or tailnet information

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
- User-focused dashboard design
- Backup planning
- Cross-layer troubleshooting
- Operational documentation

## Documentation

Start with:

- [`docs/README.md`](docs/README.md)
- [`docs/operations/project-evolution.md`](docs/operations/project-evolution.md)
- [`docs/projects/README.md`](docs/projects/README.md)

The value of this project is not simply that it runs a list of tools. The value is in how the pieces fit together, how problems were narrowed, and how the environment changed after real use exposed better requirements.
