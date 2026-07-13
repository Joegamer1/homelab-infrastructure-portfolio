# Projects

This folder contains focused project writeups from my homelab and household operations work.

The main repository explains the overall infrastructure platform. These project folders document the individual systems that make the platform useful at home, including design decisions, operational problems, user feedback, validation methods, and troubleshooting that led to the current implementation.

## Current Projects

- [`plex-hardware-transcoding/`](plex-hardware-transcoding/) — Proxmox PCI passthrough, NVIDIA container access, Plex playback diagnostics, and hardware-transcoding validation.
- [`home-assistant-family-dashboard/`](home-assistant-family-dashboard/) — the main household-facing Home Assistant landing page, responsive mobile design, navigation, shopping and todo access, and planned Hermes hero summary.
- [`home-assistant-work-week/`](home-assistant-work-week/) — shift entry and display using Browser Mod, overnight date handling, detailed and all-day display calendars, midnight defaults, and date-picker troubleshooting.
- [`home-assistant-chores/`](home-assistant-chores/) — a Home Assistant chore workflow backed by self-hosted Donetick, REST-based rolling recurrence, assignment, and completion visibility.
- [`homepage-dashboard/`](homepage-dashboard/) — the Homepage launchpad organization and application troubleshooting project.
- [`house-brain-hermes/`](house-brain-hermes/) — the planned Docker-based AI coordination layer, read-only Home Assistant integration, least-privilege design, and household hero summary.

## Project Narrative

- [`../operations/project-evolution.md`](../operations/project-evolution.md) — how the environment progressed from a traditional homelab into a household infrastructure and operations platform.
- [`../operations/historical-build-and-troubleshooting.md`](../operations/historical-build-and-troubleshooting.md) — broader infrastructure build history and troubleshooting.
- [`../operations/recent-project-troubleshooting.md`](../operations/recent-project-troubleshooting.md) — detailed recent Home Assistant implementation problems, regressions, and resolutions.

## Current Planned Work

- Deploy Hermes under `/srv/docker/hermes`.
- Create a dedicated Home Assistant account and read-only token for Hermes.
- Publish a cozy household hero summary to the Home dashboard.
- Add per-shift deletion to the Work Week workflow.
- Add monthly and quarterly rolling chore workflows.
- Finish Shopping List Card integration and documentation.
- Add sanitized current-state screenshots and configuration examples.
- Complete backup restore validation and Donetick recovery documentation.
- Improve monitoring beyond basic availability checks.

Cybersecurity-specific labs should stay separate unless they document the infrastructure foundation they depend on.