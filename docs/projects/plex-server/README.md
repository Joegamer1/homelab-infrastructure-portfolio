# Plex Server

This project documents the Plex media platform running on the Debian Docker VM in my Proxmox homelab. Plex is one part of a broader automated media workflow rather than a standalone application.

## Current Design

```text
Seerr requests
    -> Radarr / Sonarr
    -> Prowlarr
    -> SABnzbd
    -> media import
    -> subtitle post-processing
    -> Plex library analysis
    -> playback on local and remote clients
```

Plex runs as a LinuxServer.io Docker container with host networking. Media is stored under `/srv/media`, while persistent Plex configuration is stored under `/srv/docker/plex`.

## Supporting Services

- **Seerr** provides the request interface.
- **Radarr** manages movies.
- **Sonarr** manages television series.
- **Prowlarr** manages indexers.
- **SABnzbd** handles downloads.
- **Plex** provides library management and playback.
- **Tailscale** provides private remote administrative access and an alternate playback path.

## Implemented Improvements

### Automated media workflow

Movie and television requests flow through the Arr stack and are imported into the shared media library without manual file movement. The containers use consistent paths so download handling and imports remain predictable.

See [Media automation](media-automation/).

### NVIDIA hardware transcoding

A Quadro P620 is passed through from Proxmox to the Debian VM and exposed to the Plex container. This allows supported video transcodes to use dedicated hardware rather than relying entirely on VM CPU resources.

See [Hardware transcoding](hardware-transcoding/).

### Subtitle standardization

A post-processing pipeline checks imported MKV and MP4-family files, removes unnecessary embedded subtitle tracks, retains one preferred English subtitle when available, and requests a Plex analysis after the file is updated.

The same process was later applied to the small existing Movies and TV libraries so historical files and future imports follow the same policy.

See [Subtitle processing](subtitle-processing/).

### Remote playback and client compatibility

Plex Remote Access is enabled through a single public port forward. Administrative services remain private. Remote playback quality is still constrained by available upload bandwidth and client codec support, so direct play remains preferable to unnecessary transcoding.

## Operational Notes

- The Plex container is recreated from `/srv/docker/plex` with Docker Compose.
- New imports are processed automatically.
- Subtitle processing preserves file ownership, permissions, and timestamps.
- Plex analysis is requested after successful media modification.
- Logs are retained for troubleshooting and retry validation.
- Plex is the only media service intentionally exposed through the router; administrative interfaces remain limited to LAN or Tailscale access.

## Current Limitations

- Household internet upload bandwidth limits high-bitrate remote streaming.
- Some clients still require audio transcoding for formats such as DTS-HD MA.
- The Quadro P620 has finite simultaneous-transcode capacity.
- Tailscale is useful for private access but can introduce a less direct playback path than local LAN playback.

## Planned Improvements

- Add a small playback compatibility test matrix.
- Add sanitized screenshots showing hardware transcoding and pipeline validation.
- Continue refining release preferences to favor direct-play-compatible audio and video.
- Revisit firewall hardening when local or Proxmox console access is available.
