# Historical Build and Troubleshooting Narrative

This document records the iterative work that led to the current homelab. It focuses on problems, tests, failed assumptions, and the reasoning behind the final design rather than presenting the environment as though it appeared fully formed.

## Proxmox Rebuild and Host Foundation

### Goal

Create a stable virtualization foundation on the Dell Precision T7820 and avoid mixing application services directly into the hypervisor.

### Challenges

- Rebuilding the Proxmox host meant re-establishing repositories, updates, networking, remote access, and VM definitions.
- Remote work created additional risk because a networking mistake could remove access to the host.
- Hardware passthrough work required distinguishing host-driver problems from guest-driver problems.

### Iteration

1. Installed and updated Proxmox VE.
2. Configured the no-subscription repository.
3. Restored private remote management through Tailscale.
4. Created the Debian Docker VM as the primary service host.
5. Added QEMU Guest Agent support.
6. Investigated a guest-agent timeout caused by the missing virtio communication path.
7. Enabled the agent in the Proxmox VM configuration and verified the guest-side service.
8. Kept application containers off the Proxmox host to preserve a clean virtualization boundary.

### Result

The T7820 became a dedicated virtualization platform rather than a mixed-use server. This reduced coupling and made later troubleshooting easier because hypervisor, VM, and application problems had clearer boundaries.

## Debian Docker VM

### Goal

Create one Linux VM for the majority of containerized services.

### Challenges

- The VM needed to remain lightweight while still supporting Docker, Compose, remote administration, and storage mounts.
- Initial paths were spread between `/opt/docker`, `/srv/docker`, Docker named volumes, and service-specific mounts.
- Container examples often assumed names and paths that did not match the actual environment.

### Iteration

1. Installed Debian without a desktop environment.
2. Enabled SSH and the QEMU Guest Agent.
3. Installed Docker and deployed Portainer.
4. Added infrastructure services incrementally rather than as one large stack.
5. Inspected live container mounts to identify where persistent data actually lived.
6. Standardized newer application data under `/srv/docker` where practical while documenting older `/opt/docker` paths that remained active.
7. Corrected ownership so the administrative user could manage the intended directories without operating as root for routine edits.

### Result

The Debian VM became the main application platform. The mixed historical paths are documented rather than hidden, and future services can follow the cleaner `/srv/docker/<service>` convention.

### Lesson

Standardization usually happens after a system begins growing. The important operational step is to inspect and document the real state before moving anything.

## Pi-hole and Internal DNS

### Goal

Provide internal DNS and memorable service names.

### Challenges

- DNS services require port 53 and can conflict with existing resolvers.
- The web interface was exposed on a non-default host port.
- Internal records needed to resolve both locally and through the private remote-access path.

### Iteration

1. Deployed Pi-hole with persistent configuration mounts.
2. Mapped the web interface to port 8081 while preserving DNS on TCP and UDP 53.
3. Confirmed the correct admin path was `/admin` rather than the host root.
4. Added internal names such as service-specific `.home.arpa` records.
5. Verified name resolution using the intended DNS path instead of assuming browser success proved DNS was correct.

### Result

Core services can be reached by internal names rather than raw IP addresses, and the Pi-hole container remains isolated from the host operating system.

## Nginx Proxy Manager

### Goal

Provide a manageable reverse-proxy layer for internal services and future controlled publication.

### Challenges

- Initial login and credential state did not behave as expected.
- Named Docker volumes made data locations less obvious than bind-mounted services.
- It was easy to assume that because Nginx Proxy Manager was installed, it was already providing value to every service.

### Iteration

1. Deployed the container and database-backed configuration.
2. Resolved initial login problems through account/reset troubleshooting.
3. Inspected named volumes to identify persistent data and certificate storage.
4. Separated the fact that Nginx Proxy Manager was running from the question of whether a given application was actually routed through it.

### Result

Nginx Proxy Manager is available as an access layer, but documentation avoids claiming it protects or fronts services that still use direct ports.

### Lesson

Installed is not the same as integrated.

## Uptime Kuma

### Goal

Provide simple service-health visibility.

### Challenges

- A dashboard can show links to services without proving that those services are healthy.
- Monitoring the container host from the same host has limitations during full host failure.

### Iteration

1. Deployed Uptime Kuma with persistent storage.
2. Added service monitors after confirming stable URLs and ports.
3. Used Homepage for navigation and Uptime Kuma for active availability checks instead of duplicating both roles.
4. Documented that the current setup is useful operational monitoring, not full observability or independent external monitoring.

### Result

The environment has an accessible status view and clearer separation between launchpad and monitoring responsibilities.

## Tailscale Remote Access

### Goal

Reach management services remotely without exposing administrative interfaces to the public internet.

### Challenges

- Different systems had different Tailscale identities and addresses.
- Browser callbacks and applications occasionally generated `localhost` or LAN-based URLs that did not work from the remote client.
- Some setup flows appeared to hang while authentication pages loaded slowly.

### Iteration

1. Installed Tailscale on the Proxmox host and relevant VMs.
2. Verified each machine separately rather than treating the tailnet as a single endpoint.
3. Used the correct Tailscale address for the relevant host.
4. Worked around setup callbacks that referenced `localhost` by returning to the actual Home Assistant address after authentication.
5. Kept administrative services private and reserved traditional port forwarding for the specific service that required it.

### Result

Proxmox, the Debian VM, Home Assistant, and internal services are manageable through a private overlay network.

### Lesson

Remote access failures are often address-context problems: the service may be healthy, but the URL is valid only from the machine that generated it.

## Plex and Media Stack

### Goal

Build a self-hosted media workflow with request management, automated acquisition, and hardware-accelerated playback.

### Challenges

- Multiple services needed consistent paths and permissions.
- The request frontend's real container name differed from generic examples.
- Plex remote access, direct play, direct stream, and transcoding were initially easy to conflate.
- Audio formats could trigger transcoding even when the video stream was compatible.
- Pausing or resuming playback exposed buffering behavior that was not caused by simple port availability.
- GPU passthrough and container GPU visibility required configuration at several layers.

### Iteration

1. Deployed Plex, SABnzbd, Radarr, Sonarr, Prowlarr, and the request frontend.
2. Corrected operational commands to use the actual `seerr` container name.
3. Tested downloads and verified media files directly over SSH instead of relying only on application status.
4. Distinguished Plex playback modes:
   - Direct Play
   - Direct Stream
   - Video transcode
   - Audio-only transcode
5. Identified DTS-HD audio as a source of audio transcoding on clients that otherwise played the video directly.
6. Reviewed release-selection preferences to favor more compatible audio where appropriate.
7. Tested public Plex remote access separately from Tailscale-based access.
8. Confirmed that opening an additional public port would not increase simultaneous-stream capacity.
9. Configured NVIDIA GPU passthrough from Proxmox to the Debian VM.
10. Bound the Quadro GPU functions to `vfio-pci` on the host.
11. Exposed the GPU to the Plex container with the NVIDIA runtime and required library paths.
12. Preserved the working GPU configuration through a documented Compose recreation process and custom initialization mount.

### Result

The media stack supports automated requests and hardware-assisted Plex transcoding, with a clearer understanding of when a client is direct-playing versus transcoding.

### Lessons

- A second forwarded port does not create more compute or bandwidth capacity.
- Playback diagnostics must be read at the video and audio level separately.
- GPU acceleration is an end-to-end chain: host IOMMU, passthrough binding, guest drivers, container runtime, and application recognition all have to work.

## Plex Public Exposure and SSH Hardening

### Goal

Enable Plex remote access while keeping the rest of the management plane private.

### Challenges

- Public access for one application can easily turn into unnecessary exposure for unrelated services.
- Firewall changes performed remotely can lock out the administrator.

### Iteration

1. Forwarded only the port required for Plex remote access.
2. Verified that management services remained LAN/Tailscale-only.
3. Disabled root SSH login.
4. Restricted SSH to the intended administrative user.
5. Deferred UFW activation until local or Proxmox console access would be available for recovery.

### Result

Plex has the required public path while administrative services remain private. SSH hardening is complete; host-level UFW hardening remains intentionally pending.

### Lesson

Delaying a security control can be the correct decision when applying it without a recovery path creates a larger operational risk. The delay should be documented and revisited, not forgotten.

## Backup Script

### Goal

Preserve enough configuration and metadata to rebuild important services without backing up the entire media library.

### Challenges

- Persistent data was split across bind mounts and Docker named volumes.
- A successful archive command did not prove that all necessary paths were included.
- Repeated test runs created several archives while the script was being corrected.

### Iteration

1. Inventoried active bind mounts and named volumes.
2. Selected configuration-heavy services for the first backup scope.
3. Added staging output for Docker container, volume, network, and mount metadata.
4. Generated timestamped archives.
5. Reviewed archive sizes and contents to catch obviously incomplete runs.
6. Added retention handling and logging.
7. Explicitly separated completed backup generation from still-pending restore validation.

### Result

The backup script can create configuration-focused archives and operational metadata. Restore testing remains the next required proof step.

### Lesson

A backup job succeeding proves that a file was written. It does not prove recoverability.

## Home Assistant OS Deployment

### Goal

Run Home Assistant as a dedicated household operations platform rather than another application container.

### Challenges

- The add-on menu was not immediately visible during onboarding.
- Tailscale authentication loaded slowly and appeared stuck.
- Authentication callbacks pointed to `localhost`, which was incorrect from the user's device.

### Iteration

1. Created a dedicated Home Assistant OS VM.
2. Completed onboarding and installed Tailscale through the supported Home Assistant mechanism.
3. Waited through the slow authentication redirect and confirmed the device registered in the tailnet.
4. Used the Home Assistant VM's real Tailscale address after the callback instead of the generated `localhost` URL.
5. Confirmed a clean private access path before adding Google Calendar and household workflows.

### Result

Home Assistant runs independently from the Docker VM and is reachable privately through Tailscale.

## Google Calendar and Alexa Integration

### Goal

Use an existing family calendar workflow rather than forcing every event to be entered through Home Assistant.

### Challenges

- Calendar data originated in multiple interfaces.
- The system needed to confirm that Alexa-created Google Calendar events propagated through to Home Assistant.
- The built-in calendar presentation was not always visually rich enough for the intended family display.

### Iteration

1. Integrated Google Calendar with Home Assistant.
2. Confirmed events created through Alexa appeared in Google Calendar.
3. Confirmed those events then appeared in Home Assistant.
4. Evaluated default Home Assistant calendar views and custom calendar cards.
5. Selected the Daylight calendar card direction because it better matched the desired ribbon-style household calendar.
6. Continued separating calendar data correctness from dashboard presentation requirements.

### Result

Google Calendar remains the shared schedule source while Home Assistant provides the household display and future automation layer.

## Household Chores

Detailed troubleshooting is documented in the Home Assistant chores project page. The major iteration was moving from simple entity visibility to a complete workflow that creates recurring, assigned Donetick tasks from Home Assistant while hiding controls that bypass the intended recurrence logic.

## Work Week and Shift Calendars

Detailed troubleshooting is documented in the Work Week project page. The major design change was separating accurate timed calendars from simplified all-day display calendars after overnight shifts created misleading multi-day presentation.

## House Brain / Hermes Direction

### Goal

Create a household assistant capable of answering plain-language questions using Home Assistant data.

### Early Design Tension

The assistant could have been embedded directly into Home Assistant or placed on the Debian Docker VM.

### Decision

Keep experimental AI and language-processing code on the Debian Docker VM under a separate project path. Use Home Assistant as the controlled source of household state and actions.

### Reasoning

- Protect Home Assistant stability.
- Allow model and API experimentation without risking core household dashboards.
- Create a clearer future boundary for intent validation and action authorization.
- Start with typed local access before adding the complexity of Alexa voice bridging.

### First Test

The first meaningful test remains a calendar-grounded question such as:

```text
What does tomorrow look like?
```

This is intentionally planned work and should not be represented as implemented yet.

## Overall Pattern

Across these projects, the recurring troubleshooting method has been:

1. Verify the actual running state rather than trusting an example configuration.
2. Isolate the layer where the failure occurs.
3. Test one visible change at a time.
4. Inspect live mounts, entities, playback data, or network paths.
5. Preserve working configurations before making broad changes.
6. Separate accurate backend data from user-facing presentation when their needs conflict.
7. Document incomplete work honestly.

The homelab's strongest portfolio value is not that every component worked immediately. It is that problems were narrowed, tested, corrected, and incorporated into a more maintainable design.
