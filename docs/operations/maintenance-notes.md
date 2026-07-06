# Maintenance Notes

This document describes how I currently think about maintaining my homelab.

The environment is actively used and still growing, so my maintenance approach is practical rather than overly formal. My main priority is keeping the services that affect access, DNS, monitoring, dashboards, and household workflows stable.

## What I Care About Most

The most important parts of the environment are the systems that everything else depends on.

My highest-priority systems are:

- Proxmox VE
- Debian Docker VM
- Home Assistant OS VM
- Docker
- Pi-hole
- Tailscale
- Homepage
- Uptime Kuma

If those are healthy, the rest of the environment is usually manageable.

## Regular Checks

The checks I care about most are simple:

- Is the Proxmox host reachable?
- Is the Debian Docker VM running?
- Is Home Assistant running?
- Are the expected Docker containers up?
- Is Pi-hole reachable?
- Is Tailscale connected?
- Is Homepage loading?
- Is Uptime Kuma reporting anything down?

These checks give me a quick sense of whether the environment is healthy.

## Docker Service Review

Most of my services run on the Debian Docker VM, so Docker health matters a lot.

When reviewing Docker services, I care about:

- Containers that are stopped unexpectedly
- Containers stuck restarting
- Containers marked unhealthy
- Services that recently changed after an update
- Disk usage that could become a problem
- Ports or access paths that changed unexpectedly

This is especially important because the Docker VM hosts many unrelated services. One bad change can affect dashboards, monitoring, media, DNS, or household tools.

## Monitoring Review

Uptime Kuma is my quick health view.

I use it to see whether important services are reachable and whether anything has been down or unstable.

Things I want to improve over time:

- Add monitors for any important services not currently tracked
- Remove stale monitors
- Tune alerting so it is useful instead of noisy
- Make the status view more polished for portfolio screenshots

## Homepage Review

Homepage is the launchpad for the environment.

As I add services, I want Homepage to stay organized instead of becoming a messy wall of links.

Current Homepage sections include:

- Core Infrastructure
- Home
- Docker Services
- Media Stack

Homepage now includes Home Assistant, Donetick, Donetick Docker metrics, Tailscale Machines, and other useful service links.

The goal is for Homepage to show the environment clearly at a glance.

## Home Assistant Review

Home Assistant is becoming the household operations layer.

The parts I care about most are:

- Calendar visibility
- Family dashboard usability
- Donetick chore integration
- Chore creation workflows
- Completed-today summaries
- Notifications that are useful instead of annoying

This part of the homelab is especially important because it affects actual household use, not just technical learning.

## Backup Review

The backup script exists, but restore testing is still the big missing piece.

When reviewing backups, I care about:

- Whether backup archives are being created
- Whether important configuration paths are included
- Whether backup storage has enough space
- Whether the backup process still matches the current service layout
- Whether restore documentation is improving

The next real milestone is a documented restore test.

## Change Notes

I want to avoid making too many major changes at once.

Before changing something important, I try to think through what it could affect.

High-impact areas include:

- Proxmox networking
- Debian Docker VM networking
- Docker Compose files
- Pi-hole DNS settings
- Nginx Proxy Manager proxy hosts
- Tailscale access
- Home Assistant core configuration
- Backup script paths
- Firewall rules

If a change affects access, DNS, backups, or household dashboards, it deserves extra care.

## Documentation Review

This repo should stay aligned with the real environment.

Documentation should be updated when:

- A new service is added
- A service is removed
- A service moves to a different host
- Backup scope changes
- Security posture changes
- A planned item becomes completed
- Screenshots or diagrams are added
- A major troubleshooting lesson is learned

The goal is not to document every tiny change. The goal is to keep the repo accurate enough that it remains useful and professional.

## Current Maintenance Gaps

Current gaps include:

- Restore testing still needs to be completed
- UFW/firewall hardening is still planned
- More formal diagrams are still needed
- Monitoring can be improved
- Alerting can be improved
- Some service-specific recovery steps still need to be documented

These are not failures. They are the next areas of growth.

## Maintenance Summary

My maintenance approach is based on keeping the homelab understandable and recoverable.

The environment is most useful when I can quickly answer:

- What is running?
- Where is it hosted?
- What depends on it?
- How do I access it?
- How would I recover it?
- What still needs improvement?

That is the standard I want this documentation to support.
