# Home Assistant Household Dashboard Project

This document describes my Home Assistant household dashboard project.

Home Assistant started as part of the homelab, but it has grown into one of the most practical parts of the environment. The goal is to turn the homelab into something useful for daily household coordination, not just a collection of self-hosted services.

This project currently focuses on family dashboard views, calendar visibility, recurring chores, and self-hosted task tracking through Donetick.

## Project Status

Current status: active and functional.

The current setup includes:

- Home Assistant OS running in a Proxmox VM
- Google Calendar integrated with Home Assistant
- A family-facing Home Assistant dashboard
- Donetick running in Docker
- Donetick integrated with Home Assistant
- Active chore visibility
- Completed-today chore summaries
- Daily and weekly chore creation workflows
- Donetick-backed completion notifications

This is one of the first parts of the homelab that became directly useful for household operations.

## Why This Project Exists

The original goal was not just to run Home Assistant for the sake of running Home Assistant.

I wanted a practical household dashboard that could help organize daily life. Calendar visibility was the starting point, but the project expanded once I started looking at recurring chores and household task management.

The bigger goal is to make the homelab useful to more than just me.

This project helps answer questions like:

- What is happening today?
- What is coming up tomorrow?
- What chores are active?
- What has already been completed today?
- What still needs attention?
- Can household workflows be made easier without relying entirely on cloud apps?

## Design Philosophy

The design is meant to be practical, visible, and spouse/family-friendly.

Home Assistant is the dashboard and automation layer. Donetick is the self-hosted chore backend. Google Calendar provides household schedule visibility.

The goal is not to automate everything just because I can. The goal is to create simple workflows that are actually useful.

Important design principles:

- Keep the dashboard readable
- Avoid clutter
- Make common actions easy
- Keep chore workflows visible
- Use self-hosted tools where practical
- Avoid exposing private household data publicly
- Build features incrementally based on actual usefulness

## Architecture

The project uses multiple parts of the homelab.

```text
Proxmox VE
├── Home Assistant OS VM
│   ├── Home Assistant dashboard
│   ├── Google Calendar integration
│   ├── Donetick integration
│   └── Chore dashboard views
│
└── Debian Docker VM
    └── Donetick
