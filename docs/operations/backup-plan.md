# Backup Plan

This document describes the current backup approach for the homelab infrastructure environment.

The goal of the backup process is to preserve important service configuration data, support recovery from service failure or host issues, and clearly separate implemented backup work from restore testing that still needs to be completed.

## Backup Goals

The backup strategy is intended to support:

- Recovery of important Docker service configuration
- Preservation of dashboard and infrastructure service settings
- Recovery of selected service metadata
- Faster rebuild of the Debian Docker VM service stack
- Documentation of what is currently protected
- Identification of restore testing as planned work

This backup plan is focused on infrastructure and service configuration. It is not intended to be a full backup of all media data, databases, VM disks, or personal files.

## Current Backup Scope

A backup script exists for selected Docker and infrastructure configuration paths.

Current backup targets include configuration data for services such as:

- Homepage
- Pi-hole
- Nginx Proxy Manager
- Portainer
- Uptime Kuma
- Docker service metadata
- Selected Docker configuration paths

The backup scope is intentionally focused on configuration and operational recovery data rather than large media libraries or raw production data.

## Backup Storage

Backups are generated outside this public repository.

Backup files should not be committed to GitHub.

Backup archives may contain sensitive configuration data, service metadata, hostnames, tokens, or environment-specific values. They should be treated as private operational artifacts.


## Backup Method

The current backup process uses a script-based approach.

The script collects selected configuration paths and writes them into timestamped backup archives. It also records useful service metadata such as Docker container, network, and volume information where appropriate.

This approach is useful because it preserves enough information to help rebuild or troubleshoot the environment without requiring a full image-level backup of every workload.

## Backup Validation Status

The backup process has been implemented, but full restore testing is still planned.

Current status:

- Backup script exists
- Selected configuration paths are included
- Backup archives can be generated
- Restore testing has not yet been completed
- Recovery documentation is still being expanded

A backup is only fully proven after a successful restore test.

## Restore Testing Plan

Planned restore testing should confirm that backups can be used to recover key services in a controlled way.

A future restore test should verify:

- Backup archive integrity
- Required files are present
- Important service configuration can be restored
- Docker services can be recreated or reattached to restored data
- Homepage configuration can be recovered
- Pi-hole configuration can be recovered
- Nginx Proxy Manager configuration can be recovered
- Portainer data can be recovered
- Documentation accurately reflects the process

## Recovery Priorities

In a recovery scenario, services should be restored based on operational importance.

Suggested recovery order:

1. Proxmox host availability
2. Debian Docker VM availability
3. Docker engine and Compose support
4. Network and DNS services
5. Reverse proxy and management services
6. Monitoring and dashboard services
7. Home automation support services
8. Media stack services

This order prioritizes infrastructure dependencies before convenience services.

## Known Gaps

Current known gaps include:

- Restore testing has not yet been completed
- Backup storage redundancy should be improved over time
- Some services may require additional application-specific export steps
- Media data is outside the current configuration-focused backup scope
- VM-level backup strategy should be documented separately if implemented
- Backup encryption and off-host storage should be evaluated

## Planned Improvements

Planned improvements include:

- Perform a documented restore test
- Add a restore checklist
- Document backup frequency and retention
- Add off-host backup storage
- Review whether backups should be encrypted
- Add VM-level backup notes if Proxmox backups are implemented
- Document service-specific restore requirements

## Operational Summary

The current backup approach provides a practical foundation for infrastructure recovery by preserving important configuration data and service metadata.

The next major improvement is restore testing. Until restore testing is completed, the backup system should be treated as useful but not fully validated.
