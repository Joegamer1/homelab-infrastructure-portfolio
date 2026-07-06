# Security Model

This document describes the current security model for the homelab infrastructure environment.

The goal is to document how the environment reduces exposure, separates responsibilities, and tracks future hardening work. This is not intended to represent the environment as enterprise-grade or fully hardened. It is a practical home infrastructure environment with security improvements applied incrementally.

## Security Goals

The current security goals are:

- Keep administrative services private where possible
- Avoid unnecessary public exposure
- Use private remote access for management
- Separate major workloads by role
- Document completed hardening honestly
- Track planned hardening separately from completed work
- Avoid publishing secrets or production configuration in public documentation

## Current Security Posture

The environment currently uses a private-by-default approach.

Administrative access is intended to occur through trusted paths such as local network access or Tailscale rather than direct public exposure.

Current security-related controls include:

- Proxmox workload separation
- Dedicated Debian Docker VM for service hosting
- Dedicated Home Assistant OS VM for home automation
- Tailscale private remote access
- SSH hardening
- Limited public exposure
- Sanitized public documentation
- Separation of completed work from planned work

## Workload Separation

The environment separates major service roles across virtual machines.

### Proxmox VE

Proxmox provides the virtualization layer and hosts the major workloads.

Its role is to provide:

- VM hosting
- Resource allocation
- Workload separation
- Infrastructure management

Management access to Proxmox should remain private.

### Debian Docker VM

The Debian Docker VM hosts most containerized services.

This keeps application services separate from the Proxmox host and provides a dedicated Linux environment for Docker workloads.

Services hosted on this VM include:

- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Plex/Arr stack
- Donetick

### Home Assistant OS VM

Home Assistant OS runs separately from the main Docker service stack.

This separation reduces coupling between household automation workflows and general infrastructure services.

## Remote Access

Remote access is handled through Tailscale.

The purpose of Tailscale in this environment is to provide private access to services without exposing management interfaces directly to the public internet.

Remote access principles:

- Prefer Tailscale for administrative access
- Avoid public exposure of management dashboards
- Keep Proxmox, Portainer, Home Assistant, and monitoring interfaces private
- Use public exposure only where specifically required and understood

## Public Exposure

The environment is designed to minimize public exposure.

Most services should remain reachable only through:

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

The goal of SSH hardening is to reduce unnecessary access risk and avoid weak administrative patterns.

Completed SSH hardening work includes:

- Root SSH login disabled
- SSH access restricted to the intended administrative user
- Password and remote access behavior reviewed
- Administrative access kept private where practical

Future SSH improvements may include:

- Key-only SSH authentication
- Additional logging review
- Fail2ban or similar brute-force protection if needed
- More formal access documentation

## Firewall Status

UFW/firewall hardening is planned work.

It should not be represented as completed until implemented and tested.

The planned firewall model should define allowed access by source and service role.

Expected future firewall goals include:

- Allow trusted LAN access where required
- Allow Tailscale access for management
- Limit unnecessary inbound traffic
- Keep Docker service exposure intentional
- Document allowed ports and service purpose
- Test before and after firewall changes

Firewall hardening will be documented separately in:

```text
docs/security/hardening-roadmap.md

or:

docs/security/ufw-plan.md

depending on final repo structure.

Secrets Handling

This public repository does not store production secrets.

The following should not be committed:

API tokens
Passwords
Private keys
.env files
Home Assistant secrets.yaml
Donetick API tokens
Plex claim tokens
Tailscale auth keys
Cloudflare tokens
Personal calendar data
Raw production configuration files

Example files should use placeholders such as:

CHANGEME
LOCAL_IP
SERVICE_URL
API_TOKEN_HERE
HOME_ASSISTANT_URL
DONETICK_HOST
Backup Security

A backup script exists for selected Docker and infrastructure configuration paths.

Backup security considerations include:

Backups may contain sensitive configuration data
Backups should not be committed to this repository
Backup files should be stored outside the public repo
Restore testing is still planned
Backup access should be limited to trusted users and systems

Until restore testing is completed, backups should be considered implemented but not fully validated.

Monitoring and Visibility

Uptime Kuma provides service health monitoring.

Homepage provides centralized service visibility and navigation.

These tools help identify service outages and improve operational awareness, but they are not a replacement for security monitoring or SIEM-style logging.

Future cybersecurity monitoring work will be handled in a separate dedicated security lab project.

Documentation Security

This repository is intentionally documentation-focused.

Public documentation should describe:

Architecture
Service purpose
Design decisions
Operational practices
Sanitized examples
Planned improvements

Public documentation should not expose:

Secrets
Private operational data
Sensitive screenshots
Raw production configs
Personal household details
Current Completed Security Work

Completed security-related work includes:

Tailscale private remote access
SSH hardening on the Debian Docker VM
Avoidance of public exposure for administrative services
Public documentation sanitized for portfolio use
Clear separation between completed work and planned improvements
Planned Security Improvements

Planned improvements include:

UFW/firewall hardening
Restore testing
More formal network/service access documentation
More complete hardening checklist
Improved monitoring and alerting
Dedicated cybersecurity lab project using this infrastructure as the foundation
Security Design Summary

The current security model is based on practical risk reduction:

Keep management private
Separate workloads by role
Avoid unnecessary public exposure
Use Tailscale for remote access
Harden SSH access
Document what is completed versus planned
Keep public documentation sanitized

This approach provides a solid operational foundation while leaving room for future hardening and dedicated cybersecurity lab work.
