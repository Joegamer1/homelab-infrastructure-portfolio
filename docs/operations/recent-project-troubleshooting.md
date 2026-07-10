# Recent Project Troubleshooting Log

This document summarizes significant implementation problems encountered during recent Home Assistant household-operations work and the steps used to resolve them.

The purpose is to preserve the reasoning behind the final design. A finished dashboard rarely shows the failed assumptions, rendering limitations, integration gaps, and iterative testing that were required to make it reliable.

## Work Schedule and Calendar Display

### Requirement

Display accurate work shifts, including overnight shifts, while keeping the household calendar visually simple.

### Initial Problem

A timed shift that started in the afternoon and ended after midnight was correctly stored as a multi-date event. However, calendar views visually extended the event into the following day. This made a single overnight shift look like two work days.

### Investigation

1. Confirmed that the event end date must be the following day for an overnight shift.
2. Compared the needs of the weekly schedule card with the needs of the family calendar.
3. Tested the expected behavior of all-day events.
4. Identified that an all-day event uses an exclusive end date.
5. Determined that changing the original timed event would damage the accuracy needed by schedule sensors and cards.

### Resolution

Created separate detailed and display calendar entities.

- The detailed calendar stores the real start and end times.
- The display calendar receives a one-day all-day event on the date the shift starts.
- No display entry is created on the date the overnight shift ends.

### Result

The schedule logic remains accurate, while the family calendar shows one clean day-level work indicator.

### Lesson

When the same source data must support both calculations and simplified presentation, separate storage and display models can be cleaner than increasingly complex formatting logic.

## Shift Notes Handling

### Requirement

Allow optional notes without adding empty or invalid descriptions to calendar events.

### Problem

Home Assistant helpers may return blank strings or states such as `unknown` and `unavailable`. Passing these directly to the calendar service creates poor descriptions.

### Resolution

Added conditional logic that only includes the event description when a meaningful note is present.

### Result

Normal shifts remain clean, while optional context is preserved when entered.

## Work Week Dashboard Controls

### Problem

Default action buttons for editing and refreshing the schedule were too large and visually competed with the schedule itself.

### Resolution

Reworked the lower actions into the slimmer button pattern used by other dashboard tabs while preserving the existing navigation paths and entity targets.

### Lesson

Functional consistency includes visual weight. Primary navigation and secondary actions should not look equally important.

## Donetick Recurring Chore Creation

### Requirement

Create assigned, recurring chores directly from Home Assistant.

### Initial Problem

The Donetick integration exposed chore data but did not provide the complete dashboard-driven recurrence creation workflow.

### Investigation

1. Verified that Donetick was reachable from Home Assistant.
2. Tested task creation through the Donetick API.
3. Identified the payload fields needed for recurrence and assignment.
4. Moved API details behind Home Assistant scripts so household users would not interact with raw requests.
5. Separated daily and weekly workflows to make behavior easier to validate.

### Resolution

Implemented a Home Assistant REST command and script-based workflow using dashboard input helpers.

### Result

Users can create daily and weekly chores, assign them, and see them through the Donetick integration without leaving Home Assistant.

### Lesson

An integration can provide useful entity visibility without covering every operational workflow. A small API bridge can fill that gap while preserving one source of truth.

## Chore Dashboard Input State

### Problem

Home Assistant input helpers retain their values. Without cleanup, an old chore name or assignee could remain selected for the next entry.

### Resolution

Added post-submission helper resets and visible confirmation after successful requests.

### Result

Each new chore entry begins from a predictable state.

## Todo Card Header Styling

### Requirement

Hide the built-in `Active` label without hiding the chore names.

### Initial Problem

A broad Card Mod CSS selector hid both the unwanted header and the individual chore titles.

### Investigation

1. Confirmed Card Mod was functioning with a visible border test.
2. Narrowed the problem to heading-level selectors.
3. Determined that the section label was rendered as `h2` while chore names used a different heading level.

### Resolution

Targeted only:

```css
ha-card h2
```

and avoided broad `h3` or generic header selectors.

### Result

The `Active` label is hidden while chore names remain visible.

### Lesson

When modifying frontend components, first prove that the styling mechanism works, then narrow selectors against the actual rendered structure.

## Built-In Todo Add Item Interface

### Problem

The default todo add-item controls allowed users to create entries that bypassed the custom assignment and recurrence workflow.

### Resolution

Hid the built-in add-item interface and made the custom chore creation controls the only normal entry path.

### Result

New chores consistently use the intended Donetick-backed workflow.

## Family Dashboard Visual Design

### Initial Problem

The Home Assistant landing page was functional but looked like a collection of default controls rather than the main page of a family application.

### Resolution

Reworked the page around stronger visual hierarchy, full-width section usage, clear destination cards, and consistent household-focused navigation.

### Additional Refinement

Removed unnecessary explanatory text from live cards and kept technical documentation in the repository instead.

### Lesson

Operational dashboards are more useful when the interface prioritizes decisions and actions while documentation carries the implementation detail.

## Calendar View Simplification

### Problem

Some calendar view modes are not useful for the shared household display and may introduce confusion.

### Current Status

The dashboard navigation and calendar configuration are being reviewed to determine whether unused view modes can be hidden entirely or should simply be excluded from the normal household navigation path.

This remains an open refinement rather than a completed feature.

## Overall Engineering Lessons

- Real household feedback revealed requirements that were not obvious during initial implementation.
- Correct source data and good visual presentation sometimes require separate entities.
- UI simplification should not weaken the underlying data model.
- Small CSS changes should be tested incrementally against rendered component structure.
- Integrations often need script or API glue to support a complete user workflow.
- Public documentation should record failed assumptions and design changes, not only the final configuration.
