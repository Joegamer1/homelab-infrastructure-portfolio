# Home Assistant Family Dashboard

This project documents the main household-facing Home Assistant interface. It acts as the entry point for family scheduling, chores, work-week visibility, personal todo lists, shopping, and future household automation features.

## Purpose

The dashboard is intended to feel like a small household application rather than a default Home Assistant administration screen.

Its goals are to:

- Provide one clean landing page for family tools.
- Make calendars, chores, todo lists, and shopping easy to reach.
- Avoid exposing unnecessary administrative controls.
- Remain readable on phones, tablets, and a shared wall display.
- Give future House Brain features a natural place to surface household summaries.

## Current Navigation

The home page provides direct access to:

- Family Calendar
- Chores
- Work Week
- Personal Todo Lists
- Shopping
- Home Assistant Overview

The layout uses large, clear navigation cards on the main splash page and slimmer controls within individual project views. Navigation paths were tested directly because dashboard URL paths do not always match the visible dashboard title.

## Design Work

The original landing page was functional but visually flat. It consisted primarily of a markdown heading and standard buttons.

The updated design focuses on:

- A more intentional splash-page layout.
- Strong visual hierarchy.
- Clear grouping of household functions.
- Consistent icons and navigation behavior.
- Fewer explanatory blocks inside the interface.
- Better use of the available width in a Home Assistant sections view.
- A consistent warm Catppuccin Latte/Rosewater-inspired theme.
- Responsive behavior for both portrait and landscape mobile layouts.

## Problems Encountered and Resolutions

### The landing page looked like a configuration screen

**Problem:** A plain heading followed by default buttons did not feel like the main page of a family application.

**Resolution:** Reworked the page as a dedicated visual entry point with stronger card hierarchy and clearer navigation destinations.

### Dashboard navigation paths did not always match expectations

**Problem:** Some buttons opened the wrong dashboard or failed because the visible dashboard name was assumed to be its actual Lovelace path.

**Resolution:** Verified each working URL directly and used the exact paths in navigation actions. This included separate paths for Work Week, Todo Lists, Shopping, Calendar, and the Home Assistant Overview.

### Large action buttons consumed too much space inside project views

**Problem:** Standard buttons worked well for the home page but felt oversized below schedule and workflow cards.

**Resolution:** Kept prominent navigation on the splash page while using slimmer action controls inside detailed views.

### Mobile calendar controls shifted or overlapped

**Problem:** Changes that improved one screen orientation caused regressions in another. Header controls could become crowded, misaligned, or wrap poorly between portrait and landscape views.

**Resolution:** Iterated on responsive Card Mod rules and tested both orientations after each change instead of treating mobile as one fixed layout.

### Some calendar controls are implemented inside the custom card

**Problem:** The calendar add button remained a plus symbol even after styling attempts, indicating that the icon or label was controlled by the custom card rather than ordinary Lovelace button YAML.

**Resolution:** Kept the working control instead of applying increasingly brittle CSS. Similar caution applies to repositioning the calendar view selector because internal card structure can change across releases.

### Calendar views exposed options that were not useful for the household

**Problem:** The calendar interface can expose multiple view modes, including modes that may be confusing or unnecessary on a shared display.

**Resolution:** The normal household workflow now enters the calendar through a purpose-built dashboard. Hiding every internal view option remains dependent on what the custom calendar card exposes.

### Functional cards accumulated too much explanatory text

**Problem:** Instructions inside cards made the dashboard feel busier and less application-like.

**Resolution:** Moved technical explanation into documentation and kept the live dashboard focused on actions, state, and navigation.

### Theme changes initially appeared to do nothing

**Problem:** A theme file could exist without being loaded or selected, making visual changes appear ineffective.

**Resolution:** Added the themes directory to Home Assistant configuration, reloaded or restarted as needed, and explicitly selected the theme before judging the CSS or palette.

## Design Principles

### Household first

The interface is built around what a household member needs to do, not around Home Assistant's internal organization.

### Clean defaults

A user should be able to open the dashboard and immediately understand where to go without reading setup instructions.

### Separate overview from administration

The family home page links to purpose-built views. Administrative configuration remains outside the normal household workflow.

### Consistent interaction patterns

Large cards are used for primary destinations. Compact controls are used for secondary actions such as refresh or edit.

### Test responsive changes in both orientations

A mobile layout is not complete when it only works in portrait mode. Landscape width, wrapping, and control alignment must be validated separately.

### Prefer stable behavior over fragile cosmetic overrides

When a custom card hard-codes an internal control, preserving a reliable interface is better than depending on selectors that may break after an update.

## Future Improvements

- Standardize spacing, card sizing, and icon behavior across household views.
- Reduce or hide calendar view modes that are not useful on the shared display when supported by the calendar card.
- Continue integrating the Shopping List Card into the shopping workflow.
- Evaluate whether personal todo lists should remain separate entities or gain an assignment layer through future House Brain logic.
- Add household status summaries from the future House Brain project.
- Add sanitized screenshots after the layout stabilizes.

## Privacy

Public screenshots should avoid personal calendar events, work locations, names that are not intentionally public, device identifiers, entity IDs containing private details, and internal URLs.

## Skills Demonstrated

- Home Assistant Lovelace dashboard design
- Information architecture
- Household-focused UX design
- Responsive sections and grid layouts
- Navigation-path troubleshooting
- Home Assistant theme configuration
- Card Mod and custom-card UI troubleshooting
- Iterative visual testing