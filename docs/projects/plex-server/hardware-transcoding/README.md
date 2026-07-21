# Plex Hardware Transcoding

This project passes an NVIDIA Quadro P620 through Proxmox to the Debian Docker VM and exposes it to Plex for hardware transcoding.

## Architecture

```text
Dell Precision T7820
-> Proxmox with IOMMU and VFIO
-> Debian Docker VM
-> NVIDIA guest drivers and container runtime
-> Plex container
```

Both the GPU and its audio function are passed through together. A failure at any layer can leave Plex without hardware acceleration.

## Implementation

### Proxmox

- Enable Intel IOMMU.
- Bind both NVIDIA PCI functions to `vfio-pci`.
- Prevent Nouveau from claiming the device.
- Add the GPU functions to the Q35 Debian VM.

### Debian and Docker

- Install the NVIDIA guest driver.
- Confirm the device nodes are present.
- Configure NVIDIA container support.
- Expose the device and required libraries to Plex.
- Preserve the runtime and environment settings in Compose.

The container is recreated from its Compose directory:

```bash
cd /srv/docker/plex
docker compose up -d --force-recreate
```

## Validation

Hardware transcoding is checked at each layer:

1. Proxmox shows `vfio-pci` as the device owner.
2. Debian sees the NVIDIA device.
3. The Plex container has device and library access.
4. A test stream requires transcoding.
5. Plex playback details show hardware use.

Playback details are read separately for video and audio. A stream may direct-stream video while transcoding an incompatible audio track, such as DTS-HD MA to AAC.

## Lessons

- PCI presence does not prove correct driver ownership.
- VM passthrough does not automatically grant Docker access.
- Device access alone is insufficient if the container cannot find the NVIDIA libraries.
- A second external Plex port does not add compute, bandwidth, or transcode capacity.
- Avoiding unnecessary transcoding through compatible video, audio, subtitles, and client choices remains preferable.

## Limitations

Remote playback still depends on upload bandwidth, client support, and the selected network path. Audio may transcode even when video is compatible, and the P620 has finite simultaneous-transcode capacity.

Sanitized validation screenshots and a small client/codec test matrix remain planned.

[Back to Plex Server](../)
