# Homelab Infrastructure Project

This repository documents my personal homelab and household infrastructure environment.

What started as a practical home server project has become a working infrastructure platform for self-hosted services, monitoring, private remote access, home automation, backups, media services, and day-to-day household workflows. The environment supports real services that I use, maintain, troubleshoot, and improve over time.

The goal of this repository is to present the project in a professional, portfolio-ready way while keeping it grounded in my actual environment. This is not a generic best-practices checklist and it is not a dump of raw production configuration files. It documents what I built, why I built it this way, where the first approach failed, and how I improved it.

This repository focuses on infrastructure and operations. Future cybersecurity-specific work, such as SIEM deployment, detection engineering, log analysis, attack simulation, and incident writeups, will be documented separately as a dedicated security lab project built on top of this environment.

## Project Purpose

The purpose of this homelab is to build and operate a real infrastructure environment that is useful at home and valuable for career development.

This project demonstrates hands-on experience with:

- Proxmox VE virtualization
- Linux server administration
- Docker-based service hosting
- Internal service monitoring
- Private remote access
- DNS and reverse proxy management
- Home automation
- REST API integration
- Backup planning
- Infrastructure hardening
- Operational documentation
- Practical troubleshooting
- User-focused dashboard design

The most important part of this project is that it is real. These are not isolated lab exercises. The services documented here support actual household workflows and infrastructure needs.

## Environment Overview

The environment is built around a Dell Precision T7820 workstation running Proxmox VE.

Proxmox provides the virtualization layer. It hosts multiple virtual machines, including:

- A Debian Docker VM used as the main service host
- A Home Assistant OS VM used for household automation and dashboarding

The Debian Docker VM runs most self-hosted services, including dashboards, monitoring, DNS, reverse proxy management, media services, Donetick, and the planned House Brain assistant stack.

Home Assistant provides the household automation layer. It integrates with Google Calendar and Donetick to support family dashboard views, work schedule visibility, recurring chores, task completion feedback, and household coordination.

## Core Infrastructure

### Proxmox VE

Proxmox VE is the foundation of the environment.

I use it to separate major workloads into virtual machines instead of running everything directly on one operating system. This provides cleaner boundaries and makes the environment easier to expand, troubleshoot, and recover.

### Debian Docker VM

The Debian Docker VM is the primary service host.

It runs the majority of Docker-based applications in the environment, including infrastructure services, dashboards, monitoring tools, media services, Donetick, and future assistant services.

I chose this approach because it keeps the Proxmox host focused on virtualization while giving Docker services their own dedicated Linux environment.

### Home Assistant OS VM

Home Assistant OS runs as its own VM.

I chose to keep Home Assistant separate from the general Docker stack because it functions more like a household operations platform than a standard containerized service. Running it in its own VM keeps home automation isolated, simplifies maintenance, and reduces the chance that changes to other Docker services will affect household dashboards and automations.

## Current Services

### Infrastructure and Operations

- Docker
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

### Home Automation and Household Operations

- Home Assistant OS
- Google Calendar integration
- Donetick
- Home Assistant family dashboard
- Work Week schedule dashboard
- Detailed and simplified work calendar entities
- Home Assistant chore dashboard
- Daily and weekly recurring chore creation
- Donetick-backed chore completion notifications

## Home Assistant and Household Workflows

One of the most active parts of this environment is the Home Assistant household operations work.

### Family Dashboard

The main Home Assistant page is being developed as a family application landing page rather than a default administration dashboard.

It provides clear navigation to:

- Family Calendar
- Chores
- Work Week
- Additional household views as they are added

Recent work improved the visual hierarchy, card layout, navigation consistency, and distinction between prominent destination cards and smaller secondary controls.

### Work Week and Calendar Design

The work schedule workflow now separates accurate shift storage from simplified household display.

- Detailed work calendars store real shift start and end times.
- Overnight shifts correctly end on the following date.
- Separate display calendars create a single all-day entry on the date a shift begins.
- An overnight shift does not create a second household-facing work entry on the following day.
- Optional shift notes are only added when a meaningful note is present.

This solved a practical calendar problem: technically correct overnight events appeared to occupy two days in calendar views, even though the family wanted one clear indicator on the day the shift started.

### Chores and Donetick

The chores workflow uses Donetick as the source of truth and Home Assistant as the household-facing interface.

The current implementation includes:

- Active chore visibility
- Daily recurring chore creation
- Weekly recurring chore creation
- Assignee selection
- Completed-today summaries
- Completion notifications
- A REST-command bridge for workflow features not exposed directly by the integration
- Helper reset and confirmation behavior after submissions
- Card Mod styling that hides the built-in `Active` label without hiding chore names
- Removal of the default todo add-item interface so new chores use the intended recurring workflow

This part of the project turns the homelab into something useful beyond the server rack. It supports daily household coordination while also exercising API integration, state handling, UI design, and troubleshooting.

## Recent Troubleshooting Highlights

Recent work included several design and implementation problems that required iterative testing:

- Overnight calendar events appeared on both the starting and ending day.
- One calendar entity could not simultaneously provide exact shift data and the desired simplified visual display.
- Empty Home Assistant helper states could create poor calendar descriptions.
- Donetick entity visibility did not provide the complete recurring-chore creation workflow.
- Recurrence payloads required careful API request formatting.
- Input helpers retained stale values after submission.
- Broad Card Mod selectors hid chore names along with the unwanted card header.
- Built-in todo controls bypassed assignment and recurrence logic.
- Default dashboard buttons were too visually heavy for secondary actions.
- The main Home Assistant page needed to look and behave more like a household application.

The consolidated investigation steps, resolutions, results, and lessons are documented in:

- `docs/operations/recent-project-troubleshooting.md`

## Architecture Summary

At a high level, the environment is organized into four layers:

1. **Physical and virtualization layer**
   - Dell Precision T7820
   - Proxmox VE
   - VM separation for major workloads

2. **Service hosting layer**
   - Debian Docker VM
   - Docker and Portainer
   - Infrastructure, monitoring, DNS, reverse proxy, media, household, and future assistant services

3. **Home automation layer**
   - Home Assistant OS VM
   - Google Calendar integration
   - Donetick integration
   - Household dashboards and workflow scripts

4. **Access and visibility layer**
   - Tailscale for private remote access
   - Homepage for centralized service navigation
   - Uptime Kuma for service health visibility
   - Home Assistant for family-facing operational visibility

This structure keeps the environment understandable as it grows.

## Completed Work

Completed work includes:

- Proxmox VE deployed on Dell Precision T7820 hardware
- Debian Docker VM created as the primary service host
- Docker service stack deployed
- Portainer configured for Docker visibility
- Homepage deployed and organized into useful service sections
- Uptime Kuma deployed for service monitoring
- Pi-hole deployed for DNS and ad-blocking
- Nginx Proxy Manager deployed as a reverse proxy layer
- Tailscale configured for private remote access
- Plex/Arr media stack deployed
- Home Assistant OS deployed in Proxmox
- Google Calendar integrated with Home Assistant
- Home Assistant family dashboard created and visually refined
- Work Week schedule entry and dashboard workflow implemented
- Overnight shift handling implemented
- Separate detailed and display work calendars designed
- Donetick deployed in Docker
- Donetick integrated with Home Assistant
- Chore dashboard created in Home Assistant
- Daily and weekly chore creation workflows implemented
- REST-based Donetick task creation implemented
- Donetick-backed chore completion notifications configured
- Todo card display cleaned up with targeted Card Mod styling
- Homepage expanded with Home Assistant, Donetick, Docker metrics, Tailscale Machines, and organized service sections
- Backup script created for selected Docker and infrastructure configuration paths
- SSH hardening completed
- Recent project troubleshooting documented

## Planned Improvements

Planned work includes:

- UFW/firewall hardening
- Restore testing from backups
- More formal architecture diagrams
- Improved monitoring and alerting
- Better documentation of recovery procedures
- House Brain/Hermes local AI assistant layer
- Typed household summary prototype using Home Assistant calendar data
- Alexa bridge for voice-assisted household workflows
- Additional Home Assistant dashboard refinements
- Evaluation of whether unused calendar view modes can be hidden from the shared display
- Monthly recurring chore creation
- Sanitized configuration examples and screenshots
- Dedicated future cybersecurity lab repository built on top of this infrastructure

UFW/firewall hardening and calendar view hiding are documented as planned work and should not be represented as completed until implemented and tested.

## Security and Privacy

This repository uses sanitized documentation and example files.

It does not include production secrets, raw configuration files, private keys, API tokens, passwords, personal calendar data, private household details, or sensitive screenshots.

Example files use placeholder values such as:

- `CHANGEME`
- `LOCAL_IP`
- `SERVICE_URL`
- `API_TOKEN_HERE`
- `HOMELAB_HOST`
- `HOME_ASSISTANT_URL`
- `DONETICK_HOST`

## Skills Demonstrated

This project demonstrates practical experience with:

- Linux administration
- Virtualization
- Docker and containerized services
- Service monitoring
- Internal DNS
- Reverse proxy management
- Private remote access
- Home automation
- REST API integration
- Calendar and time-range modeling
- Dashboard and workflow design
- Backup planning
- Infrastructure troubleshooting
- Operational documentation
- Security-conscious system design

## Documentation

Start here:

- `docs/README.md`
- `docs/projects/README.md`

Current documentation includes:

- `docs/architecture/overview.md`
- `docs/architecture/service-map.md`
- `docs/security/security-model.md`
- `docs/operations/backup-plan.md`
- `docs/operations/maintenance-notes.md`
- `docs/operations/restore-notes.md`
- `docs/operations/recent-project-troubleshooting.md`
- `docs/projects/home-assistant-family-dashboard/README.md`
- `docs/projects/home-assistant-work-week/README.md`
- `docs/projects/home-assistant-chores/README.md`
- `docs/projects/house-brain-hermes/README.md`
- `docs/projects/homepage-dashboard/README.md`
- `examples/README.md`

Planned documentation includes:

- `docs/security/remote-access.md`
- `docs/security/hardening-roadmap.md`
- `docs/career/project-summary.md`
- Sanitized Home Assistant YAML examples

## Notes for Reviewers

This project is a living homelab. Some areas are complete and actively used, while others are planned improvements.

The value of this project is not simply that it runs a list of tools. The value is in how the pieces fit together: virtualization, Linux administration, Docker operations, monitoring, private remote access, home automation, API integration, backup planning, user feedback, and practical troubleshooting.

This is the infrastructure foundation I plan to keep building on.
