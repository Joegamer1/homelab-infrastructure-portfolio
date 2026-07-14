# Backup Plan

The current backup process protects configuration-heavy services on the Debian Docker VM. It does not attempt to back up the media library or every VM disk.

## Scope

Current targets include:

- Homepage
- Pi-hole
- Nginx Proxy Manager
- Portainer
- Uptime Kuma
- Selected Docker configuration paths
- Docker container, volume, network, and mount metadata

Persistent data is split across `/srv/docker`, `/opt/docker`, bind mounts, and named volumes. The scope was built by inspecting live container mounts rather than assuming one directory convention.

## Implementation

The private backup script creates timestamped archives, records activity in a log, and removes archives older than the retention period. Archive size and included paths are reviewed because a successful `tar` command does not prove the correct data was captured.

Backups may contain hostnames, tokens, service metadata, and environment-specific values and are not stored in this public repository.

## Validation Status

Completed:

- Selected bind mounts and named volumes are included.
- Docker metadata is captured.
- Timestamped archives, logging, and retention work.

Still required:

- Full service restore testing
- Automated off-host copies
- Backup encryption decision
- VM-level recovery testing
- Application-specific exports where needed

A backup is not fully proven until it has been restored successfully.

## Restore Priorities

1. Proxmox and VM availability
2. Docker Engine and Compose
3. Network and DNS services
4. Reverse proxy and management services
5. Monitoring and dashboards
6. Household services
7. Media applications

A controlled restore should verify archive integrity, file ownership, permissions, service recreation, and recovery of each configuration-heavy application.

## Lessons

- Inspect live mounts before writing backup logic.
- Named volumes are easy to miss.
- Metadata helps reconstruction but does not replace data backups.
- Configuration-focused backups provide high value without media-scale storage.
- Archive creation and recovery validation are separate milestones.