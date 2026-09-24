# Project summary for interviews and resumes

I built and run a Proxmox homelab on a Dell Precision T7820. It hosts Docker services, Home Assistant, media, DNS, monitoring, remote access, and household calendars and chores.

## Decisions I can explain

- I put Docker services in a Debian VM so application changes stay separate from the Proxmox host.
- I gave Home Assistant its own VM because the household uses it, and changes to the media stack shouldn't take it down.
- I use Tailscale for remote administration to keep management interfaces off the public internet.
- I inspected running container mounts before writing the backup script. Data was spread across bind mounts, named volumes, `/srv/docker`, and older `/opt/docker` paths.
- I keep backup creation and restore testing separate in these notes. The script creates archives; a documented restore test is still needed.

## Resume bullets

- Built and maintained a Proxmox homelab with separate Debian Docker and Home Assistant VMs.
- Deployed Portainer, Homepage, Uptime Kuma, Pi-hole, Nginx Proxy Manager, Plex, Donetick, and Tailscale.
- Configured NVIDIA GPU passthrough through Proxmox, Debian, and Docker for Plex hardware transcoding.
- Connected Home Assistant to Google Calendar and Donetick for schedules and recurring chores.
- Documented service dependencies, configuration backups, troubleshooting, and remaining security work.

## Problems worth discussing

- Tracing GPU visibility from the Proxmox host through the VM and into Plex
- Finding the configuration directory a running container actually uses
- Keeping accurate overnight shift times while showing a single entry on the family calendar
- Checking Donetick's stored API values when a YAML change didn't affect existing chores
- Testing remote playback separately from port availability
- Deferring firewall changes until I have a console recovery path

The [troubleshooting history](../operations/historical-build-and-troubleshooting.md) and [recent fixes](../operations/recent-project-troubleshooting.md) have the details.

## Still to do

Restore testing, host firewall rules, improved monitoring and alerting, service recovery notes, and the read-only Hermes integration are unfinished. A separate cybersecurity lab for detection and incident work is also planned.
