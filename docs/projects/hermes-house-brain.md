# Hermes plans

Hermes is a planned AI assistant for the household. I want to ask questions such as “What does tomorrow look like?” or “What chores are still due?” and get an answer from the information already in Home Assistant.

The lab already has Proxmox, a Debian Docker VM, Home Assistant OS, Google Calendar, Donetick, Tailscale, Homepage, and Uptime Kuma. Hermes would run as a separate Docker application and use selected data from those existing systems.

## First version

The first version will have read-only access through a dedicated Home Assistant account and token. It should retrieve only the context needed for the supported question. Home Assistant will continue to store household state and run automations.

```text
Question
    -> Hermes
    -> selected Home Assistant data
    -> answer
```

Voice input and actions such as creating a task come later. Each write action needs its own authorization and failure handling before I add it.

The [project folder](house-brain-hermes/) has the deployment outline, placeholder Compose file, and initial milestones. Hermes is still in planning.
