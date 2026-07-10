# Backup Plan

This document describes the current backup approach for my homelab, including the iterative work used to determine what needed to be preserved.

The backup process is focused on important configuration data so the environment is easier to recover, rebuild, or troubleshoot if something breaks. This is not a full disaster recovery plan yet. Restore testing remains planned work rather than being implied by the existence of backup archives.

## Backup Goals

- Preserve important Docker service configuration.
- Preserve selected infrastructure service settings.
- Capture useful Docker metadata.
- Make the Debian Docker VM service stack easier to rebuild.
- Avoid losing dashboard, DNS, reverse proxy, monitoring, and management configuration.
- Build toward a documented restore process.

This plan does not attempt to back up all media data, every VM disk, or every personal file.

## How the Backup Scope Was Developed

The first challenge was that persistent data did not live in one standard directory. The environment had grown through several deployment stages and used both bind mounts and Docker named volumes.

The backup work therefore began with discovery rather than immediately writing an archive command.

### Step 1: Inspect active storage

Running containers were inspected to identify their real mounts. This revealed important paths such as:

- Homepage configuration under `/opt/docker/homepage/config`
- Uptime Kuma data under `/opt/docker/uptime-kuma/data`
- Pi-hole bind mounts under `/srv/docker/pihole`
- Nginx Proxy Manager data in Docker named volumes
- Portainer data in a Docker named volume

This prevented an early backup script from relying only on the newer `/srv/docker` directory convention and silently missing older deployments.

### Step 2: Prioritize configuration over bulk data

The first version of the plan intentionally excluded replaceable or extremely large data such as the media library. The focus became the files that represented the most manual configuration effort.

### Step 3: Capture operational metadata

The script was expanded to record Docker metadata such as containers, volumes, networks, and mounts. These records are not substitutes for data backups, but they make reconstruction and troubleshooting easier.

### Step 4: Test archive generation repeatedly

Multiple timestamped archives were created while the script and included paths were being verified. Archive size was used as a basic sanity check; an unexpectedly small archive could indicate missing data or a path problem.

### Step 5: Add logging and retention

The script writes to a backup log and removes archives older than the configured retention period.

## Current Backup Scope

Current targets include:

- Homepage
- Pi-hole
- Nginx Proxy Manager
- Portainer
- Uptime Kuma
- Docker service metadata
- Selected Docker configuration paths

The scope is intentionally practical. The most important goal is to reduce the amount of manual work required to rebuild the service stack.

## Current Implementation

The backup directory is outside the public repository. The script creates timestamped archives and records its activity in a private operational log.

Backups may contain sensitive configuration, service metadata, hostnames, tokens, and environment-specific values. They should not be committed to GitHub.

## Problems Encountered and Resolutions

### Persistent data was split across multiple storage models

**Problem:** Backing up only `/srv/docker` would have omitted active services stored under `/opt/docker` and Docker named volumes.

**Resolution:** Inspected each running container and included the real data paths rather than assuming a single directory structure.

### A successful command could still produce an incomplete backup

**Problem:** `tar` completing without an obvious error did not prove the correct service data had been captured.

**Resolution:** Reviewed archive sizes, enumerated included paths, and created metadata receipts that can be checked during recovery.

### Repeated testing created several valid-looking archives

**Problem:** Iterative script changes produced multiple archives in a short period, making it difficult to distinguish test output from the preferred current result.

**Resolution:** Added consistent timestamped naming, logging, and retention. Future restore tests should record the exact archive used.

### Backups initially existed only on the same system

**Problem:** A backup stored solely on the Debian VM does not protect against VM or host-storage failure.

**Resolution:** Documented the need for off-host copies. Copying archives to another personal system is an improvement, but a more formal redundant backup target is still planned.

## Backup Validation Status

Completed:

- Backup script exists.
- Selected bind mounts and named volumes are included.
- Docker metadata is captured.
- Timestamped archives can be generated.
- Logging and retention are configured.

Not yet completed:

- Full service restore testing.
- Formal off-host automation.
- Backup encryption decision.
- VM-level recovery testing.

A backup is only fully proven after a successful restore.

## Restore Testing Plan

A future controlled restore should verify:

- Archive integrity.
- Required files are present.
- File ownership and permissions can be restored correctly.
- Docker services can be recreated or reattached to restored data.
- Homepage configuration can be recovered.
- Pi-hole configuration can be recovered.
- Nginx Proxy Manager data and certificates can be recovered.
- Portainer data can be recovered.
- Uptime Kuma monitors can be recovered.
- Documentation matches the actual procedure.

## Recovery Priorities

1. Proxmox host availability
2. Debian Docker VM availability
3. Docker Engine and Compose support
4. Network and DNS services
5. Reverse proxy and management services
6. Monitoring and dashboard services
7. Household support services
8. Media application services

This order prioritizes dependencies before convenience services.

## Known Gaps

- Restore testing has not yet been completed.
- Backup storage redundancy should be improved.
- Some services may require application-specific exports.
- Media data is outside the current scope.
- VM-level backups need separate documentation if implemented.
- Encryption and automated off-host storage still need evaluation.

## Lessons Learned

- Inspect live mounts before writing backup logic.
- Named volumes are easy to overlook when most newer services use bind mounts.
- Archive creation is not recovery validation.
- Configuration-focused backups provide high value without requiring media-scale storage.
- Incomplete recovery work should remain explicitly labeled as incomplete.
