# House Brain / Hermes

I plan to use Hermes to answer questions and summarize information from Home Assistant. Home Assistant will keep the household records and run the automations.

## Deployment

Hermes will run as a separate Docker application under `/srv/docker/hermes` on the Debian VM. Keeping it outside Home Assistant OS lets me change the model, prompts, or application without changing Home Assistant OS.

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

## First feature: household summary

The first feature will be a summary at the top of the Home dashboard, using selected events, shifts, chores, lists, and household status.

It should refresh on a schedule and when the information it mentions changes.

## Initial Milestones

1. Deploy the Docker project.
2. Create the dedicated Home Assistant identity and token.
3. Verify read-only API access from Debian.
4. Build a narrow household-context adapter.
5. Generate a typed calendar summary.
6. Publish it to a Home Assistant entity or helper.
7. Display it on the Home dashboard.
8. Add controlled event-driven refreshes.

## Future Direction

Alexa can continue handling direct device commands. Hermes is intended for requests that require context or coordination. Proactive announcements will require an explicit Home Assistant automation, scheduler, or event trigger.

Standard Home Assistant todo items do not contain native person assignment metadata, and Donetick assignee IDs remain authoritative for chores. Hermes must preserve those data boundaries.

Public documentation excludes tokens, private schedules, household data, internal URLs, and unredacted prompt context.
