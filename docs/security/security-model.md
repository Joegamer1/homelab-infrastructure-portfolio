# Security Model

This document describes the current security model for my homelab and the reasoning that led to it.

This is not an enterprise network, but the management plane should remain private, the attack surface should stay limited, and completed work should be clearly separated from planned hardening.

## Security Goals

- Keep administrative services private where possible.
- Avoid unnecessary public exposure.
- Use Tailscale for private remote access.
- Separate major workloads by role.
- Harden obvious administrative access paths.
- Preserve a recovery path while making security changes.
- Keep public documentation sanitized.

The goal is practical risk reduction, not cosmetic compliance.

## Current Security Posture

Current controls include:

- Proxmox workload separation
- Dedicated Debian Docker VM
- Dedicated Home Assistant OS VM
- Tailscale private remote access
- Root SSH login disabled
- SSH restricted to the intended administrative user
- Only the required Plex service path publicly forwarded
- Administrative services kept on LAN/Tailscale
- Sanitized public documentation

## How the Model Evolved

### Private access before public exposure

Tailscale was installed early so that Proxmox, the Debian VM, Home Assistant, and internal web interfaces could be reached without exposing each management port through the router.

This established the default pattern: remote administration should use the private overlay network unless a service has a specific user-facing reason to be public.

### Plex required a deliberate exception

Plex remote access required a public router forwarding rule. Rather than treating this as permission to publish the rest of the stack, the exposure was limited to Plex.

Testing also clarified that forwarding an additional Plex port would not improve simultaneous streaming capacity. Port count and server capacity are separate concerns.

### SSH was hardened after remote access was stable

The Debian VM was first made reliably reachable through LAN and Tailscale. Root SSH login was then disabled and access was restricted to the intended administrative account.

This order reduced the risk of hardening the system before a dependable management path existed.

### UFW was intentionally deferred

UFW hardening remains planned, not forgotten.

The work was postponed because changes were being made remotely and an incorrect firewall rule could have blocked SSH, Tailscale, or Docker-published services. The safer implementation point is when local access or the Proxmox console is open and available for recovery.

This is an example of risk-based sequencing: applying a control without a rollback path can create an avoidable outage.

## Problems Encountered and Lessons

### Remote URLs were sometimes valid only in the wrong context

Home Assistant onboarding and authentication flows occasionally returned `localhost` or LAN-based callback addresses. The service itself was healthy, but the generated URL did not make sense from the remote client.

**Resolution:** Used the correct Tailscale address for the target system after authentication.

**Lesson:** Access control troubleshooting must distinguish service health from address context.

### Public exposure could have expanded by accident

Once router configuration was being changed for Plex, it would have been easy to expose management services for convenience.

**Resolution:** Kept Portainer, Proxmox, Pi-hole, Nginx Proxy Manager, Uptime Kuma, Home Assistant administration, and the Arr applications private.

### Docker and host firewall behavior require testing together

Docker publishes ports through its own networking and packet-filtering behavior. A future UFW policy must be tested against actual container access rather than assuming host rules alone describe the effective exposure.

### Installed security tooling is not automatically effective

Nginx Proxy Manager being installed does not mean every service is routed through it. Uptime Kuma being installed does not provide security monitoring. Tailscale being installed does not eliminate the need to review local listening ports.

**Lesson:** Security documentation should describe effective controls, not merely installed products.

## Workload Separation

### Proxmox VE

Proxmox hosts and manages major workloads. Its management interface remains private.

### Debian Docker VM

The Debian VM contains most containerized services. This keeps application changes away from the hypervisor and provides one place to apply Linux and Docker operational controls.

### Home Assistant OS VM

Home Assistant runs separately because it supports household operations. Media-stack or Docker experimentation should not directly destabilize household dashboards and automations.

## Remote Access Principles

- Prefer Tailscale for administration.
- Keep Proxmox private.
- Keep Portainer private.
- Keep Home Assistant administration private.
- Keep monitoring and DNS administration private.
- Review every public forwarding rule by service need.
- Do not treat obscurity or alternate port numbers as security controls.

## SSH Hardening

Completed:

- Root SSH login disabled.
- Access restricted to the intended administrative user.
- Administrative access kept private through LAN/Tailscale.

Possible future improvements:

- Key-only authentication.
- Additional authentication logging review.
- Fail2ban if the threat model warrants it.
- More formal administrative access documentation.

## Firewall Status

UFW/firewall hardening is planned.

The intended baseline should:

- Allow required trusted-LAN access.
- Allow required Tailscale management access.
- Preserve established SSH access.
- Limit unnecessary inbound traffic.
- Account for Docker-published ports.
- Document every allowed service and source.
- Be applied only with console recovery available.
- Be tested before the remote session is closed.

It should not be represented as complete until these tests are performed.

## Backup Security

Backups contain sensitive operational data and remain outside the public repository. Restore testing is still pending, so the backups are useful but not yet fully validated.

## Monitoring Boundaries

Uptime Kuma provides availability monitoring. Homepage provides navigation and selected status visibility. Neither is a SIEM, EDR, IDS, or complete observability platform.

Security-specific telemetry, detection engineering, and attack simulation will be documented in a separate future cybersecurity lab project.

## Documentation Security

Public documentation may include:

- Architecture
- Service roles
- Design decisions
- Sanitized examples
- Troubleshooting methods
- Honest status and limitations

It should not include:

- Passwords or API tokens
- Private keys
- Raw production configurations
- Personal calendar data
- Private household details
- Sensitive screenshots
- Unnecessary public IP or tailnet details

## Current Completed Security Work

- Private remote administration through Tailscale
- SSH hardening on the Debian Docker VM
- Limited public exposure for Plex only
- Administrative services kept private
- Workload separation by role
- Sanitized portfolio documentation

## Planned Security Improvements

- UFW/firewall implementation and validation
- Key-only SSH authentication review
- Restore testing
- Better network and allowed-port documentation
- Improved alerting
- Dedicated cybersecurity lab built on this infrastructure

## Security Design Summary

The current model is based on controlled exposure, workload separation, and recoverable change management.

The important lesson from the build process is that security changes are operational changes. They should be tested with the same care as application changes, and they should not be marked complete simply because a package was installed or a configuration line was added.
