# House Brain / Hermes

Hermes is the planned AI coordination layer for the Home Assistant environment. It will interpret requests, select context, create summaries, and coordinate approved workflows without replacing Home Assistant as the source of truth.

## Deployment

Hermes will run as a separate Docker application under `/srv/docker/hermes` on the Debian VM. Keeping it outside Home Assistant OS creates a cleaner failure boundary for model, prompt, and application changes.

Planning examples:

- [`docker-compose.yaml`](docker-compose.yaml)
- [`.env.example`](.env.example)

The Compose image remains a placeholder until an implementation is selected and tested.

## Responsibility Boundary

### Home Assistant owns

- Calendars, chores, lists, people, devices, and entities
- Automations and service execution
- Dashboard presentation

### Hermes handles

- Natural-language interpretation
- Context selection and household summaries
- Reasoning across Home Assistant domains
- Coordination of individually approved higher-level actions

Hermes will not maintain a separate household database.

## Security Model

The first phase is read-only:

- Dedicated Home Assistant account and long-lived token
- Secrets stored in an untracked `.env` file
- Curated context instead of unrestricted API exposure
- No broad Home Assistant service catalog given to the model
- Write actions added individually after authorization and failure behavior are defined

## First Feature: Hero Summary

The first household-facing feature will be a calm summary at the top of the Home dashboard. It may use important events, work schedules, chores, lists, and meaningful household status.

The summary should update on a reasonable interval and after material source changes, not continuously.

## Initial Milestones

1. Deploy the Docker project.
2. Create the dedicated Home Assistant identity and token.
3. Verify read-only API access from Debian.
4. Build a narrow household-context adapter.
5. Generate a typed calendar summary.
6. publish it to a Home Assistant entity or helper.
7. Display it on the Home dashboard.
8. Add controlled event-driven refreshes.

## Future Direction

Alexa can continue handling direct device commands. Hermes is intended for requests that require context or coordination. Proactive announcements will require an explicit Home Assistant automation, scheduler, or event trigger.

Standard Home Assistant todo items do not contain native person assignment metadata, and Donetick assignee IDs remain authoritative for chores. Hermes must preserve those data boundaries.

Public documentation excludes tokens, private schedules, household data, internal URLs, and unredacted prompt context.