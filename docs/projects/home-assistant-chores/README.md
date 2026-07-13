# Home Assistant Chores Dashboard

This project documents a household chore system that combines a self-hosted Donetick backend with a Home Assistant dashboard designed for everyday family use.

## Purpose

The project was built to:

- Show active household chores in Home Assistant.
- Create recurring chores without opening Donetick.
- Assign chores to the appropriate household member.
- Display chores completed today.
- Surface completion feedback.
- Keep the interface clean enough for a shared household dashboard.

## Architecture

The system deliberately separates responsibilities:

- **Donetick** is the task, assignment, completion-history, and recurrence backend.
- **Home Assistant** is the household-facing interface.
- **The Donetick Home Assistant integration** exposes chore data and completion state.
- **A Home Assistant REST command and scripts** create recurring chores from dashboard inputs.
- **Template and dashboard logic** present active and completed chores in a simplified format.

This lets Donetick handle task data while Home Assistant remains the normal place the household interacts with the system.

## Current Status

- Donetick is self-hosted on the Debian Docker VM.
- Donetick is integrated with Home Assistant through a custom integration.
- Active chores are visible in Home Assistant.
- Daily recurring chore creation works.
- Weekly recurring chore creation works.
- New chores use rolling recurrence.
- Chores can be assigned to Joe or Samantha from Home Assistant.
- Completed-today summaries are displayed.
- Completion feedback is available in Home Assistant.
- The built-in todo add-item interface is hidden.
- Individual chore names use stronger font weight for readability.

## Rolling Recurrence

The Donetick API payload now explicitly sets:

```json
"isRolling": true
```

Rolling recurrence means the next due date advances from completion rather than remaining tied to a rigid original date. This matches the intended household behavior for chores where completing the task should start the next interval.

The payload also preserves Donetick's recurrence and assignment fields, including frequency type, frequency, due date, assignee, and active state.

Existing chores created with older payload behavior may retain their previous recurrence configuration. Editing and saving an older chore in Donetick can cause Donetick to rewrite it using the current settings, but that should be verified per chore rather than assumed as a universal migration mechanism.

## User Workflow

A household user can:

1. Open the Chores dashboard in Home Assistant.
2. Enter the chore name and optional description.
3. Select the assignee.
4. Choose the recurrence workflow.
5. Submit the chore.
6. See the created chore appear through the Donetick integration after its refresh cycle.
7. Complete the chore and receive visible confirmation.

Home Assistant does not poll Donetick continuously. A newly created chore may take a short period to appear through the custom integration, so the dashboard confirms submission immediately instead of implying the request failed.

## Problems Encountered and Resolutions

### Home Assistant displayed Donetick data but could not create recurring chores

**Problem:** The custom integration provided visibility but did not provide the complete recurring-task creation workflow needed for the dashboard.

**Resolution:** Added a REST command targeting the Donetick API and wrapped it in Home Assistant scripts.

### Recurring chores did not follow the intended completion-based schedule

**Problem:** A recurring task could be created successfully while still behaving like a fixed-date task.

**Resolution:** Added the explicit rolling recurrence field to the request payload and retained the required recurrence metadata.

### Recurrence data required careful request formatting

**Problem:** A task could be created but fail to behave correctly when the payload did not match Donetick's expected structure.

**Resolution:** Tested the request structure, isolated daily and weekly workflows, and kept dashboard-facing inputs simpler than the underlying API payload.

### Empty or stale helper values could create poor entries

**Problem:** Input helpers retain state, which can carry an old chore name or assignee into the next submission.

**Resolution:** The workflow validates inputs, sends a clean notification, and resets dashboard helpers after a successful request.

### Built-in todo controls conflicted with the custom workflow

**Problem:** Home Assistant's todo card displayed an `Add item` interface that bypassed recurrence and assignment logic.

**Resolution:** Hid the built-in add-item controls and directed all new recurring chores through the purpose-built dashboard inputs.

### Removing the `Active` heading also hid chore names

**Problem:** Broad Card Mod selectors targeted multiple heading levels.

**Resolution:** Narrowed the selector to the built-in section heading while preserving each chore name.

### Chore names did not become bold through ordinary card styling

**Problem:** Styling the outer card did not reach the nested todo item text.

**Resolution:** Targeted the todo-list component's nested summary element through Card Mod.

### Home Assistant people are not a native assignment model for todo entities

**Problem:** A standard Home Assistant todo entity does not automatically associate each item with a `person` entity.

**Resolution:** Kept Donetick's user IDs as the authoritative assignee model. Personal Home Assistant todo lists remain separate entities whose ownership is established by configuration and naming.

This distinction matters for Hermes: it can map known list entities to household members, but it must not pretend the underlying todo items contain native person metadata.

## Design Decisions

### Keep Donetick as the source of truth

Home Assistant does not maintain a second independent chore database. This prevents recurrence state, assignment, and completion history from diverging.

### Use rolling behavior where completion should reset the interval

The next due date should reflect when the household actually completed the chore rather than blindly following an old calendar date.

### Keep the dashboard focused

The live interface exposes only the controls used during normal operation. API details and recurrence mechanics stay in documentation.

### Do not confuse visual grouping with data ownership

Displaying a list beneath a person's name does not create an actual backend relationship between that person and the todo items.

## Future Improvements

- Add monthly and quarterly recurring chore workflows.
- Add more polished completion history.
- Improve error feedback when the Donetick API is unavailable.
- Add sanitized dashboard and workflow examples.
- Document Donetick backup and recovery.
- Verify or migrate older fixed-date chores to rolling recurrence.
- Expose stable assignee metadata to Hermes without duplicating Donetick's database.

## Files To Add or Expand

- `dashboard-card.yaml`
- `scripts.yaml`
- `rest-command.yaml`
- `template-sensors.yaml`
- `screenshots/`

## Privacy

Public examples should avoid private household details, API tokens, authentication headers, internal URLs, and sensitive screenshots.

## Skills Demonstrated

- Docker-hosted application integration
- REST API troubleshooting
- Recurrence-model analysis
- Home Assistant scripts and helpers
- Dashboard UX design
- CSS/Card Mod debugging
- State management and input validation
- Data-model boundary analysis
- Separation of backend and presentation responsibilities