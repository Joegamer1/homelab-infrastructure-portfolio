# Plex Media Automation

This project documents the automated request, download, import, and library workflow supporting Plex.

## Architecture

```text
Seerr
  -> Radarr / Sonarr
  -> Prowlarr
  -> SABnzbd
  -> /srv/media/downloads
  -> /srv/media/movies or /srv/media/tv
  -> subtitle post-processing
  -> Plex
```

## Services

- **Seerr** provides the household request interface.
- **Radarr** manages movie monitoring, downloads, naming, and imports.
- **Sonarr** provides the same workflow for television series.
- **Prowlarr** centralizes indexer configuration.
- **SABnzbd** downloads content from the Usenet provider.
- **Plex** scans and presents the completed media library.

The request frontend container is named `seerr`.

## Storage Layout

```text
/srv/media/downloads/incomplete
/srv/media/downloads/complete/movies
/srv/media/downloads/complete/tv
/srv/media/movies
/srv/media/tv
```

Using one consistent host-side media tree simplifies Docker volume mappings and prevents import failures caused by mismatched paths.

## Import Workflow

1. A movie or series is requested through Seerr.
2. Radarr or Sonarr selects a release based on its quality profile.
3. Prowlarr supplies indexer results.
4. SABnzbd downloads the release.
5. Radarr or Sonarr imports and renames the completed media.
6. The subtitle queue processes the imported file.
7. Plex analyzes the updated item and exposes it to clients.

## Playback Policy

The preferred outcome is Direct Play. Hardware transcoding is available when necessary, but release selection and client compatibility remain the cleaner first line of defense.

Operational considerations include:

- Prefer compatible video and audio formats where practical.
- Avoid unnecessary WEBRip releases through quality-profile controls.
- Recognize that audio can transcode even when video direct-plays.
- Treat Tailscale playback as an alternate path rather than assuming it matches LAN performance.
- Limit remote bitrate to fit the household's available upload bandwidth.

## Remote Access

Plex uses a single public router port forward for normal remote access. The remaining media-management interfaces stay private on the LAN or through Tailscale.

This keeps the public exposure narrow while preserving remote administration.

## Operations

Useful container recreation command for Plex:

```bash
cd /srv/docker/plex
docker compose up -d --force-recreate
```

The media stack is monitored through container status checks and a custom health-check script. Disk capacity is also monitored because low free space previously interrupted successful imports.

## Lessons

- Shared and consistent Docker paths matter as much as application configuration.
- Download completion does not guarantee a successful import when permissions or disk space are wrong.
- Direct Play compatibility is usually more valuable than adding more transcoding capacity.
- Remote playback failures can come from upload bandwidth, client support, audio format, or network path—not only Plex itself.
- Automation should remain observable through logs and health checks rather than operating as an opaque chain.

[Back to Plex Server](../)
