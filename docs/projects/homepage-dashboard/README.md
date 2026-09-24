# Homepage Dashboard

I use Homepage to open services and check a few status widgets. Uptime Kuma handles availability monitoring.

## Design

Services are grouped by function:

- Core infrastructure
- Household services
- Docker services
- Media stack

Each group puts related service links together.

## Troubleshooting

### Host validation

The container was running but rejected requests because the access hostname was not allowed. Adding the correct `HOMEPAGE_ALLOWED_HOSTS` setting and recreating the container resolved the issue.

The rejection came from Homepage host validation; Docker networking and the firewall were working.

### Configuration path

The active host path mounted to `/app/config` was verified from the running container before editing. Homepage uses several files, including `services.yaml`, `widgets.yaml`, `settings.yaml`, `docker.yaml`, and `custom.css`.

A correct file in the wrong host directory has no effect.

### Dashboard clutter

The first layout was a flat list of links. Grouping by operational role made it easier to scan. I removed widgets I wasn't using.

### Naming accuracy

Dashboard configuration uses actual container names and live Docker state. For example, the media request service is `seerr`, not an assumed generic name.

## Planned Additions

Sanitized examples still need to be added for the active Homepage YAML files and screenshots.

Public examples will use placeholder URLs and exclude tokens, internal addresses, household data, and Tailscale device information.
