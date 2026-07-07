# Helpers

This file documents the Home Assistant helpers used by the Work Week dashboard.

## Work Week Navigation

### `input_number.work_week_offset`

Used to control which Monday–Sunday week is displayed.

Recommended settings:

- Minimum: `-4`
- Maximum: `8`
- Step: `1`
- Mode: Slider or Box

Offset behavior:

- `0` = current Monday–Sunday week
- `1` = next Monday–Sunday week
- `-1` = previous Monday–Sunday week

Both Joe and Samantha’s Work Week sensors use this same helper, so the week navigation controls both schedules together.

## Work Week Refresh

### `input_button.work_week_refresh`

Used to manually refresh the Work Week template sensors.

The Work Week sensors refresh when this helper is pressed.

Triggered by:

- Manual refresh button, if added to the dashboard
- `script.add_samantha_shift` after creating a new calendar event

This helper replaced the earlier 15-minute polling behavior for the Work Week sensors.

## Samantha Shift Entry

### `input_select.samantha_shift_location`

Dropdown used to select Samantha’s shift location or shift type.

Current options:

- Central
- Contemporary
- Coordinating
- Other

The selected value is used in the calendar event title.

Example event title:

```text
Samantha Work - Central
```

The Work Week markdown sensor strips the `Samantha Work -` prefix before displaying the shift.

Example display:

```text
Central
Tue, Jul 7
3:45 PM - Wed 12:45 AM
```

### `input_datetime.samantha_shift_start`

Date/time helper for the shift start.

Used by:

- `script.add_samantha_shift`

The value becomes the event start time on:

- `calendar.samantha_work`

### `input_datetime.samantha_shift_end`

Date/time helper for the shift end.

Used by:

- `script.add_samantha_shift`

The value becomes the event end time on:

- `calendar.samantha_work`

For overnight shifts, the end date should be set to the following day.

Example:

```text
Start: Tuesday 3:45 PM
End: Wednesday 12:45 AM
```

### `input_text.samantha_shift_notes`

Optional notes field for Samantha’s shift.

Used by:

- `script.add_samantha_shift`

If the notes field is empty, unavailable, unknown, or blank, the script does not add notes to the calendar event description.

If notes are present, they are added to the calendar event description.

Example:

```text
Notes: Closing shift
```

## Calendar Entities

### `calendar.samantha_work`

Dedicated Home Assistant calendar for Samantha’s manually entered work shifts.

Used by:

- `script.add_samantha_shift`
- `sensor.samantha_work_week`

This calendar is the storage layer for Samantha’s schedule.

### `calendar.deputy_calendar_for_joe_belcher`

Deputy calendar containing Joe’s auto-populated work schedule.

Used by:

- `sensor.joe_work_week`

This calendar is treated as read-only. Schedule corrections should be made in Deputy.

## Template Sensors

### `sensor.samantha_work_week`

Generated from:

- `calendar.samantha_work`
- `input_number.work_week_offset`
- `input_button.work_week_refresh`

Displays Samantha’s selected Monday–Sunday work week as markdown.

It formats overnight shifts as one readable entry.

### `sensor.joe_work_week`

Generated from:

- `calendar.deputy_calendar_for_joe_belcher`
- `input_number.work_week_offset`
- `input_button.work_week_refresh`

Displays Joe’s selected Monday–Sunday work week as markdown.

It formats overnight shifts as one readable entry.

## Script

### `script.add_samantha_shift`

Creates a new calendar event on:

- `calendar.samantha_work`

Uses:

- `input_select.samantha_shift_location`
- `input_datetime.samantha_shift_start`
- `input_datetime.samantha_shift_end`
- `input_text.samantha_shift_notes`

After creating the event, the script presses:

- `input_button.work_week_refresh`

This refreshes the Work Week markdown sensors so the dashboard updates shortly after a new shift is added.
