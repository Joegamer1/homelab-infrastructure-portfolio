# Documentation Index

This directory contains the main documentation for my homelab infrastructure portfolio.

The repository is organized so the core infrastructure story stays easy to follow while individual projects have room to document the actual build process. The goal is not to present a frictionless lab template. It is to show a real environment that was built through testing, correction, and continued improvement.

## Architecture

- [`architecture/overview.md`](architecture/overview.md) — high-level architecture and design decisions
- [`architecture/service-map.md`](architecture/service-map.md) — service roles, hosting locations, and dependencies

## Operations

- [`operations/historical-build-and-troubleshooting.md`](operations/historical-build-and-troubleshooting.md) — the broader build history, failed assumptions, troubleshooting steps, and lessons across Proxmox, Docker, networking, media, backups, and Home Assistant
- [`operations/recent-project-troubleshooting.md`](operations/recent-project-troubleshooting.md) — detailed recent Home Assistant calendar, chore, and family-dashboard troubleshooting
- [`operations/backup-plan.md`](operations/backup-plan.md) — current backup approach, how the scope was discovered, and planned restore validation
- [`operations/maintenance-notes.md`](operations/maintenance-notes.md) — maintenance priorities and review habits
- [`operations/restore-notes.md`](operations/restore-notes.md) — restore planning and service recovery notes

## Security

- [`security/security-model.md`](security/security-model.md) — current controls, security decision history, completed work, and planned hardening

## Projects

- [`projects/README.md`](projects/README.md) — project index
- [`projects/home-assistant-family-dashboard/`](projects/home-assistant-family-dashboard/) — household-facing Home Assistant landing page
- [`projects/home-assistant-work-week/`](projects/home-assistant-work-week/) — Work Week dashboard and calendar display architecture
- [`projects/home-assistant-chores/`](projects/home-assistant-chores/) — Donetick-backed chores workflow
- [`projects/house-brain-hermes/`](projects/house-brain-hermes/) — planned local assistant layer
- [`projects/homepage-dashboard/`](projects/homepage-dashboard/) — Homepage launchpad organization and troubleshooting

## Documentation Standard

Each document should remain grounded in what is actually running or intentionally planned.

A useful project writeup should answer:

1. What problem was being solved?
2. What was tried first?
3. What failed or behaved unexpectedly?
4. How was the failing layer isolated?
5. What change solved the problem?
6. Why was that solution kept?
7. What remains incomplete?

Completed work should be described as completed only after implementation and testing. Planned work should stay clearly marked as planned.

The strength of this portfolio is not that the homelab was perfect on the first attempt. The strength is that I can build, operate, troubleshoot, revise, and document real infrastructure over time.
