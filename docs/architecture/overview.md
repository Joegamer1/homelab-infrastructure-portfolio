# Architecture Overview

This document provides a high-level overview of the homelab infrastructure architecture, including the virtualization layer, primary service host, home automation environment, and major supporting services.

## Purpose

The purpose of this environment is to provide a practical, self-hosted infrastructure platform for learning, household operations, automation, monitoring, and service management.

The environment is designed to demonstrate:

- Virtualization and VM lifecycle management
- Linux server administration
- Docker-based service hosting
- Service monitoring and health visibility
- Private remote access
- Internal DNS and reverse proxy management
- Home automation integration
- Backup planning and operational documentation
- Incremental infrastructure hardening

## Physical Host

The environment is built on a Dell Precision T7820 workstation running Proxmox VE.

Proxmox provides the core virtualization layer and allows services to be separated into dedicated virtual machines instead of running everything directly on one bare-metal operating system.

## Virtualization Layer

Proxmox VE is used to host the main infrastructure workloads.

Current major virtual machines include:

- Debian Docker VM
- Home Assistant OS VM

This separation allows the infrastructure service stack and home automation platform to be managed independently.

## Debian Docker VM

The Debian Docker VM is the primary service host for most containerized applications.

It hosts services including:

- Docker
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

The Debian VM acts as the operational core for self-hosted services.

## Home Assistant OS VM

Home Assistant OS runs as a dedicated VM in Proxmox.

Home Assistant is used for household automation and dashboarding, including:

- Family dashboard views
- Google Calendar integration
- Donetick chore integration
- Chore status visibility
- Completed-today summaries
- Chore creation workflows
- Automation-backed notifications

Keeping Home Assistant in its own VM reduces coupling between the home automation layer and the general Docker service stack.

## Service Organization

Services are organized into functional groups.

### Core Infrastructure

- Proxmox VE
- Debian Docker VM
- Portainer
- Homepage
- Uptime Kuma
- Pi-hole
- Nginx Proxy Manager
- Tailscale

### Home Services

- Home Assistant
- Donetick
- Google Calendar integration

### Media Services

- Plex
- Seerr
- Radarr
- Sonarr
- Prowlarr
- SABnzbd

## Remote Access Model

Remote access is handled through Tailscale.

The design goal is to avoid exposing administrative interfaces directly to the public internet. Management services are intended to remain reachable through private network paths rather than public port forwarding.

## Monitoring Model

Uptime Kuma provides service health monitoring and status visibility.

Homepage provides a central dashboard for launching and viewing services, including selected Docker metrics and organized service sections.

The monitoring approach is intended to make service state visible without requiring direct inspection of every container or VM.

## DNS and Reverse Proxy Model

Pi-hole provides internal DNS and ad-blocking capabilities.

Nginx Proxy Manager provides a reverse proxy management layer for services where proxying is useful.

The environment separates internal name resolution, reverse proxy management, and private remote access so each layer has a clear role.

## Backup Approach

A backup script exists for selected Docker and infrastructure configuration paths, including important service configuration locations.

Restore testing is planned work and should be completed before the backup process is considered fully validated.

## Security Approach

The environment follows a private-by-default approach.

Current security-related work includes:

- SSH hardening
- Private remote access using Tailscale
- Avoidance of public exposure for management interfaces
- Sanitized public documentation
- Separation of completed work from planned work

Planned security improvements include:

- UFW/firewall hardening
- Restore testing
- More formal hardening documentation
- Improved monitoring and alerting
- Security-focused lab work in a separate future repository

## Design Rationale

This architecture was chosen to balance practicality, learning value, and operational reliability.

Key design decisions include:

- Using Proxmox for workload separation
- Using a Debian Docker VM as the main service host
- Running Home Assistant OS separately from the Docker service stack
- Using Tailscale for private remote access
- Using Homepage and Uptime Kuma for visibility
- Keeping public documentation sanitized and architecture-focused

## Current Status

The environment is functional and actively maintained.

Completed work includes virtualization setup, Docker service hosting, service monitoring, Home Assistant deployment, Donetick integration, dashboarding, backup scripting, and SSH hardening.

UFW/firewall hardening, restore testing, and formal diagrams are planned future improvements.
