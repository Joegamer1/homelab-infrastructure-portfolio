# Home Assistant household dashboard

I started with a shared calendar view and added chores and task tracking as the household started using it. I wanted common actions in one place, without needing to work through Home Assistant's administration screens.

## Current setup

Home Assistant OS runs in a Proxmox VM. It connects to Google Calendar and to Donetick, which runs in the Debian Docker VM.

The dashboard shows upcoming events, active chores, and chores completed today. Daily and weekly chore creation and Donetick completion notifications are working.

```text
Proxmox VE
├── Home Assistant OS VM
│   ├── Household dashboard
│   ├── Google Calendar integration
│   └── Donetick integration and chore views
└── Debian Docker VM
    └── Donetick
```

## Interface decisions

I keep common actions visible and the layout readable on shared devices. Home Assistant handles the interface and automations, Google Calendar supplies the schedule, and Donetick stores chores and their recurrence. I add features when there is a household use for them.

Public examples leave out private household data.

## Related notes

- [Family dashboard](home-assistant-family-dashboard/)
- [Work Week calendar](home-assistant-work-week/)
- [Chore creation and recurrence](home-assistant-chores/)
- [Recent fixes](../operations/recent-project-troubleshooting.md)
