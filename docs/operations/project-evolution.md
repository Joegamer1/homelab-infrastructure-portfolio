# Project Evolution

The environment grew from a traditional homelab into a household infrastructure platform. This document records the major architectural changes; implementation details remain in the project folders.

## 1. Virtualization Foundation

Proxmox VE was established on the Dell Precision T7820. The hypervisor was kept focused on virtualization, with general applications placed in dedicated workloads. A Debian VM became the main Docker host.

## 2. Operated Docker Platform

Portainer, Pi-hole, Nginx Proxy Manager, Uptime Kuma, and Homepage began as individual services. As the stack grew, the operational work shifted toward mount discovery, naming consistency, backups, monitoring, and dependency management.

Key lesson: inspect the running environment rather than trusting assumed paths or tutorial defaults.

## 3. Media and Hardware Acceleration

The media stack added Plex, Seerr, Radarr, Sonarr, Prowlarr, and SABnzbd. GPU passthrough for Plex required validation across Proxmox, the Debian guest, Docker, and Plex itself.

Playback troubleshooting also showed that client support, audio formats, network paths, and upload bandwidth can matter as much as video transcoding.

## 4. Private Remote Administration

Tailscale became the standard administrative access path. Management interfaces remained private, while Plex received the limited public exposure required for remote playback.

## 5. Home Assistant as a Separate Platform

Home Assistant expanded from an automation tool into the household operations layer for calendars, work schedules, chores, lists, and shared dashboards. It was kept in a dedicated VM so changes to the Docker or media stack would not disrupt family-facing workflows.

## 6. Design Through Household Feedback

Real use changed several requirements:

- Overnight shifts needed exact source data but only one family-facing display entry.
- Chores needed recurring creation, assignment, helper reset, and completion feedback—not visibility alone.
- Dashboard controls needed to reflect household tasks rather than Home Assistant internals.
- Mobile changes had to work in both portrait and landscape.

These changes reinforced a central rule: technically correct behavior is not always operationally useful behavior.

## 7. Hermes Planning

Hermes was defined as a separate reasoning and coordination layer. Home Assistant remains authoritative for household data and service execution. The first Hermes phase will be read-only through a dedicated account, curated context, and secrets stored outside Git.

## Operating Lessons

- Standardize service paths and container names early.
- Inventory mounts before writing backup logic.
- Validate changes at the layer closest to the behavior.
- Preserve accurate source data separately from simplified presentation when needed.
- Treat systems used by other people as production-like services.
- Record incomplete work honestly, especially restore testing and security hardening.

The current priorities are restore validation, improved firewall and monitoring controls, expanded household workflows, and the initial Hermes integration.