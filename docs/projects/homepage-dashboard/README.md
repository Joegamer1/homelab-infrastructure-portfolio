# Homepage Dashboard Organization

This project documents the Homepage dashboard used as the homelab launchpad.

Homepage gives the environment a cleaner front door. Instead of remembering individual service URLs, the dashboard organizes services by role and makes the homelab easier to operate day to day.

## Purpose

- Organize services into clear sections.
- Provide quick links to important tools.
- Surface selected service health and status information.
- Make the environment easier to navigate.
- Keep the dashboard useful instead of turning it into a wall of widgets.
- Provide a clean, sanitized overview for the portfolio.

## Current Sections

- Core Infrastructure
- Home
- Docker Services
- Media Stack

The dashboard has also been expanded with selected Home Assistant, Donetick, Docker, and Tailscale information where that data adds operational value.

## Iterative Build Process

Homepage was not deployed as a finished dashboard in one pass. The work moved through several stages:

1. Deploy the container and confirm the web interface was reachable.
2. Resolve host validation errors that prevented normal access.
3. Identify the actual configuration bind mount used by the container.
4. Build basic service links.
5. Reorganize those links into functional groups.
6. Add Docker-backed service status.
7. Add infrastructure and household services as the homelab expanded.
8. Remove or avoid widgets that added clutter without helping operations.
9. Sanitize the layout so it could also serve as a portfolio overview.

## Problems Encountered and Resolutions

### `Host validation failed`

**Problem:** The Homepage container was running, but requests were rejected because the hostname used to access it was not allowed.

**Investigation:**

- Confirmed the container was healthy and the port was listening.
- Distinguished an application-level validation error from a Docker networking failure.
- Reviewed the expected host allowlist behavior.

**Resolution:** Added the appropriate `HOMEPAGE_ALLOWED_HOSTS` configuration and recreated the container.

**Result:** Homepage became accessible through the intended local and private access paths.

**Lesson:** A reachable container is not necessarily a correctly configured application. Error text at the browser layer can point to application policy rather than port or firewall problems.

### Configuration location was not obvious

**Problem:** Homepage has several YAML files, and the important question was not only what to edit but which host path was actually mounted into `/app/config`.

**Investigation:** Inspected the running container mounts instead of relying on assumed directory conventions.

**Resolution:** Verified the active host configuration path and documented the files present there, including `services.yaml`, `widgets.yaml`, `settings.yaml`, `docker.yaml`, `custom.css`, and related files.

**Lesson:** Inspect the running container before editing. A correct file in the wrong directory is still a broken configuration.

### The first dashboard was technically correct but poorly organized

**Problem:** A flat collection of links made services harder to scan as the environment grew.

**Resolution:** Grouped services by operational role instead of by deployment order.

Examples:

- Infrastructure and management services together.
- Household services together.
- Media acquisition and playback services together.
- Docker-specific visibility separated from user-facing applications.

**Result:** The dashboard became a map of the environment rather than a bookmark list.

### Too much information reduced usefulness

**Problem:** It was tempting to add every available widget and metric. That made the dashboard busier without necessarily improving decisions.

**Resolution:** Kept widgets that answer practical questions such as whether a service is running, how to reach it, or whether a core dependency is healthy. Avoided turning Homepage into a replacement for dedicated monitoring.

### Service names and container reality did not always match assumptions

**Problem:** Some service names changed or differed from generic examples. For example, the media request frontend container is `seerr`, not an assumed `overseerr` name.

**Resolution:** Used the actual running container names and inspected Docker state before wiring widgets or operational commands.

**Lesson:** Portfolio documentation and dashboards should describe the environment that exists, not the environment a tutorial expected.

## Design Decisions

### Homepage is a launchpad, not the monitoring system

Uptime Kuma is responsible for dedicated availability checks. Homepage provides quick visibility and navigation. Keeping those roles separate prevents the front page from becoming overloaded.

### Group by function

Services are organized around what they do for the environment. This makes the dashboard understandable to someone who did not build the stack.

### Keep household services visible

Home Assistant and Donetick are included because the homelab is not only a media server or container exercise. They represent the household operations direction of the platform.

## Files To Add or Sanitize

- `services.yaml`
- `widgets.yaml`
- `bookmarks.yaml`
- `settings.yaml`
- `docker.yaml`
- `screenshots/`

## Privacy

Public examples should use sanitized hostnames, placeholder URLs, generic service addresses, and screenshots that do not expose private network details, tokens, household data, or Tailscale device information.

## Skills Demonstrated

- Docker deployment troubleshooting
- Bind-mount inspection
- Application allowlist configuration
- YAML configuration management
- Service inventory design
- Operational dashboard UX
- Separation of monitoring and navigation responsibilities
