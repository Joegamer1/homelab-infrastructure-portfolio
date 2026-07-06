# Architecture Overview

This document explains how my homelab is structured and why I organized it this way.

The environment is built to be useful, understandable, and expandable. It supports real services I use at home while also giving me a strong platform for learning infrastructure, operations, automation, and eventually more advanced security lab work.

## Architecture Goals

The main goals for this architecture are:

- Keep the Proxmox host focused on virtualization
- Use a dedicated Debian VM for Docker-based services
- Keep Home Assistant separate from the general Docker stack
- Use private remote access instead of exposing management interfaces publicly
- Make service health visible through monitoring
- Keep the environment documented enough that I can troubleshoot and improve it over time
- Build a foundation that can later support a dedicated cybersecurity lab

This is not meant to be an enterprise design copied into a home environment. It is my practical infrastructure design for a real homelab.

## Physical Host

The environment runs on a Dell Precision T7820 workstation.

The T7820 gives me enough room to run multiple workloads while keeping everything centralized on one physical host. It is the base of the homelab and runs Proxmox VE as the virtualization platform.

## Virtualization Layer

Proxmox VE is the virtualization layer.

I use Proxmox because it gives me clean separation between workloads and makes the environment easier to expand. Instead of installing every service directly on one operating system, I can separate major roles into virtual machines.

Current major VMs include:

- Debian Docker VM
- Home Assistant OS VM

This separation makes the environment easier to understand and maintain.

## Debian Docker VM

The Debian Docker VM is the main service host.

Most of the self-hosted applications run here in Docker. This includes:

- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd
- Donetick

I chose this model because it keeps Docker workloads off the Proxmox host while still giving me one central place to manage most services.

The Debian VM is one of the most important systems in the environment. If it is down, many of the hosted services are down with it.

## Home Assistant OS VM

Home Assistant OS runs as a dedicated VM.

I chose to keep Home Assistant separate because it is becoming the household operations layer of the environment. It is not just another container to me. It manages dashboards, calendar visibility, chore workflows, and future automation ideas.

Current Home Assistant use cases include:

- Family dashboard views
- Google Calendar integration
- Donetick chore integration
- Active chore visibility
- Completed-today summaries
- Daily and weekly chore creation workflows
- Automation-backed notifications

Keeping Home Assistant in its own VM makes it easier to treat as a dedicated platform.

## Logical Service Layers

The environment can be understood in several layers.

### Physical and Virtualization Layer

- Dell Precision T7820
- Proxmox VE
- Debian Docker VM
- Home Assistant OS VM

This layer provides the foundation for the rest of the environment.

### Service Hosting Layer

- Debian Linux
- Docker
- Portainer

This layer runs most of the containerized services.

### Visibility Layer

- Homepage
- Uptime Kuma

This layer helps me quickly see what is running, what is reachable, and where to access services.

### Network and Access Layer

- Pi-hole
- Nginx Proxy Manager
- Tailscale

This layer supports internal DNS, reverse proxy management, and private remote access.

### Household Operations Layer

- Home Assistant
- Donetick
- Google Calendar integration

This layer is where the homelab becomes useful for day-to-day household coordination.

### Media Services Layer

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

This layer supports media hosting and media management workflows.

## Remote Access Model

Remote access is handled through Tailscale.

I use Tailscale because I want remote access to my environment without exposing administrative dashboards directly to the public internet.

The goal is simple: if I need to manage the homelab remotely, I want to come in through a private access path instead of opening unnecessary public ports.

## Monitoring Model

Uptime Kuma provides service monitoring.

Homepage provides a central dashboard for navigating services and viewing selected service information.

Together, these tools give me basic operational awareness. I can quickly see whether important services are reachable without manually checking every container or VM.

This is not a full observability stack yet, but it is a practical starting point.

## DNS and Reverse Proxy Model

Pi-hole provides DNS and ad-blocking capabilities.

Nginx Proxy Manager provides a reverse proxy management layer.

In my environment, these services help make self-hosted applications easier to access and organize. They also give me more practice managing internal service access patterns.

## Backup Approach

A backup script exists for selected Docker and infrastructure configuration paths.

The current backup work is focused on preserving important configuration data rather than backing up every large file or media directory.

Restore testing is still planned. Until I complete restore testing, I consider the backup process implemented but not fully proven.

## Security Approach

The environment follows a private-by-default mindset.

Current security-related work includes:

- SSH hardening
- Tailscale private remote access
- Avoiding public exposure for administrative services
- Sanitized public documentation
- Clear separation between completed and planned work

Planned security improvements include:

- UFW/firewall hardening
- Restore testing
- More complete hardening documentation
- Better monitoring and alerting
- A separate future cybersecurity lab project

## Why This Design Works for Me

This architecture works because it keeps the environment understandable.

Proxmox handles virtualization. Debian handles Docker services. Home Assistant handles household automation. Tailscale handles private remote access. Uptime Kuma handles service health visibility. Homepage ties the experience together as a launchpad.

That separation makes troubleshooting easier and gives the homelab room to grow.

## Current Status

The environment is functional and actively maintained.

The core infrastructure is running, the service stack is useful, Home Assistant is integrated into household workflows, and the documentation is now being built into a public portfolio.

The next major improvements are firewall hardening, restore testing, stronger monitoring, and more polished diagrams.
