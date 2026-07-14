# Service Map

| Service | Role | Hosted On | Access | Status |
|---|---|---|---|---|
| Proxmox VE | Virtualization | Physical host | Private | Active |
| Debian Docker VM | Container host | Proxmox | Private | Active |
| Home Assistant OS | Household operations | Proxmox | Private | Active |
| Portainer | Docker management | Debian VM | Private | Active |
| Homepage | Service launchpad | Debian VM | Private | Active |
| Uptime Kuma | Availability monitoring | Debian VM | Private | Active |
| Pi-hole | DNS and filtering | Debian VM | Internal | Active |
| Nginx Proxy Manager | Reverse-proxy management | Debian VM | Private/internal | Active |
| Tailscale | Remote administration | Host and VMs | Private mesh | Active |
| Plex | Media server | Debian VM | Limited public exposure | Active |
| Seerr | Media requests | Debian VM | Private/internal | Active |
| Radarr | Movie automation | Debian VM | Private/internal | Active |
| Sonarr | TV automation | Debian VM | Private/internal | Active |
| Prowlarr | Indexer coordination | Debian VM | Private/internal | Active |
| SABnzbd | Download processing | Debian VM | Private/internal | Active |
| Donetick | Chore backend | Debian VM | Private/internal | Active |
| Google Calendar integration | Calendar source | Home Assistant | Cloud integration | Active |
| Hermes | AI coordination | Debian VM | Private | Planned |

## Dependencies

The Debian Docker VM is the shared dependency for most self-hosted applications. If it is unavailable, infrastructure dashboards, DNS, monitoring, media services, and Donetick are affected.

Home Assistant is isolated from that stack. An outage there affects household dashboards and automations but does not stop the Docker services themselves.

Proxmox remains the top-level dependency for both VMs.

## Role Separation

- **Homepage** provides navigation and selected status information.
- **Uptime Kuma** performs availability checks.
- **Home Assistant** owns household state and automation.
- **Donetick** owns chore recurrence and assignment.
- **Hermes** will interpret household context without replacing Home Assistant as the source of truth.

Implementation and troubleshooting details are documented in the relevant project folders.