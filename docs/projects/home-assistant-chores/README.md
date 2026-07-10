# Home Assistant Chores Dashboard

This project documents a household chore system that combines a self-hosted Donetick backend with a Home Assistant dashboard designed for everyday family use.

## Purpose

The project was built to:

- Show active household chores in Home Assistant.
- Create recurring chores without opening the Donetick application.
- Assign chores to the appropriate household member.
- Display chores completed today.
- Surface completion notifications.
- Keep the interface clean enough to use on a shared household dashboard.

## Architecture

The system deliberately separates responsibilities:

- **Donetick** is the task and recurrence backend.
- **Home Assistant** is the household-facing interface.
- **The Donetick Home Assistant integration** exposes chore data and completion state.
- **A Home Assistant REST command and scripts** create recurring chores from dashboard inputs.
- **Template and dashboard logic** present active and completed chores in a simplified format.

This lets Donetick handle task data while Home Assistant remains the single place the household interacts with the system.

## Current Status

- Donetick is self-hosted on the Debian Docker VM.
- Donetick is integrated with Home Assistant through a custom integration.
- Active chores are visible in Home Assistant.
- Daily recurring chore creation works.
- Weekly recurring chore creation works.
- Chores can be assigned to Joe or Samantha from Home Assistant.
- Completed-today summaries are displayed.
- Completion notifications are available in Home Assistant.
- The built-in todo add-item interface is hidden so users use the intended recurring-chore workflow.
- Individual chore names are displayed with stronger font weight for better readability.

## User Workflow

A household user can:

1. Open the Chores dashboard in Home Assistant.
2. Enter the chore name.
3. Select the assignee.
4. Choose the recurrence workflow.
5. Submit the chore.
6. See the created chore appear through the Donetick integration.
7. Complete the chore and receive visible confirmation.

The goal is not to reproduce every Donetick administration feature in Home Assistant. It is to expose the small set of actions needed during normal household use.

## Problems Encountered and Resolutions

### Home Assistant displayed Donetick data but could not create recurring chores

**Problem:** The custom integration provided visibility but did not provide the complete recurring-task creation workflow needed for the dashboard.

**Resolution:** Added a REST command targeting the Donetick API and wrapped it in Home Assistant scripts that collect helper values and construct the required request.

### Recurrence data required careful request formatting

**Problem:** A task could be created but fail to behave as a recurring daily or weekly chore when the payload did not match Donetick's expected structure.

**Resolution:** Tested the request structure, isolated daily and weekly workflows, and kept the dashboard-facing inputs simpler than the underlying API payload.

### Empty or stale helper values could create poor entries

**Problem:** Input helpers retain state, which can accidentally carry an old chore name or assignee into the next submission.

**Resolution:** The creation workflow validates the relevant inputs, sends a clean notification, and resets the dashboard helpers after a successful request.

### Built-in todo controls conflicted with the custom workflow

**Problem:** Home Assistant's todo card displayed an `Add item` interface that bypassed recurrence and assignment logic.

**Resolution:** Hid the built-in add-item controls and directed all new recurring chores through the purpose-built dashboard inputs.

### Removing the `Active` heading also hid chore names

**Problem:** Broad Card Mod selectors targeted multiple heading levels. Hiding `h3` removed the chore names along with the unwanted section label.

**Resolution:** Narrowed the CSS selector to `ha-card h2`, which hides the built-in `Active` heading while preserving each chore name.

This was a useful reminder that dashboard CSS should be tested against the rendered component structure rather than applied broadly.

### Chore names did not become bold through ordinary card styling

**Problem:** Styling the outer card or common heading elements did not affect the rendered chore text because the todo-list component uses nested internal elements.

**Resolution:** Targeted the todo-list component's nested structure through Card Mod and applied font weight to the actual item summary element. The final selector was kept narrow so completion controls and surrounding labels were not unintentionally changed.

### Home Assistant people are not a native assignment model for todo entities

**Problem:** A standard Home Assistant todo entity does not automatically associate each list item with a `person` entity. Separate personal todo lists can be presented next to each person's dashboard content, but this is organizational convention rather than item-level ownership enforced by Home Assistant.

**Resolution:** Kept Donetick's user IDs as the authoritative assignee model for chores. Personal Home Assistant todo lists remain separate list entities rather than being treated as a substitute for Donetick assignment.

This distinction matters for future automation: House Brain or Hermes can interpret which list belongs to which person, but it should not assume Home Assistant todo items contain native person metadata that is not actually present.

### Successful actions needed clear feedback

**Problem:** A submitted chore could take a moment to appear through the integration, making the interface feel unresponsive.

**Resolution:** Added clean persistent notifications confirming the request rather than adding explanatory text or status clutter to the dashboard itself.

### The dashboard needed to remain useful for non-admin users

**Problem:** The household interface needed to be understandable without exposing backend configuration or requiring users to manage Donetick directly.

**Resolution:** Reduced the visible workflow to chore name, assignee, recurrence action, current chores, and completion summaries.

## Design Decisions

### Keep Donetick as the source of truth

Home Assistant does not maintain a second independent chore database. This prevents recurrence state, assignment, and completion history from diverging.

### Keep the dashboard focused

The dashboard avoids instructional markdown and backend terminology. It exposes only controls used during normal operation.

### Use separate workflows for recurrence types

Daily and weekly creation are separate enough to remain understandable and testable. Monthly recurrence can be added later without destabilizing the working flows.

### Do not confuse visual grouping with data ownership

Displaying a list beneath a person's name does not create an actual relationship between the todo items and that Home Assistant person. Integrations and future agents should use the real backend assignment data when assignment matters.

## Future Improvements

- Add a monthly recurring chore workflow.
- Add more polished completed-chore history.
- Improve error feedback when the Donetick API is unavailable.
- Add sanitized dashboard and workflow examples.
- Document backup and recovery of Donetick data.
- Expose stable assignee metadata to the future House Brain layer without duplicating Donetick's task database.

## Files To Add or Expand

- `dashboard-card.yaml`
- `scripts.yaml`
- `rest-command.yaml`
- `template-sensors.yaml`
- `screenshots/`

## Privacy

Public examples should avoid private household details, API tokens, authentication headers, internal URLs that are not intended to be public, and sensitive screenshots.

## Skills Demonstrated

- Docker-hosted application integration
- REST API troubleshooting
- Home Assistant scripts and helpers
- Dashboard UX design
- CSS/Card Mod debugging
- State management and input validation
- Data-model boundary analysis
- Separation of backend and presentation responsibilities