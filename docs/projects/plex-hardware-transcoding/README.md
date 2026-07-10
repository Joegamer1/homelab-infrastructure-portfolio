# Plex Hardware Transcoding and Media Delivery

This project documents the work required to make Plex hardware transcoding function reliably through a Proxmox-hosted Debian Docker VM.

The final result depended on several layers working together. The project was not only a Plex setting change; it required hardware identification, IOMMU configuration, PCI passthrough, guest driver support, container runtime configuration, and playback validation.

## Project Goal

Use the NVIDIA Quadro P620 installed in the Dell Precision T7820 to handle Plex transcoding workloads instead of relying entirely on the VM's CPU.

This was intended to:

- Reduce CPU load during incompatible playback.
- Improve remote streaming performance.
- Support more consistent playback across different clients.
- Preserve the Debian VM as the central Docker service host.

## Architecture

The working path is:

```text
Dell Precision T7820 hardware
        ↓
Proxmox VE with IOMMU
        ↓
PCI passthrough to Debian Docker VM
        ↓
NVIDIA guest drivers and libraries
        ↓
NVIDIA container runtime/device access
        ↓
Plex Media Server hardware transcoding
```

A failure at any layer can make Plex fall back to software transcoding or fail to see the GPU entirely.

## Hardware Identification

The Quadro P620 exposes two PCI functions:

- NVIDIA graphics function
- NVIDIA audio function

Both functions were identified and handled together for passthrough.

The relevant PCI addresses and device IDs were verified on the Proxmox host rather than copied from a generic example.

## Proxmox Configuration

### IOMMU

Intel IOMMU was enabled through the Proxmox boot configuration using the appropriate kernel parameters.

The host was rebooted and the IOMMU state was verified before continuing.

### VFIO binding

The Quadro graphics and audio device IDs were assigned to `vfio-pci`.

The open-source Nouveau driver was blacklisted on the Proxmox host so the hypervisor would not claim the GPU before passthrough.

After rebooting, both PCI functions were checked to confirm they were using `vfio-pci`.

### VM passthrough

The GPU functions were added to the Debian Docker VM. The VM uses a Q35 machine type and host CPU configuration, which supports the passthrough design.

## Guest and Container Configuration

Passing the device into the VM was only the first half of the work.

The Debian guest also required:

- NVIDIA driver support
- Access to the device nodes
- The NVIDIA container runtime
- Correct library paths
- Plex container configuration that requested GPU access

The working Plex setup includes the NVIDIA runtime/device configuration and the required NVIDIA library path inside the container.

A custom LinuxServer initialization script is mounted into the container to preserve required startup behavior.

## Problems Encountered and Resolutions

### The GPU existed on the host but was not available to the VM

**Problem:** Installing the GPU physically did not make it available to Plex.

**Investigation:** Verified the PCI device, IOMMU state, current host driver, and VM hardware assignment separately.

**Resolution:** Enabled IOMMU, bound both functions to `vfio-pci`, blacklisted Nouveau, and passed the device into the VM.

**Lesson:** Hardware passthrough must be validated layer by layer. The presence of a PCI device does not prove that the correct driver owns it.

### Passing the GPU into the VM did not automatically expose it to Docker

**Problem:** The Debian VM could receive the hardware while the Plex container still lacked access.

**Resolution:** Installed the required guest-side NVIDIA support and configured the Plex container to use the NVIDIA runtime and devices.

### NVIDIA libraries needed to be available inside the container

**Problem:** Device visibility alone was insufficient when the container could not find the expected NVIDIA libraries.

**Resolution:** Added the working `LD_LIBRARY_PATH` value for the Debian NVIDIA library location.

### Recreating the Plex container risked losing the working configuration

**Problem:** Ad hoc container changes can disappear during recreation.

**Resolution:** Preserved the GPU runtime, device, environment, and initialization settings in the Compose configuration and documented the clean recreation process.

The current workflow is to recreate Plex from its Compose directory rather than manually replacing the container:

```bash
cd /srv/docker/plex
docker compose up -d --force-recreate
```

### Playback problems were not always video-transcoding problems

**Problem:** A stream could appear to be mostly compatible while still buffering or transcoding.

**Investigation:** Read Plex playback details for video and audio separately.

One observed case showed:

- Video: Direct Stream
- Audio: DTS-HD MA 7.1 transcoded to AAC

The video was not the main compatibility issue; the client could not directly play the selected audio format.

**Resolution:** Treated audio compatibility as a separate release-selection and client-support concern rather than assuming every playback issue required more GPU capacity.

### A second public port was considered as a capacity fix

**Problem:** It was unclear whether exposing Plex through an additional external port would help with multiple simultaneous users.

**Finding:** An extra port does not create more server compute, upload bandwidth, or transcoding capacity.

**Resolution:** Kept the focus on actual playback mode, network throughput, and hardware resources.

## Playback Validation

Hardware transcoding is validated through multiple checks:

- Confirm the GPU is bound to `vfio-pci` on Proxmox.
- Confirm the Debian VM sees the NVIDIA device.
- Confirm the Plex container has NVIDIA device access.
- Start a playback session that requires transcoding.
- Inspect Plex playback details.
- Verify the transcoding indicator shows hardware use where expected.
- Check whether video, audio, or both are being transcoded.

This avoids treating “the stream played” as sufficient proof that hardware acceleration is functioning.

## Direct Play, Direct Stream, and Transcoding

The project also improved the operational understanding of Plex playback modes.

### Direct Play

The original media file is delivered without modifying the video or audio.

### Direct Stream

The compatible streams are repackaged into a different container without re-encoding the video.

### Transcoding

Plex re-encodes incompatible video or audio. Video transcoding is substantially more resource-intensive than simple remuxing or many audio transcodes.

Reading these fields separately made troubleshooting more accurate.

## Media Selection Lessons

Hardware transcoding is useful, but avoiding unnecessary transcoding is still preferable.

Release-selection decisions can improve compatibility by considering:

- Video codec
- HDR behavior
- Audio codec
- Channel layout
- Client support
- Subtitle behavior

For example, releases with a broadly compatible audio track may provide a better client experience than relying on Plex to transcode DTS-HD for every session.

## Known Limitations

- Remote performance still depends on available upload bandwidth and the remote client's connection.
- Audio compatibility may trigger transcoding even when video is compatible.
- Tailscale routing can affect the observed network path and performance.
- One entry-level GPU still has finite simultaneous-transcode capacity.
- Hardware acceleration does not solve storage, network, or client-decoder limitations.
- Sanitized screenshots of hardware-transcoding validation still need to be added.

## What I Would Do Differently

- Document every host and guest verification command during the first implementation.
- Capture sanitized before-and-after Plex playback screenshots.
- Standardize the Plex Compose file earlier instead of relying on temporary container changes.
- Test known video and audio combinations as a small compatibility matrix.
- Record expected behavior for local, public remote, and Tailscale playback separately.

## Skills Demonstrated

- Proxmox PCI passthrough
- Intel IOMMU configuration
- VFIO driver binding
- Linux kernel driver troubleshooting
- NVIDIA guest and container integration
- Docker Compose persistence
- Plex playback diagnostics
- Cross-layer infrastructure troubleshooting
- Performance and compatibility analysis
