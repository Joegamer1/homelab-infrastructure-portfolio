# Home Assistant Work Week Dashboard

This project manages household work schedules while keeping detailed shift data separate from simplified family-calendar presentation.

## Calendar Model

### Detailed calendar

Stores exact start and end times, including shifts that cross midnight. Weekly schedule sensors and cards use this data.

### Display calendar

Creates one all-day entry, such as `Sam Works`, on the date the shift begins. An overnight shift does not create a second display entry on the following day.

This split allows accurate calculations and clean calendar ribbons without forcing one entity to satisfy both needs.

## Shift Workflow

1. Dashboard helpers collect the date, start time, end time, location, and optional notes.
2. Browser Mod shows a confirmation popup.
3. The confirmed action calls the Home Assistant script.
4. The script writes the detailed event and the all-day display event.
5. The schedule is refreshed and the input helpers are reset.

Time helpers default to `00:00` instead of inheriting the current time.

Sanitized examples:

- [`helpers.yaml`](helpers.yaml)
- [`dashboard-card.yaml`](dashboard-card.yaml)
- [`scripts.yaml`](scripts.yaml)
- [`configuration.yaml`](configuration.yaml)

## Overnight Handling

When the end time is earlier than or equal to the start time, the detailed event ends on the following date. The display event remains on the starting date.

## Troubleshooting Notes

- Browser Mod must be installed as an integration for popup services to work.
- Popup rendering, helper state, script execution, date calculation, and calendar writes should be tested separately.
- Home Assistant's date picker can retain a stale month label after navigation. This appears to be a frontend issue rather than script logic.
- Secondary action buttons were reduced in size so they did not dominate the schedule cards.

## Remaining Work

Per-shift deletion is still planned. The delete action must identify both the detailed event and its matching display event without relying only on title text.

Public examples omit real work locations, private notes, tokens, internal URLs, and personal schedule data.