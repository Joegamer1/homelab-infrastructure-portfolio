# Home Assistant Family Dashboard

This project documents the main household-facing Home Assistant interface. It acts as the entry point for family scheduling, chores, work-week visibility, personal todo lists, shopping, and the future Hermes household summary.

## Purpose

The dashboard is intended to feel like a small household application rather than a default Home Assistant administration screen.

Its goals are to:

- Provide one clean landing page for family tools.
- Make calendars, chores, todo lists, and shopping easy to reach.
- Avoid exposing unnecessary administrative controls.
- Remain readable on phones, tablets, and a shared wall display.
- Provide a natural home for a calm AI-generated household summary.

## Current Navigation

The home page provides direct access to:

- Family Calendar
- Chores
- Work Week
- Personal Todo Lists
- Shopping
- Home Assistant Overview

The layout uses large, clear navigation cards on the splash page and slimmer controls within individual project views. Navigation paths were tested directly because dashboard URLs do not always match visible dashboard titles.

## Current Design Direction

The dashboard uses a warm Catppuccin Latte/Rosewater-inspired theme and prioritizes:

- Strong visual hierarchy
- Clear grouping of household functions
- Consistent icons and navigation behavior
- Minimal instructional clutter
- Full use of Home Assistant sections layouts
- Responsive behavior in portrait and landscape
- Stable custom-card behavior over fragile cosmetic overrides

## Hermes Hero Summary

The next major visible feature is a Hermes-generated hero summary at the top of the Home dashboard.

The card should read like a calm household note, not a technical status report. It should remain cozy, reassuring, and useful throughout the day.

Potential context includes:

- Important events and work schedules
- Chores due or recently completed
- Useful shopping or todo reminders
- Meaningful household status

The summary should update on a reasonable interval and after important source changes rather than regenerating continuously.

## Problems Encountered and Resolutions

### The landing page looked like a configuration screen

**Problem:** A plain heading followed by default buttons did not feel like the main page of a family application.

**Resolution:** Reworked the page as a dedicated visual entry point with stronger card hierarchy and clearer destinations.

### Dashboard navigation paths did not always match expectations

**Problem:** Some buttons opened the wrong dashboard because the visible name was assumed to be its actual Lovelace path.

**Resolution:** Verified each working URL directly and used the exact paths in navigation actions.

### Mobile edge-to-edge calendar experiments caused regressions

**Problem:** Attempts to remove side margins improved the portrait layout briefly but caused the card to shrink back, hide or disturb surrounding navigation, or become left-aligned in landscape.

**Resolution:** Reverted the margin experiments to the known stable layout. A cosmetic improvement was not kept when it damaged orientation changes or kiosk behavior.

### Kiosk mode and layout styling became entangled

**Problem:** Some margin experiments unintentionally affected the sidebar or header behavior.

**Resolution:** Kept kiosk mode as a separate, explicit dashboard concern and avoided using layout CSS as a substitute for navigation visibility controls.

### Large action buttons consumed too much space

**Problem:** Standard buttons worked well on the home page but felt oversized below schedule and workflow cards.

**Resolution:** Kept prominent navigation on the splash page while using slimmer action controls inside detailed views.

### Calendar controls shifted or overlapped

**Problem:** Changes that improved one orientation caused regressions in another.

**Resolution:** Tested portrait and landscape after each change and retained the version that behaved acceptably in both.

### Some controls are implemented inside the custom card

**Problem:** The calendar add button remained a plus symbol after styling attempts, indicating that the label was controlled internally by the card.

**Resolution:** Kept the working control instead of depending on brittle DOM manipulation.

### Theme changes initially appeared to do nothing

**Problem:** A theme file could exist without being loaded or selected.

**Resolution:** Added the themes directory to Home Assistant configuration, reloaded or restarted, and explicitly selected the theme.

## Design Principles

### Household first

The interface is built around what a household member needs to do, not Home Assistant's internal organization.

### Separate overview from administration

The family home page links to purpose-built views. Administrative configuration remains outside the normal workflow.

### Test both mobile orientations

A layout is not complete when it works only in portrait mode.

### Prefer stable behavior over fragile cosmetic overrides

When a custom card hard-codes an internal control, preserving a reliable interface is better than depending on selectors that may break after an update.

### Keep the AI layer informative before making it powerful

The first Hermes feature is a read-only summary. Device and service actions will be introduced separately and deliberately.

## Future Improvements

- Add the Hermes hero summary.
- Continue Shopping List Card integration.
- Standardize spacing and icon behavior across household views.
- Reduce calendar view modes when supported by the custom card.
- Evaluate personal todo ownership mapping in Hermes without inventing metadata.
- Add sanitized screenshots after the layout stabilizes.

## Privacy

Public screenshots should avoid personal calendar events, work locations, private names, device identifiers, entity IDs containing private details, tokens, and internal URLs.

## Skills Demonstrated

- Home Assistant Lovelace dashboard design
- Information architecture
- Household-focused UX design
- Responsive sections and grid layouts
- Navigation-path troubleshooting
- Home Assistant theme configuration
- Card Mod and custom-card UI troubleshooting
- Regression testing across viewport orientations
- AI feature placement and content design