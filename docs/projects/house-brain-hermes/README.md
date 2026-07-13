# House Brain / Hermes Assistant

This project documents the planned AI coordination layer for the homelab and Home Assistant environment.

Hermes is not intended to replace Home Assistant. Home Assistant remains the source of truth for calendars, chores, shopping lists, people, devices, entities, and services. Hermes will handle natural-language interpretation, summaries, reasoning, and coordination across that existing data.

## Deployment Model

Hermes will run as a separate Docker application on the Debian VM under:

```text
/srv/docker/hermes
```

Keeping it outside the Home Assistant OS VM provides a cleaner failure boundary. Experimental model, prompt, or application changes can be tested without treating the household automation platform as an AI development environment.

## Current Environment

- Home Assistant OS VM on the household network
- Debian Docker VM hosting the existing container stack
- Private remote access through Tailscale
- Home Assistant used as the household data and automation platform
- Hermes not yet deployed

Internal addresses and credentials are intentionally omitted from this public repository.

## Responsibility Boundary

### Home Assistant

Home Assistant owns:

- Calendar entities and events
- Donetick chore visibility and completion state
- Personal todo lists
- Shopping lists
- People and presence
- Devices and entities
- Automations and service execution
- Dashboard presentation

### Hermes

Hermes will own:

- Natural-language interpretation
- Household summaries
- Context selection
- Reasoning across calendars, chores, lists, and service state
- Coordination of approved higher-level actions
- Future conversational workflows

This boundary avoids creating a second household database inside the assistant.

## Security Model

The initial integration will be deliberately conservative.

- Create a dedicated Home Assistant user for Hermes.
- Use a dedicated long-lived access token.
- Store secrets in an `.env` file outside version control.
- Begin with read-only access.
- Avoid exposing every Home Assistant service directly to the model.
- Build a curated application layer that returns only the household context needed for a request.
- Add write actions individually after their behavior, authorization, and failure handling are understood.

A model should not receive unrestricted control simply because the underlying Home Assistant API can perform an action.

## First Visible Feature: Household Hero Summary

The first major household-facing feature will be a Hermes-generated hero summary at the top of the Home Assistant Home dashboard.

The summary should feel like a calm household note rather than a status report. Its tone should remain cozy, reassuring, and useful throughout the day instead of changing into a purely evening-oriented message.

Useful source context may include:

- Important calendar events
- Work schedules
- Chores due or recently completed
- Shopping or todo reminders
- Meaningful household status
- Time-of-day context

The summary should prioritize what is helpful now and avoid reciting every available entity.

## Update Strategy

The hero summary should not regenerate continuously.

A practical design is to refresh:

- On a reasonable scheduled interval
- When important source data changes
- When a relevant chore is completed
- When a calendar or household state change materially affects the summary

This reduces model usage while allowing the card to react when the information it mentioned is no longer current.

## Initial Milestones

1. Create the Hermes Docker project directory.
2. Add Compose and environment files without committing secrets.
3. Create the dedicated Home Assistant account and token.
4. Verify read-only Home Assistant API access from the Debian VM.
5. Define a narrow household-context endpoint or adapter.
6. Generate a typed summary from calendar data.
7. Publish the resulting text to a Home Assistant entity or helper.
8. Display that entity in the Home dashboard hero card.
9. Add controlled event-driven refreshes.
10. Evaluate higher-level actions only after the read-only path is stable.

## Future Capabilities

- Calendar and day summaries
- Chore and list summaries
- Service-health explanations
- Complex requests that combine interpretation with approved Home Assistant actions
- Proactive announcements through a separately controlled Alexa path
- Voice access through Alexa or Home Assistant Assist
- Safer intent validation and action confirmation

Alexa can continue handling ordinary direct commands such as turning off a lamp. Hermes becomes useful when a request requires context, coordination, or reasoning across multiple systems.

## Known Constraints

- Standard Home Assistant todo items do not contain native person assignment metadata.
- Donetick assignee IDs remain authoritative for chores.
- A dashboard grouping or entity name may provide application context, but it is not the same as backend ownership metadata.
- Proactive announcements require an explicit scheduling or event mechanism; a conversational model does not independently wake up and act.
- Write access must be introduced gradually rather than granted as one broad service tool.

## Privacy

Public documentation should avoid exposing private schedule details, household names beyond those intentionally public, addresses, tokens, API keys, raw Home Assistant data, internal URLs, and unredacted prompt context.

## Skills Demonstrated

- AI application architecture
- Home Assistant API integration planning
- Least-privilege design
- Secret management
- Source-of-truth boundaries
- Event-driven summary design
- Human-centered dashboard content
- Safe separation of reasoning and action execution