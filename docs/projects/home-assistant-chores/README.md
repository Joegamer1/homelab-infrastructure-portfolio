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

New daily and weekly chores created through the Home Assistant buttons explicitly send both the initial due date and rolling state:

```json
"dueDate": "2026-07-20T21:00:00-04:00",
"isRolling": true
```

The next due date advances from the actual completion date. A late daily chore becomes due the following day after one completion. A late weekly chore becomes due one week after completion. Missed occurrences do not require repeated catch-up clicks.

This behavior was validated through both Home Assistant and the Donetick API.

## Troubleshooting and Validation

The initial change appeared correct in YAML but did not change existing chores. API inspection showed that older records still stored `isRolling: false`, so those chores had to be recreated.

A second issue appeared after changing the payload field from `dueDate` to `nextDueDate`. Donetick accepted `isRolling: true`, but stored `nextDueDate: null`. Those chores appeared in Donetick without a due date and did not populate correctly in the Home Assistant chore list.

A direct API test isolated the behavior from Home Assistant. The installed Donetick API accepted `dueDate`, not `nextDueDate`, for chore creation. Restoring `dueDate` while keeping `isRolling: true` produced a valid due date and the expected rolling behavior.

The final test process was:

1. Create a new daily or weekly chore through the Home Assistant dashboard.
2. Query the Donetick API and confirm a populated `nextDueDate` plus `isRolling: true`.
3. Complete the chore once.
4. Force a Home Assistant entity update during testing.
5. Confirm that daily chores moved to tomorrow and weekly chores moved to the following week.

Existing records do not inherit changes made to Home Assistant creation scripts. Chores created before this fix were recreated through the dashboard.

## Assignment Model

Donetick user IDs are authoritative. Home Assistant person entities and per-person dashboard grouping do not create native ownership metadata on todo items.

This matters for Hermes: it may map known list entities and Donetick assignees to household members, but it must not invent backend relationships that do not exist.

## Operational Notes

New chores may not appear immediately because the integration refreshes on its own interval. During testing, `homeassistant.update_entity` was used to refresh the relevant todo entity and completed-today sensor without waiting for the normal polling cycle.

The dashboard confirms submission separately so a short synchronization delay does not look like a failed request.

## Remaining Work

- Add monthly and quarterly recurrence workflows.
- Improve API failure feedback and completion history.
- Document Donetick backup and recovery.
- Expose stable assignment context to Hermes without duplicating Donetick data.

Public examples exclude tokens, authentication headers, internal URLs, and private household details.
