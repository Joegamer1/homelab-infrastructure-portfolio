# Home Assistant Family Dashboard

This project documents the main household-facing Home Assistant interface. It acts as the entry point for family scheduling, chores, work-week visibility, and future household automation features.

## Purpose

The dashboard is intended to feel like a small household application rather than a default Home Assistant administration screen.

Its goals are to:

- Provide one clean landing page for family tools.
- Make calendars and chores easy to reach.
- Avoid exposing unnecessary administrative controls.
- Remain readable on a wall-mounted or shared display.
- Give future House Brain features a natural place to surface household summaries.

## Current Navigation

The home page provides direct access to:

- Family Calendar
- Chores
- Work Week
- Other household-focused Home Assistant views as they are added

The layout uses large, clear navigation cards on the main splash page and slimmer controls within individual project views.

## Design Work

The original landing page was functional but visually flat. It consisted primarily of a markdown heading and standard buttons.

The updated design focuses on:

- A more intentional splash-page layout.
- Strong visual hierarchy.
- Clear grouping of household functions.
- Consistent icons and navigation behavior.
- Fewer explanatory blocks inside the interface.
- Better use of the available width in a Home Assistant sections view.

## Problems Encountered and Resolutions

### The landing page looked like a configuration screen

**Problem:** A plain heading followed by default buttons did not feel like the main page of a family application.

**Resolution:** Reworked the page as a dedicated visual entry point with stronger card hierarchy and clearer navigation destinations.

### Large action buttons consumed too much space inside project views

**Problem:** Standard buttons worked well for the home page but felt oversized below schedule and workflow cards.

**Resolution:** Kept prominent navigation on the splash page while using slimmer action controls inside detailed views.

### Calendar views exposed options that were not useful for the household

**Problem:** The calendar interface can expose multiple view modes, including modes that may be confusing or unnecessary on a shared display.

**Resolution:** Began evaluating which calendar modes should remain available and whether unused views can be hidden or avoided through dashboard-level navigation and configuration.

### Functional cards accumulated too much explanatory text

**Problem:** Instructions inside cards made the dashboard feel busier and less application-like.

**Resolution:** Moved technical explanation into documentation and kept the live dashboard focused on actions, state, and navigation.

## Design Principles

### Household first

The interface is built around what a household member needs to do, not around Home Assistant's internal organization.

### Clean defaults

A user should be able to open the dashboard and immediately understand where to go without reading setup instructions.

### Separate overview from administration

The family home page links to purpose-built views. Administrative configuration remains outside the normal household workflow.

### Consistent interaction patterns

Large cards are used for primary destinations. Compact controls are used for secondary actions such as refresh or edit.

## Future Improvements

- Continue visual refinement of the main home page.
- Standardize spacing, card sizing, and icon behavior across household views.
- Reduce or hide calendar view modes that are not useful on the shared display.
- Add household status summaries from the future House Brain project.
- Add sanitized screenshots after the layout stabilizes.

## Privacy

Public screenshots should avoid personal calendar events, work locations, names that are not intentionally public, device identifiers, entity IDs containing private details, and internal URLs.

## Skills Demonstrated

- Home Assistant Lovelace dashboard design
- Information architecture
- Household-focused UX design
- Responsive sections and grid layouts
- Navigation design
- Iterative visual troubleshooting
