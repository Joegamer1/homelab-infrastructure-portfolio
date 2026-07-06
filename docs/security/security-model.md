# Security Model

This document describes the current security model for my homelab.

This is not an enterprise network, but I still want the management plane to stay private, the attack surface to stay limited, and the documentation to be honest about what is complete versus what is planned.

## Security Goals

My current security goals are:

- Keep administrative services private where possible
- Avoid unnecessary public exposure
- Use Tailscale for private remote access
- Separate major workloads by role
- Harden obvious administrative access paths
- Document completed security work honestly
- Track planned improvements separately from completed work
- Keep public documentation sanitized

The goal is practical risk reduction, not pretending the homelab is a fully mature enterprise environment.

## Current Security Posture

The environment currently follows a private-by-default approach.

Most administrative access is intended to happen through either the local network or Tailscale. I do not want management dashboards exposed directly to the public internet unless there is a specific reason and I understand the risk.

Current security-related controls include:

- Proxmox workload separation
- Dedicated Debian Docker VM for service hosting
- Dedicated Home Assistant OS VM for home automation
- Tailscale private remote access
- SSH hardening
- Limited public exposure
- Sanitized public documentation
- Clear separation between completed and planned work

## Workload Separation

The environment separates major service roles across virtual machines.

### Proxmox VE

Proxmox provides the virtualization layer.

Its role is to host and manage the major workloads. Management access to Proxmox should remain private.

### Debian Docker VM

The Debian Docker VM hosts most containerized services.

This keeps application services separate from the Proxmox host and gives Docker workloads their own dedicated Linux environment.

Services hosted on this VM include:

- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Plex/Arr stack
- Donetick

### Home Assistant OS VM

Home Assistant OS runs separately from the Docker service stack.

This separation matters because Home Assistant is becoming the household operations platform. I want it isolated enough that changes to the media or infrastructure service stack do not directly tangle with home automation.

## Remote Access

Remote access is handled through Tailscale.

This is one of the most important security decisions in the environment. Tailscale lets me access internal services remotely without exposing administrative dashboards directly to the public internet.

Remote access principles:

- Prefer Tailscale for administrative access
- Keep Proxmox private
- Keep Portainer private
- Keep Home Assistant administrative access private
- Keep monitoring and DNS admin interfaces private
- Review any public exposure carefully

## Public Exposure

The environment is designed to minimize public exposure.

Most services should be reachable only through:

- Local network access
- Tailscale private access
- Controlled internal DNS or reverse proxy paths

Services that should not be directly exposed to the public internet include:

- Proxmox VE
- Portainer
- Home Assistant administrative interfaces
- Uptime Kuma administrative interface
- Pi-hole administrative interface
- Nginx Proxy Manager administrative interface
- Radarr
- Sonarr
- Prowlarr
- SABnzbd
- Donetick administrative access

Any service exposure should be reviewed based on need, authentication, update status, and risk.

## SSH Hardening

SSH hardening has been completed on the Debian Docker VM.

The goal was to reduce unnecessary administrative access risk and avoid weak default patterns.

Completed SSH hardening work includes:

- Root SSH login disabled
- SSH access restricted to the intended administrative user
- Administrative access kept private where practical

Future SSH improvements may include:

- Key-only SSH authentication
- Additional logging review
- Fail2ban or similar brute-force protection if needed
- More formal access documentation

## Firewall Status

UFW/firewall hardening is planned work.

This should not be represented as completed until it is implemented and tested.

The future firewall model should define allowed access by source and service role.

Expected firewall goals include:

- Allow trusted LAN access where required
- Allow Tailscale access for management
- Limit unnecessary inbound traffic
- Keep Docker service exposure intentional
- Document allowed ports and service purpose
- Test access before and after firewall changes

## Backup Security

A backup script exists for selected Docker and infrastructure configuration paths.

Backups may contain sensitive operational data, so they are treated as private artifacts and are not stored in this public repository.

Restore testing is still planned. Until restore testing is completed, backups should be considered implemented but not fully validated.

## Monitoring and Visibility

Uptime Kuma provides service health monitoring.

Homepage provides centralized service visibility and navigation.

These tools help me understand whether services are reachable, but they are not a replacement for security monitoring or SIEM-style logging.

That kind of cybersecurity monitoring will be handled in a separate future security lab project.

## Documentation Security

This repository is intentionally documentation-focused.

Public documentation should describe:

- Architecture
- Service purpose
- Design decisions
- Operational practices
- Sanitized examples
- Planned improvements

It should not expose secrets, private operational data, sensitive screenshots, raw production configs, or personal household details.

## Current Completed Security Work

Completed security-related work includes:

- Tailscale private remote access
- SSH hardening on the Debian Docker VM
- Avoidance of public exposure for administrative services
- Public documentation sanitized for portfolio use
- Clear separation between completed work and planned improvements

## Planned Security Improvements

Planned improvements include:

- UFW/firewall hardening
- Restore testing
- More formal network and service access documentation
- More complete hardening notes
- Improved monitoring and alerting
- Dedicated cybersecurity lab project using this infrastructure as the foundation

## Security Design Summary

The current security model is based on practical risk reduction.

My current approach is:

- Keep management private
- Separate workloads by role
- Avoid unnecessary public exposure
- Use Tailscale for remote access
- Harden SSH access
- Document what is complete versus planned
- Keep public documentation sanitized

This gives the homelab a solid operational foundation while leaving room for future hardening and dedicated cybersecurity lab work.
