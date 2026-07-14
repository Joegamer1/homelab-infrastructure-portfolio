# Project Update — July 14, 2026

This update records the Home Assistant and Hermes work discussed after the previous documentation commits and adds sanitized configuration examples where the implementation is mature enough to publish.

## Home Assistant Family Dashboard

- Retained the household-first splash-page design and kiosk-mode separation.
- Kept the stable calendar layout after edge-to-edge mobile experiments caused landscape and navigation regressions.
- Added a sanitized `dashboard.yaml` example covering the Home navigation layout and the planned Hermes hero-summary placement.
- Shopping remains a dedicated dashboard destination. The Shopping List Card is installed, but its final interaction model still needs documentation after hands-on testing.
- The hero summary remains intended to sound like a calm household note throughout the day, not a technical status report or evening-only greeting.

## Work Week and Calendar Workflow

- Shift entry now uses separate date, start-time, and end-time helpers, with time fields initialized to `00:00`.
- Browser Mod remains part of the workflow as the confirmation popup layer. The dashboard gathers values, Browser Mod previews them with Cancel and Add Shift controls, and the confirmed action calls the calendar script.
- The calendar script creates an accurate timed event and a separate all-day display event on the shift's start date.
- Overnight shifts advance the detailed event's end date without creating a second display ribbon on the following day.
- Added sanitized examples for helpers, dashboard cards, Browser Mod confirmation, and the calendar-writing script.
- Per-shift deletion remains planned. It must remove the correct source event and corresponding display event without relying only on event title text.
- The stale month label in Home Assistant's date picker remains an upstream interface problem.

## Donetick Chores

- Daily and weekly recurring chore creation remains functional through Home Assistant REST commands and scripts.
- The request payload explicitly uses rolling recurrence so the next interval advances from completion.
- Added sanitized REST, script, and dashboard examples.
- The dashboard examples hide the built-in add-item path, preserve per-person list presentation, and bold chore names through the nested todo item selector.
- Donetick user IDs remain the authoritative assignment model. Home Assistant person entities and visual list grouping do not create native per-item ownership metadata.
- Monthly and quarterly recurring workflows remain planned.

## Hermes House Brain

- Hermes remains planned as a separate Docker application under `/srv/docker/hermes` on the Debian VM.
- Home Assistant remains the source of truth for calendars, chores, shopping, todo lists, people, devices, and service execution.
- The initial deployment remains read-only through a dedicated Home Assistant account and long-lived token.
- Secrets belong in an untracked `.env` file.
- Added planning examples for Docker Compose and `.env.example`. The image value is intentionally a placeholder until the actual Hermes implementation is selected and validated.
- Alexa can continue handling direct device commands. Hermes is intended for requests requiring interpretation, context, summaries, or coordination across systems.
- Proactive announcements require an explicit Home Assistant automation, scheduler, or event trigger; the conversational model does not independently wake itself.

## Documentation Boundaries

The published YAML is sanitized and illustrative. It excludes tokens, private URLs, internal addresses, personal calendar data, private work locations, and production-only identifiers. The examples document architecture and workflow without pretending the public repository is a drop-in backup of the live Home Assistant configuration.
