# Home Assistant Work Week Dashboard

This project documents a custom Home Assistant Work Week dashboard for displaying household work schedules in a cleaner format than the default Home Assistant calendar card.

The original problem was overnight shifts. Home Assistant’s built-in calendar views split overnight events across midnight, which made one shift appear as two separate chunks across two days. That was technically accurate, but not useful for quickly reading a weekly work schedule.

This dashboard solves that by using calendar-backed template sensors and markdown cards to display each shift as one readable entry, even when it crosses midnight.

## Goals

- Display Joe and Samantha’s work schedules in a dedicated Work Week tab.
- Show Monday–Sunday work weeks.
- Allow Previous, This Week, and Next navigation.
- Display overnight shifts as one continuous shift.
- Let Samantha manually enter shifts from Home Assistant.
- Store Samantha’s shifts in a dedicated Home Assistant calendar.
- Pull Joe’s shifts automatically from an existing Deputy calendar.
- Keep the default calendar available without relying on it as the main work schedule view.

## Architecture

```text
Joe Deputy Calendar
        ↓
Home Assistant calendar.get_events
        ↓
sensor.joe_work_week
        ↓
Work Week markdown card
