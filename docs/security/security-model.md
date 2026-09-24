# Security Model

I keep management interfaces private, separate the main workloads into VMs, and plan a recovery path before changing access settings.

## Current Controls

- Proxmox, Debian Docker, and Home Assistant are separated by workload.
- Tailscale provides private remote administration.
- Root SSH login is disabled on Debian.
- SSH access is restricted to the intended administrative user.
- Administrative services remain on LAN/Tailscale.
- Plex is the only service with deliberate public port forwarding.
- Public documentation is sanitized.

Uptime Kuma checks availability. Nginx Proxy Manager only handles traffic routed through it; installing it does not protect services reached by direct ports.

## Exposure Policy

Management interfaces such as Proxmox, Portainer, Pi-hole, Nginx Proxy Manager, Home Assistant administration, Uptime Kuma, and the Arr applications remain private.

I review public access per service. Changing a port number does not restrict access.

## SSH and Firewall Status

SSH hardening is active. Key-only authentication and additional logging remain possible future improvements.

UFW or equivalent host-firewall hardening is planned but not yet implemented. It was deferred until console recovery is available because Docker port publishing, Tailscale, and SSH must be tested together. The firewall should not be marked complete until trusted-LAN access, Tailscale management, container ports, and rollback procedures are validated.

## Hermes Trust Boundary

Hermes will use a dedicated Home Assistant identity and token. The first phase is read-only, secrets remain outside Git, and only curated context is exposed. Write actions will be added individually rather than granting unrestricted service access.

## Backups and Monitoring

Backups contain sensitive configuration and remain private. Restore testing is still incomplete.

Uptime Kuma provides reachability checks. Security telemetry, detection engineering, and attack simulation will belong in a separate cybersecurity lab project.

## Planned Improvements

- Implement and validate host firewall rules.
- Review key-only SSH authentication.
- Complete restore testing.
- Improve allowed-port and network documentation.
- Add stronger alerting where operationally useful.

After an access change, I need to check LAN, Tailscale, SSH, and the affected services before considering it finished.
