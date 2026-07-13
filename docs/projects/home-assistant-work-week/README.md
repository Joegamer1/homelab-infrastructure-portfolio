# Home Assistant Work Week Dashboard

This project documents a Home Assistant workflow for entering, storing, editing, and displaying household work schedules in a way that remains readable on weekly dashboard cards and full calendar views.

## Project Goal

The original goal was simple: make work schedules visible inside the family Home Assistant dashboard. In practice, overnight shifts, calendar rendering behavior, date-picker defects, popup dependencies, and the difference between data storage and display requirements made the project more involved.

The current design separates exact shift data from simplified household-facing calendar display.

## Current Design

The workflow uses two calendar concepts:

1. **Detailed work calendar**
   - Stores the actual start and end time of each shift.
   - Supports shifts that cross midnight.
   - Preserves the data needed by Home Assistant sensors and weekly schedule cards.

2. **Display calendar**
   - Creates a single all-day entry on the date a shift begins.
   - Uses a simple label such as `Sam Works` or `Joe Works`.
   - Does not create a second entry when a shift ends after midnight.
   - Produces a cleaner ribbon-style calendar entry for household use.

This separation avoids forcing one calendar entity to serve two incompatible presentation requirements.

## Shift Entry Workflow

The add-shift workflow is launched from Home Assistant through a Browser Mod popup.

The form collects:

- Household member
- Start date
- Start time
- End time
- Optional location or notes

The time helpers now default to `00:00` instead of the current time. This produces a predictable blank starting point and avoids accidentally submitting the time at which the form happened to be opened.

Browser Mod must be installed as a Home Assistant integration, not merely present as a frontend resource. Before the integration was added, the button appeared to do nothing because the popup service was unavailable.

## Overnight Shift Handling

An overnight shift must use the following day as its end date in the detailed calendar.

Example:

```text
Start: Tuesday 3:45 PM
End: Wednesday 12:45 AM
```

The script compares the entered end time with the start time. When the end time is earlier, the detailed event ends on the following date. The display event still appears only on Tuesday.

## Dashboard Work

The Work Week dashboard includes:

- Readable weekly schedule cards
- Separate schedule information for each household member
- Previous, current, and next week navigation
- A Browser Mod shift-entry popup
- Navigation to calendar editing
- A manual refresh control
- Slim lower action buttons
- Mobile-friendly formatting

## Problems Encountered and Resolutions

### Overnight events appeared on two days

**Problem:** A shift ending after midnight visually occupied both the starting and ending day in calendar views.

**Resolution:** Kept the accurate timed event for calculations and added a separate all-day display event only on the shift start date.

### Browser Mod popup did not open

**Problem:** The Add Shift button produced no popup even though the dashboard YAML referenced Browser Mod correctly.

**Resolution:** Installed Browser Mod as a Home Assistant integration. A frontend installation alone did not provide the required popup service.

### Add Shift stopped creating events after script changes

**Problem:** Changes to the form and function logic broke the final service call even when the popup itself worked.

**Resolution:** Revalidated the workflow in layers: popup invocation, helper values, script execution, date calculation, and calendar service calls. The working version restored both the detailed and display event creation paths.

### New shift times inherited the current time

**Problem:** Opening the form with helpers initialized to the current time made new entries less predictable and increased the chance of accidental values.

**Resolution:** Defaulted both time inputs to `00:00`.

### Date picker month label becomes stale

**Problem:** After navigating to another month, the date picker can continue showing the previous month label. Selecting a date can then feel inconsistent or appear to choose the wrong date.

**Current status:** This behavior occurred across multiple picker implementations and appears to be a frontend component defect rather than the shift script. The workflow remains usable, but the month label must be treated cautiously until the upstream picker behavior is corrected.

### One calendar could not satisfy every view

**Problem:** The weekly card needed exact times, while the family calendar needed a clean day-level indicator.

**Resolution:** Split storage and presentation into detailed and display calendars.

### Dashboard controls were visually too large

**Problem:** Default Home Assistant buttons dominated the bottom of the page.

**Resolution:** Reworked the lower controls into the slimmer button style used elsewhere in the family dashboard.

## Planned Improvement: Per-Shift Delete

The next major workflow improvement is a delete action beside each displayed shift.

The preferred design is to keep each shift in its own compact row or card so its delete control is visually associated with that specific event. The deletion path must identify the correct detailed event and its corresponding display event without relying only on title text.

This feature remains planned and is not documented as complete.

## Privacy

Public examples use neutral labels for non-public location details. Public documentation should avoid real work locations, private notes, personal schedule details, tokens, internal URLs, and raw calendar identifiers that are not intended for publication.

## Files

- `helpers.md`
- `configuration.yaml`
- `dashboard-card.yaml`
- `scripts.yaml`
- `screenshots/`

## Skills Demonstrated

- Home Assistant helper and script design
- Browser Mod integration
- Calendar event modeling
- Handling overnight time ranges
- Separation of data and presentation layers
- Layered workflow troubleshooting
- Responsive dashboard design
- User-driven iteration
- Privacy-conscious public documentation