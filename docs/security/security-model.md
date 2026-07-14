# Security Model

The security model is based on limited exposure, workload separation, private administration, and recoverable change management.

## Current Controls

- Proxmox, Debian Docker, and Home Assistant are separated by workload.
- Tailscale provides private remote administration.
- Root SSH login is disabled on Debian.
- SSH access is restricted to the intended administrative user.
- Administrative services remain on LAN/Tailscale.
- Plex is the only service with deliberate public port forwarding.
- Public documentation is sanitized.

Installed software is not counted as a control unless it is actually configured and validated. Uptime Kuma is availability monitoring, not security monitoring; Nginx Proxy Manager does not protect services that are not routed through it.

## Exposure Policy

Management interfaces such as Proxmox, Portainer, Pi-hole, Nginx Proxy Manager, Home Assistant administration, Uptime Kuma, and the Arr applications remain private.

Public exposure is approved by service need, not convenience. Alternate port numbers are not treated as security controls.

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

Security changes are operational changes. They require a recovery path and validation, not just an installed package or edited configuration line.