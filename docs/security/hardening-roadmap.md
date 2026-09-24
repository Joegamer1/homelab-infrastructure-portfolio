# Hardening Roadmap

These are the security changes I have made and the work still ahead.

## Completed

### Private Remote Access

Tailscale is used for private remote access to the homelab environment. This reduces the need to expose administrative services directly to the public internet.

### SSH Hardening

SSH hardening has been completed on the Debian Docker VM.

Current completed items include:

- Root SSH login disabled
- SSH access limited to the expected user account
- Administrative access kept private through LAN or Tailscale paths where possible

### Public Exposure Review

The current approach is to keep administrative services private and avoid direct public exposure for management interfaces.

Plex remote access has been used publicly. Other administrative interfaces should remain private unless explicitly reviewed.

## Planned

### UFW Firewall Baseline

I haven't implemented and tested the host firewall rules yet.

Planned baseline:

- Allow expected LAN access
- Allow expected Tailscale access
- Allow required service ports only where needed
- Avoid accidentally blocking current remote management access
- Validate access from both LAN and Tailscale after applying rules

### Restore Testing

Backups exist, but restore testing needs to be documented and validated.

Planned restore testing should answer:

- Can the backup archive be extracted cleanly?
- Are service configuration files present?
- Can a service be restored from backup onto a test path or replacement host?
- Are secrets excluded from public documentation?

### Router and Port Forward Review

Planned review items:

- Identify active router port forwards
- Confirm which services are intentionally public
- Remove unnecessary forwards
- Keep administrative tools private

### Monitoring Improvements

Monitoring exists through Uptime Kuma, but alerting and escalation can improve over time.

Planned improvements:

- Review monitor coverage
- Add checks for key household services
- Decide which alerts matter enough to notify on
- Avoid noisy alerts that train me to ignore the dashboard

### Documentation Hygiene

Security documentation should be reviewed when the environment changes.

Planned documentation practices:

- Keep public docs sanitized
- Use placeholders instead of secrets or private values
- Separate completed work from planned work
- Avoid posting sensitive screenshots
- Document why security decisions were made, not just what commands were run

I update the completed section after testing a change. The remaining items stay on this list until then.
