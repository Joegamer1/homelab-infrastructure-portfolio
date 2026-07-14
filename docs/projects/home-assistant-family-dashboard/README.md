# Home Assistant Family Dashboard

This project is the household-facing entry point for calendars, chores, work schedules, personal lists, shopping, and the planned Hermes summary.

## Design Goals

- Present household tasks instead of Home Assistant internals.
- Keep common destinations easy to reach.
- Limit administrative controls on shared views.
- Work on phones, tablets, and a wall display.
- Prefer reliable custom-card behavior over fragile cosmetic overrides.

## Current Navigation

The landing page links to the family calendar, chores, Work Week, personal todo lists, shopping, and the Home Assistant overview. Exact Lovelace paths are used because visible dashboard names do not always match their URLs.

A sanitized dashboard example is included in [`dashboard.yaml`](dashboard.yaml).

## Calendar and Mobile Decisions

Attempts to force the calendar to the edges of the phone improved portrait mode but caused landscape alignment and kiosk regressions. Those changes were reverted in favor of the stable layout.

Some controls are implemented internally by custom cards. For example, the calendar add button remained a plus symbol despite styling attempts. The working control was kept rather than relying on brittle DOM selectors.

Portrait and landscape are tested separately after layout changes.

## Theme and Presentation

The interface uses a warm Catppuccin Latte/Rosewater-inspired theme, clear card hierarchy, consistent icons, and smaller secondary actions inside detailed views.

Theme files must be loaded through Home Assistant configuration and then selected; creating the file alone does not apply it.

## Hermes Hero Summary

The planned hero card will display a calm household note using selected calendar, chore, list, and household context. It should refresh on a schedule and after meaningful source changes rather than regenerate continuously.

The first version is informational. Device and service actions will be introduced separately.

## Remaining Work

- Add the Hermes summary entity and card.
- Finish Shopping List Card documentation.
- Reduce calendar view choices where supported.
- Add sanitized screenshots after the layout stabilizes.

Public examples omit private events, names, tokens, internal URLs, and sensitive entity identifiers.