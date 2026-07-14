# Home Assistant Chores

This project combines self-hosted Donetick with a Home Assistant interface for creating, assigning, displaying, and completing recurring household chores.

## Responsibility Split

- **Donetick** owns chores, recurrence, assignees, and completion history.
- **Home Assistant** provides the household interface.
- **The custom integration** exposes Donetick state.
- **REST commands and scripts** create recurring chores from dashboard inputs.

Home Assistant does not maintain a second chore database.

## Current Workflow

Daily and weekly recurring chores can be created for Joe or Samantha. The dashboard validates inputs, sends the Donetick request, confirms submission, and resets its helpers.

The built-in todo add-item control is hidden because it bypasses recurrence and assignment logic. Chore names are bolded through Card Mod selectors that target the nested todo item summary.

Sanitized examples:

- [`rest-command.yaml`](rest-command.yaml)
- [`scripts.yaml`](scripts.yaml)
- [`dashboard-card.yaml`](dashboard-card.yaml)

## Rolling Recurrence

Requests explicitly set:

```json
"isRolling": true
```

The next due date therefore advances from completion rather than remaining tied to the original date. Older chores may retain their previous behavior and should be verified individually.

## Assignment Model

Donetick user IDs are authoritative. Home Assistant person entities and per-person dashboard grouping do not create native ownership metadata on todo items.

This matters for Hermes: it may map known list entities and Donetick assignees to household members, but it must not invent backend relationships that do not exist.

## Operational Notes

New chores may not appear immediately because the integration refreshes on its own interval. The dashboard confirms submission separately so a short synchronization delay does not look like a failed request.

## Remaining Work

- Add monthly and quarterly recurrence workflows.
- Improve API failure feedback and completion history.
- Verify or migrate older fixed-date chores.
- Document Donetick backup and recovery.
- Expose stable assignment context to Hermes without duplicating Donetick data.

Public examples exclude tokens, authentication headers, internal URLs, and private household details.