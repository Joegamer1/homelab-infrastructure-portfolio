# Project Evolution

This homelab did not begin as a complete household operations platform. It grew through a series of practical needs, technical constraints, failed assumptions, and user feedback.

The value of the project is not only the final collection of services. It is the way the architecture changed as the environment became more important and more widely used.

## Phase 1: Build a Stable Virtualization Foundation

The first priority was establishing a dependable Proxmox host on the Dell Precision T7820.

The early architectural lesson was that the hypervisor should remain focused on virtualization. Application services belong inside dedicated workloads where they can be maintained, restarted, backed up, and troubleshot without turning the Proxmox host into a general-purpose application server.

This led to the Debian Docker VM becoming the main container host.

## Phase 2: Move From Individual Containers to an Operated Platform

The first Docker services solved individual problems:

- Portainer for container visibility
- Pi-hole for DNS and filtering
- Nginx Proxy Manager for reverse-proxy management
- Uptime Kuma for availability checks
- Homepage for navigation

As the number of services grew, the challenge changed. Deploying containers was no longer the main problem. The new problem was operating an environment whose data lived across bind mounts, named volumes, multiple URLs, and different service roles.

This led to:

- Inspecting live container mounts instead of relying on assumed paths
- Organizing Homepage by service function
- Separating monitoring from navigation
- Creating backups for configuration-heavy services
- Documenting actual container names and storage locations

The environment became a platform that needed operational consistency, not just more applications.

## Phase 3: Add Media Services and Hardware Acceleration

The media stack introduced cross-layer troubleshooting.

Plex playback depended on more than the Plex container itself. Performance and compatibility were influenced by:

- Client capabilities
- Video codec behavior
- Audio codec behavior
- Network path
- Public versus Tailscale access
- GPU passthrough
- Guest drivers
- Container runtime configuration

This phase reinforced the need to diagnose the whole service path instead of treating an application symptom as an application-only problem.

The GPU passthrough work also demonstrated that one feature may require correct configuration across the hypervisor, VM, operating system, container runtime, and application.

## Phase 4: Establish Private Remote Administration

Tailscale changed the remote-access model.

Instead of forwarding management ports for convenience, administrative services remained private and were accessed through the tailnet. Plex received the specific public exposure needed for its user-facing purpose, while Proxmox, Portainer, Pi-hole, Home Assistant administration, and the Arr applications remained private.

This created a clearer distinction between:

- Services intended for public users
- Services intended for household users
- Administrative systems intended only for trusted access

## Phase 5: Treat Home Assistant as a Separate Platform

Home Assistant began as another useful service, but its role expanded quickly.

It became responsible for:

- Family calendar visibility
- Work schedule entry and display
- Chore creation and completion visibility
- Household navigation
- Shared dashboards

At that point, Home Assistant was no longer just another containerized application. It had become the household operations layer.

Running Home Assistant OS in a dedicated VM reduced the chance that changes to the general Docker stack or media services would affect family-facing workflows.

This was an architectural response to operational importance: systems used by other people deserve stronger isolation than experiments used only by the administrator.

## Phase 6: Design Around Real Household Use

The most important design changes came after the system was used by people other than the person who built it.

### Work schedule feedback

A technically correct overnight calendar event appeared on two dates. From a data perspective, the event was accurate. From the household user's perspective, it looked as though the person worked both days.

The requirement was refined through feedback:

- Preserve exact shift times for schedule logic.
- Show one simple work indicator on the date the shift begins.
- Do not create a second display entry because the shift ended after midnight.

The solution was to separate detailed calendar entities from simplified display calendar entities.

### Chore workflow feedback

The initial Home Assistant integration made Donetick data visible, but visibility alone was not a complete household workflow.

The dashboard needed to support:

- Recurring chore creation
- Assignment
- Predictable input reset
- Visible completion feedback
- Protection against bypassing recurrence logic

That led to REST-based task creation, dedicated scripts, helper reset behavior, targeted styling, and removal of the built-in todo creation path.

### Dashboard feedback

Default Home Assistant controls were functional but not always appropriate for a family-facing interface.

The dashboard evolved toward:

- Clear primary destinations
- Smaller secondary actions
- Less instructional text
- Consistent navigation
- Visual hierarchy based on household tasks rather than Home Assistant internals

## Phase 7: Build Documentation Around Decisions

The repository originally described what was installed. It has since evolved to document:

- What problem each component solved
- What was tried first
- Where the initial approach failed
- How the failing layer was isolated
- What solution was selected
- How the result was validated
- What remains incomplete

This change matters because a list of technologies demonstrates exposure. A decision and troubleshooting narrative demonstrates engineering judgment.

## Validation Practices

Across the environment, changes are validated through the tool or layer closest to the behavior being tested.

Examples include:

- Checking `docker ps` and container health after recreations
- Inspecting Docker mounts before editing or backing up data
- Testing DNS records directly rather than relying only on browser access
- Confirming Tailscale machine registration and correct host addresses
- Reading Plex playback details for video and audio behavior separately
- Confirming GPU binding at the Proxmox host and recognition inside the guest and container
- Testing same-day and overnight calendar cases
- Creating daily and weekly chores and verifying assignment and recurrence behavior
- Using visible CSS tests before narrowing Card Mod selectors
- Inspecting backup contents and archive sizes instead of trusting exit status alone

## Known Limitations

The environment is functional, but it is not represented as complete in every area.

Current limitations include:

- Backup restore testing is still required.
- Off-host backup handling should become more formal.
- Some Docker storage paths reflect historical deployments rather than one perfect convention.
- The family dashboard is still being visually refined.
- Monthly recurring chore creation is not yet implemented.
- The simplified work display calendar intentionally omits exact shift times.
- Monitoring is useful availability monitoring, not full observability.
- Current screenshots will be added after sensitive information is sanitized.

## What I Would Do Differently

If rebuilding the environment from the beginning, I would:

- Establish `/srv/docker/<service>` as the storage convention before deploying applications.
- Define container naming standards early.
- Inventory bind mounts and named volumes before writing backup logic.
- Capture sanitized screenshots at major milestones.
- Separate detailed calendar data from family-facing display entities earlier.
- Design Home Assistant views around household tasks before adding default cards.
- Record validation steps at the time of implementation instead of reconstructing them later.

These are not presented as failures of the project. They are lessons produced by operating a system long enough to understand where consistency matters.

## Current Identity of the Project

The environment began as a traditional homelab and media-server build.

It is now a household infrastructure and operations platform built around:

- Virtualization
- Linux and Docker administration
- Private remote access
- Monitoring and DNS
- Media services
- Home automation
- Family scheduling
- Household task workflows
- Backup and recovery planning
- Iterative operational documentation

That progression is the central narrative of the portfolio.
