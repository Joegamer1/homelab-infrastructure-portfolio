# Remote Access Model

This document explains the current remote access approach for the homelab infrastructure portfolio.

## Current Approach

The environment uses Tailscale for private remote access to administrative services and internal dashboards.

The goal is to avoid exposing management interfaces directly to the public internet whenever possible. Services such as Proxmox, Portainer, Home Assistant, Uptime Kuma, Homepage, and Pi-hole are intended to remain reachable through local network access or private Tailscale access.

## Why Tailscale

Tailscale provides a private mesh network between trusted devices. This gives me a cleaner remote access model than opening multiple administrative ports on the router.

Benefits include:

- Reduced public internet exposure
- Device-based access control
- Easier access while away from home
- Better separation between public services and administrative services
- Less dependence on brittle port-forwarding rules

## Public Exposure Philosophy

The general rule for this environment is:

> Expose only what needs to be public, and keep administration private.

Administrative services should not be publicly reachable unless there is a specific reason and the risk is understood.

## Current Public Exposure

Plex remote access has been used with router port forwarding.

Other admin services are intended to stay private behind LAN or Tailscale access.

## Administrative Access

Administrative access is handled through:

- Local LAN access when at home
- Tailscale access when remote
- SSH to the Debian Docker VM as a non-root user
- Web interfaces reachable only through private paths when possible

## Completed Security Work

Completed work includes:

- Tailscale installed and used for private access
- SSH hardening completed on the Debian Docker VM
- Root SSH login disabled
- Administrative services kept off direct public exposure where possible

## Planned Improvements

Planned work includes:

- UFW/firewall baseline on the Debian Docker VM
- Clear documentation of allowed LAN and Tailscale access paths
- Review of router port forwards
- Restore and recovery testing
- More formal monitoring alerts for service availability

## Notes

This document should be updated whenever the access model changes. Remote access is one of the most important parts of the homelab security posture because convenience can quietly turn into exposure if it is not reviewed.
