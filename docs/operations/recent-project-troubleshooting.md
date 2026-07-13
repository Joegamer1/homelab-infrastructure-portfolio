# Recent Project Troubleshooting Log

This document summarizes significant implementation problems encountered during recent Home Assistant household-operations work and the steps used to resolve them.

The purpose is to preserve the reasoning behind the final design. A finished dashboard rarely shows the failed assumptions, rendering limitations, integration gaps, and iterative testing required to make it reliable.

## Work Schedule and Calendar Display

### Overnight shifts appeared on two days

**Problem:** A timed shift that started in the afternoon and ended after midnight was correctly stored across two dates, but calendar views made it appear as two work days.

**Investigation:**

1. Confirmed that the detailed event must end on the following date.
2. Compared weekly schedule requirements with family calendar presentation requirements.
3. Verified that all-day calendar end dates are exclusive.
4. Determined that changing the detailed event would damage schedule accuracy.

**Resolution:** Created separate detailed and display calendar entities. The detailed calendar stores exact times; the display calendar receives one all-day entry on the date the shift starts.

**Lesson:** Accurate source data and simplified presentation may require separate models.

### Browser Mod popup did not open

**Problem:** The Add Shift button appeared to do nothing.

**Investigation:** The YAML called a Browser Mod popup service, but Browser Mod had not been added as a Home Assistant integration.

**Resolution:** Installed Browser Mod as an integration and retested the popup independently from the shift script.

**Lesson:** A frontend resource and an integration-provided service are different layers. Validate that the service exists before debugging the form content.

### Add Shift popup worked but no event was created

**Problem:** After function changes, the popup opened but the shift was not added and no useful confirmation appeared.

**Investigation:** The workflow was split into layers:

1. Popup invocation
2. Helper state updates
3. Script execution
4. Overnight date calculation
5. Detailed calendar service call
6. Display calendar service call

**Resolution:** Restored the complete script path and verified both calendar events after submission.

**Lesson:** For multi-layer Home Assistant workflows, test the user interface, helpers, script, and integration calls separately.

### Shift times defaulted to the current time

**Problem:** Opening the shift form populated time helpers with the current clock time, which was rarely the intended starting value.

**Resolution:** Changed the default to `00:00` for predictable new entries.

### Date picker month label became stale

**Problem:** Navigating from one month to another did not always update the displayed month label. A selected date could then appear inconsistent or wrong.

**Investigation:** The problem occurred across multiple versions of the picker while the downstream shift script behaved correctly with valid dates.

**Current status:** Treated as a frontend component defect rather than a calendar-service defect. The workflow remains usable, but the picker label is not fully trustworthy after month navigation.

### Per-shift deletion remains incomplete

The next planned improvement is a delete control beside each displayed shift. The implementation must identify both the detailed event and its corresponding all-day display event. This remains planned rather than completed.

## Work Week Dashboard Controls

Default editing and refresh buttons visually competed with the schedule. They were replaced with the slimmer action pattern used elsewhere in the family dashboard.

The calendar editing path and refresh entity targets were preserved rather than replaced with placeholders during styling changes.

## Donetick Recurring Chore Creation

### Integration visibility did not provide the full creation workflow

**Problem:** The Donetick integration exposed chore data but did not provide the required assigned recurring-task creation flow.

**Resolution:** Added a Home Assistant REST command and script-based workflow using dashboard input helpers.

### Chores were recurring but not rolling

**Problem:** A chore could be created successfully while its next due date remained tied to a fixed schedule rather than advancing from completion.

**Resolution:** Added the explicit Donetick payload field:

```json
"isRolling": true
```

**Result:** New chores created through the Home Assistant workflow use completion-based rolling recurrence.

**Caution:** Existing chores may retain older behavior. Editing an old chore can cause Donetick to rewrite its current settings, but older chores should be verified individually rather than assumed migrated.

### Chore description and payload edits

The REST payload preserves the chore name, HTML description, recurrence type, frequency, due date, assignee, rolling state, and active state. Small edits to the script affect newly created chores; they do not automatically rewrite every existing Donetick record.

### Integration refresh delay

A newly created chore may not appear in Home Assistant immediately because the custom integration refreshes on its own interval.

**Resolution:** Keep immediate submission feedback in Home Assistant and allow the integration time to refresh before treating the request as failed.

### Helper state persisted between entries

Home Assistant input helpers retained old names and assignees. The scripts now reset relevant helpers after successful submission.

### Built-in todo controls bypassed recurrence logic

The default todo `Add item` interface was hidden so new chores consistently use the Donetick-backed workflow.

### Card Mod styling hid the wrong elements

A broad selector intended to remove the `Active` heading also hid chore names. The styling was narrowed to the correct rendered heading, and the nested summary element was targeted separately to bold each chore name.

### Person assignment is not native to todo entities

Standard Home Assistant todo items do not contain a native relationship to `person` entities. Donetick user IDs remain authoritative for chore assignment. Personal todo lists are associated with people by configuration and naming, not item-level metadata.

This matters for Hermes: it can maintain an application mapping but should not claim the underlying entity provides ownership data that is not present.

## Family Dashboard and Mobile Layout

### Landing page looked like a configuration screen

The original page was reworked into a dedicated household entry point with clearer hierarchy, large destination cards, and less instructional text.

### Navigation paths were guessed incorrectly

Visible dashboard titles did not always match their actual Lovelace routes. Each destination was opened directly and its working path was used in the navigation action.

### Edge-to-edge mobile calendar experiments regressed landscape mode

**Problem:** Attempts to remove phone side margins could appear to work during refresh but then shrink back, shift the landscape card left, or interfere with surrounding layout behavior.

**Resolution:** Reverted to the last stable configuration rather than keeping brittle hard-coded offsets.

**Lesson:** A mobile change is not successful when it improves portrait mode but breaks landscape or kiosk navigation.

### Kiosk mode was affected by unrelated layout changes

Margin and shell styling became entangled with header and sidebar behavior.

**Resolution:** Kept kiosk mode explicit and separate from card sizing experiments.

### Hard-coded custom-card controls resisted styling

Attempts to replace the calendar `+` control with `Add Event` did not work because the control is rendered internally by the custom card.

**Resolution:** Kept the stable built-in control and avoided fragile DOM manipulation.

### Theme appeared ineffective

The theme file existed but was not loaded and selected. The themes directory was added to Home Assistant configuration, Home Assistant was reloaded or restarted, and the theme was explicitly applied.

## Calendar Synchronization Delay

An event added to Google Calendar from an email did not appear in Home Assistant immediately but arrived after the integration refreshed.

**Lesson:** Before changing a working calendar integration, allow for polling delay and verify the source calendar.

## Hermes Architecture Decisions

### Do not make Hermes a second source of truth

Home Assistant remains authoritative for calendars, chores, lists, people, devices, and services. Hermes will interpret, summarize, and coordinate across that data.

### Keep Hermes outside Home Assistant OS

Hermes will run as a separate Docker application on the Debian VM. This isolates AI experimentation from the household automation platform.

### Start read-only

The first integration phase will use a dedicated Home Assistant user and long-lived token. Secrets will be kept in an untracked `.env` file.

### Do not expose every service to the model

A curated adapter will provide only the data and approved actions required for supported workflows. Broad API capability is not the same as safe model capability.

### First feature: household hero summary

The first visible Hermes feature will be a cozy, reassuring summary at the top of the Home dashboard. It should refresh on a reasonable schedule and after important source changes, such as completion of a chore it mentioned, rather than regenerating continuously.

## Overall Engineering Lessons

- Real household feedback revealed requirements that were not obvious during implementation.
- Correct source data and good presentation sometimes require separate entities.
- Multi-layer Home Assistant failures should be isolated at the popup, helper, script, and integration levels.
- A custom component can impose upstream defects or customization limits that YAML cannot safely fix.
- Responsive changes require portrait and landscape regression testing.
- Integrations often need API glue to support a complete user workflow.
- Existing backend records do not automatically inherit changes made to creation scripts.
- Visual grouping should not be mistaken for authoritative assignment metadata.
- AI integrations should begin with narrow read access and explicit responsibility boundaries.
- Public documentation should record failed assumptions, reversions, and incomplete work rather than only final configurations.