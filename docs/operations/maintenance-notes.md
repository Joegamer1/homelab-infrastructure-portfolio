# Maintenance notes

I prioritize access, DNS, monitoring, and the services the household uses. Most applications depend on the Debian Docker VM, so a change there can affect several services at once.

## Regular checks

- Proxmox, the Debian VM, and Home Assistant are reachable.
- Expected containers are running, with no unexpected restarts or unhealthy states.
- Pi-hole resolves DNS and Tailscale connects.
- Homepage loads and its links point to the right services.
- Uptime Kuma reports the expected status.
- Disk space, published ports, and recently updated services look as expected.

## Dashboards and household services

Homepage groups links into Core Infrastructure, Home, Docker Services, and Media Stack. It also includes Home Assistant, Donetick, Donetick Docker metrics, and Tailscale Machines. I update those links as services change.

For Home Assistant, I check calendars, family dashboard navigation, chore creation, completed-today summaries, and notifications. These are used around the house, so a broken integration is noticeable outside the lab.

Uptime Kuma is my first stop for service availability. I still need to review monitor coverage, remove stale checks, and tune alerts.

## Backups

The backup script creates archives, but I haven't completed a documented restore test. I check that the archives exist, contain the current configuration paths, and fit within the available storage. [Restore notes](restore-notes.md) has the test plan.

## Before a larger change

I check what depends on the component and how I would recover if I lose access. Networking, Docker Compose, Pi-hole, proxy hosts, Tailscale, Home Assistant configuration, backup paths, and firewall rules need particular care.

I update these notes when services are added, removed, or moved; when backup scope or security settings change; and after troubleshooting that would be useful to have on hand again.

## Remaining work

- Complete a restore test and service recovery instructions.
- Add and test host firewall rules.
- Improve monitoring coverage and alerting.
- Add architecture diagrams and reviewed screenshots.
