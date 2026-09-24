# Restore notes

The backup script can generate archives of selected configuration paths. I haven't completed a documented restore test yet, so this is the plan I need to test.

## Recovery order

I would recover access, DNS, and management services before the media stack:

1. Confirm that Proxmox is available.
2. Start or rebuild the Debian Docker VM.
3. Install Docker and Compose.
4. Restore the service directories and configuration.
5. Restore DNS, reverse proxy, and management services.
6. Restore monitoring and dashboards.
7. Restore household support services and check Home Assistant integrations.
8. Restore media applications.

The first targets are the Debian service layout, Docker, Homepage, Pi-hole, Nginx Proxy Manager, Portainer, Uptime Kuma, and Donetick.

## First test

Use a separate test location, extract an archive, and check its contents before starting anything. Restore one service at a time. Record missing files, permissions or ownership fixes, manual steps, and how long recovery takes.

Homepage is a useful first target because I can check its links and sections without changing a service the rest of the network depends on.

| Service | What to check after restoring |
| --- | --- |
| Homepage | Links, sections, and configured Docker widgets load; public notes contain no private values. |
| Pi-hole | Service and admin page start, DNS resolves, local records and blocking settings are present. |
| Nginx Proxy Manager | Admin page, proxy hosts, certificates or certificate references, and routes work. |
| Portainer | The Docker environment, volumes, containers, and networks are visible; authentication works or can be reset safely. |
| Uptime Kuma | Monitors and any status pages are present and report correctly; review notifications before enabling them. |
| Donetick | Chore data covered by the backup is present, Home Assistant connects, and completion notifications work. |

## Gaps to resolve

- Service-specific restore commands and application export/database requirements
- Archive inspection commands and a record of the first restore test
- Off-host storage and whether to encrypt the backups
- VM recovery steps and Proxmox backup coverage, if added
- Which services or files the current archives miss

After the first test, I'll replace the proposed steps with the recovery steps that worked and record anything I had to fix manually.
