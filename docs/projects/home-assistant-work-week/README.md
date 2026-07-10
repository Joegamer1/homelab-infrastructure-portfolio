# Home Assistant Work Week Dashboard

This project documents a Home Assistant workflow for entering, storing, and displaying household work schedules in a way that remains readable on both weekly dashboard cards and full calendar views.

## Project Goal

The original goal was simple: make work schedules visible inside the family Home Assistant dashboard. In practice, overnight shifts, calendar rendering behavior, and the difference between data storage and display requirements made the project more involved.

The finished design separates the exact shift data from the simplified household-facing calendar display.

## Current Design

The workflow uses two calendar concepts:

1. **Detailed work calendar**
   - Stores the actual start and end time of each shift.
   - Supports shifts that cross midnight.
   - Preserves the data needed by Home Assistant sensors and weekly schedule cards.

2. **Display calendar**
   - Creates a single all-day entry on the date a shift begins.
   - Uses a simple label such as `Sam Works` or `Joe Works`.
   - Does not create a second entry on the following day when a shift ends after midnight.
   - Produces a cleaner ribbon-style calendar entry for household use.

This separation avoids forcing one calendar entity to serve two incompatible presentation requirements.

## Why Overnight Shifts Were a Problem

An overnight shift must use the following day as its end date in the detailed calendar.

Example:

```text
Start: Tuesday 3:45 PM
End: Wednesday 12:45 AM
```

The detailed event is technically correct, but month and day calendar views may visually extend it into Wednesday. That made it appear as though the person worked on both days, even though the desired household display was based only on the day the shift started.

## Solution

The add-shift script now creates:

- One timed event in the detailed work calendar.
- One same-day all-day event in the display calendar.

For an overnight shift beginning Tuesday, the display calendar contains only a Tuesday all-day entry. The Wednesday end time remains available in the detailed calendar without cluttering the family calendar.

## Optional Shift Notes

The shift entry workflow includes an optional notes helper.

If the notes value is empty, blank, unavailable, or unknown, the script omits the calendar description. If notes are present, they are added to the detailed calendar event.

This keeps normal entries clean while still supporting useful context such as closing duties, training, or location changes.

## Dashboard Work

The Work Week dashboard includes:

- A readable weekly schedule card.
- Separate schedule information for each household member.
- Navigation to the calendar editing interface.
- A manual refresh control.
- Slimmer lower action buttons to reduce visual weight.
- Layout refinements for better readability on the household display.

The dashboard uses the detailed calendar data, while the family calendar can use the simplified display entities.

## Problems Encountered and Resolutions

### Overnight events appeared on two days

**Problem:** A shift ending after midnight visually occupied both the starting and ending day in calendar views.

**Resolution:** Kept the accurate timed event for calculations and added a separate all-day display event only on the shift start date.

### All-day events could have covered the following day

**Problem:** Calendar end dates are exclusive. Using the wrong end date could make an all-day entry span too far.

**Resolution:** The display event starts on the shift start date and ends on the following date, which produces exactly one all-day calendar entry.

### One calendar could not satisfy every view

**Problem:** The weekly card needed exact times, while the family calendar needed a clean day-level indicator.

**Resolution:** Split storage and presentation into detailed and display calendars instead of adding increasingly fragile formatting logic to one entity.

### Calendar editing and dashboard navigation felt disconnected

**Problem:** Users needed a practical way to add or edit events without cluttering the schedule card.

**Resolution:** Added compact navigation and refresh buttons below the main display.

### Dashboard controls were visually too large

**Problem:** Default Home Assistant buttons dominated the bottom of the page.

**Resolution:** Reworked the lower controls into the slimmer button style used elsewhere in the family dashboard.

## Privacy

Public examples use neutral labels for non-public location details:

- Location A
- Location B
- Location C
- Other

Public documentation should also avoid real work locations, private notes, personal schedule details, tokens, and raw calendar identifiers that are not intended for publication.

## Files

- `helpers.md`
- `configuration.yaml`
- `dashboard-card.yaml`
- `scripts.yaml`
- `screenshots/`

## Skills Demonstrated

- Home Assistant helper and script design
- Calendar event modeling
- Handling overnight time ranges
- Separation of data and presentation layers
- Iterative dashboard design
- User-driven troubleshooting
- Privacy-conscious public documentation
