# Documentation Index

This directory contains the main documentation for my homelab infrastructure portfolio.

The repository is organized so the core infrastructure story stays easy to follow while individual projects have room to document the actual build process. The goal is not to present a frictionless lab template. It is to show a real environment built through testing, correction, user feedback, and continued improvement.

## Architecture

- [`architecture/overview.md`](architecture/overview.md) — high-level architecture, responsibility boundaries, and the planned Hermes layer
- [`architecture/service-map.md`](architecture/service-map.md) — service roles, hosting locations, and dependencies

## Operations

- [`operations/project-evolution.md`](operations/project-evolution.md) — how the homelab progressed into a household infrastructure and operations platform
- [`operations/historical-build-and-troubleshooting.md`](operations/historical-build-and-troubleshooting.md) — the broader build history, failed assumptions, troubleshooting steps, and lessons across Proxmox, Docker, networking, media, backups, and Home Assistant
- [`operations/recent-project-troubleshooting.md`](operations/recent-project-troubleshooting.md) — recent Home Assistant calendar, Browser Mod, mobile layout, chore recurrence, and family-dashboard troubleshooting
- [`operations/backup-plan.md`](operations/backup-plan.md) — current backup approach, how the scope was discovered, and planned restore validation
- [`operations/maintenance-notes.md`](operations/maintenance-notes.md) — maintenance priorities and review habits
- [`operations/restore-notes.md`](operations/restore-notes.md) — restore planning and service recovery notes

## Security

- [`security/security-model.md`](security/security-model.md) — current controls, security decision history, completed work, and planned hardening

## Projects

- [`projects/README.md`](projects/README.md) — project index and current planned work
- [`projects/plex-hardware-transcoding/`](projects/plex-hardware-transcoding/) — Proxmox PCI passthrough, NVIDIA container access, and Plex playback validation
- [`projects/home-assistant-family-dashboard/`](projects/home-assistant-family-dashboard/) — household-facing Home Assistant landing page, mobile layout, and planned Hermes hero summary
- [`projects/home-assistant-work-week/`](projects/home-assistant-work-week/) — Work Week dashboard, Browser Mod shift entry, and calendar display architecture
- [`projects/home-assistant-chores/`](projects/home-assistant-chores/) — Donetick-backed rolling chore workflow
- [`projects/homepage-dashboard/`](projects/homepage-dashboard/) — Homepage launchpad organization and troubleshooting
- [`projects/house-brain-hermes/`](projects/house-brain-hermes/) — planned Docker-based AI coordination layer with a read-only first phase

## Current Documentation Priorities

- Preserve the difference between completed work and planned work.
- Document regressions and reverted experiments, not only successful end states.
- Keep internal addresses, tokens, and household details out of public examples.
- Add sanitized screenshots only when they explain a validation result or operational decision.
- Expand configuration examples after they can be safely generalized.

## Documentation Standard

A useful project writeup should answer:

1. What problem was being solved?
2. What was tried first?
3. What failed or behaved unexpectedly?
4. How was the failing layer isolated?
5. What change solved the problem?
6. How was the result validated?
7. Why was that solution kept?
8. What remains incomplete?
9. What would be done differently during a rebuild?

Completed work should be described as completed only after implementation and testing. Planned work should stay clearly marked as planned.

The strength of this portfolio is not that the homelab was perfect on the first attempt. The strength is that I can build, operate, troubleshoot, revise, validate, and document real infrastructure over time.