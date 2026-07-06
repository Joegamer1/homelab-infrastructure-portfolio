# Restore Notes

This document tracks my current thinking around restoring the homelab from backups.

The backup script exists, but restore testing is still planned work. I am documenting restore notes separately because backups are only useful if I can actually recover from them.

## Current Restore Status

Restore testing has not yet been fully completed.

Current status:

- Backup script exists
- Backup archives can be generated
- Selected configuration paths are included
- Restore process is not fully tested yet
- Service-specific restore steps still need to be documented

For now, I consider the backup system useful but not fully proven.

## Restore Goal

The goal of restore testing is to confirm that I can recover important services without relying entirely on memory.

I want to be able to answer:

- Where are the backups?
- What is inside each backup?
- Which services can be restored from them?
- What order should services be restored in?
- What manual steps are still required?
- What gaps exist in the current backup process?

## Most Important Recovery Targets

The first restore testing should focus on services that affect visibility, access, DNS, and service management.

Priority targets:

1. Debian Docker VM service layout
2. Docker and Compose support
3. Homepage configuration
4. Pi-hole configuration
5. Nginx Proxy Manager configuration
6. Portainer data
7. Uptime Kuma configuration
8. Donetick configuration
9. Home Assistant integration notes

The media stack matters, but I care more about recovering the infrastructure layer first.

## Suggested Recovery Order

In a real recovery scenario, I would restore services in this order:

1. Confirm Proxmox host is available
2. Confirm or rebuild the Debian Docker VM
3. Install Docker and Docker Compose support
4. Restore Docker service directories and configuration
5. Restore DNS-related services
6. Restore reverse proxy and management services
7. Restore monitoring and dashboard services
8. Restore home automation support services
9. Restore media stack services

This order prioritizes the services that help me regain control and visibility.

## Restore Test Plan

A controlled restore test should include:

- Creating or using a test restore location
- Extracting a backup archive
- Reviewing the archive contents
- Confirming expected files are present
- Restoring one service at a time
- Starting the restored service
- Verifying the service is reachable
- Documenting any manual fixes required

The first restore test does not need to be perfect. The goal is to learn what works, what breaks, and what needs better documentation.

## Service-Specific Restore Notes

### Homepage

Homepage is a good first restore target because it is visible and relatively low risk.

Restore validation should confirm:

- Service links are present
- Dashboard sections load correctly
- Docker widgets or service metrics still work where configured
- No private values are accidentally introduced into public documentation

### Pi-hole

Pi-hole is important because DNS affects the rest of the environment.

Restore validation should confirm:

- Pi-hole starts correctly
- Admin interface is reachable
- DNS resolution works
- Expected local DNS records are present
- Blocking lists and settings are restored as expected

### Nginx Proxy Manager

Nginx Proxy Manager is important if service access depends on proxy hosts.

Restore validation should confirm:

- Admin interface is reachable
- Proxy hosts are present
- Certificates or certificate references behave as expected
- Proxy routes work after restoration

### Portainer

Portainer provides Docker management visibility.

Restore validation should confirm:

- Portainer starts correctly
- Existing Docker environment is visible
- Volumes, containers, and networks are visible as expected
- Authentication still works or can be safely reset

### Uptime Kuma

Uptime Kuma provides service monitoring.

Restore validation should confirm:

- Monitors are present
- Status pages are present if used
- Monitors correctly report service state
- Notification settings are reviewed before enabling alerts

### Donetick

Donetick supports household chore workflows.

Restore validation should confirm:

- Donetick starts correctly
- Chore data is present if included in backup scope
- Home Assistant can still communicate with Donetick
- Completion notifications continue working

## Known Restore Gaps

Current known gaps include:

- Restore testing has not been completed yet
- Not every service has documented restore steps
- Some services may require application-specific export or database handling
- Backup encryption has not been fully documented
- Off-host backup storage needs improvement
- VM-level restore strategy is not fully documented yet

## Lessons to Capture During Testing

During restore testing, I want to document:

- What restored cleanly
- What required manual fixes
- What was missing from the backup
- Which services were easiest to recover
- Which services need better backup coverage
- Whether the recovery order made sense
- How long the restore took

This is where the restore process becomes more than theory.

## Future Improvements

Planned improvements include:

- Perform a documented restore test
- Add service-specific restore steps
- Add backup archive inspection commands
- Document restore commands where safe
- Add VM-level restore notes if Proxmox backups are implemented
- Improve off-host backup storage
- Review whether backups should be encrypted

## Restore Summary

The current restore process is still developing.

The backup script gives me a starting point, but the next important step is proving that key services can actually be restored. Once I complete a restore test, this document should be updated with the exact lessons learned.
