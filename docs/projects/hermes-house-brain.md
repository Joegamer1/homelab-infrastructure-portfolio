# Hermes House Brain Project

This document tracks my planned “House Brain” project: a local-first AI assistant layer that connects my homelab, Home Assistant, household dashboards, and eventually voice input into one more intelligent household operations system.

The goal is not to replace Home Assistant. Home Assistant remains the automation platform. The purpose of Hermes is to act as an interpretation and orchestration layer that can understand more natural requests, summarize household context, and trigger controlled actions through existing systems.

## Project Status

Current status: planned / early design.

The core homelab foundation is already in place:

- Proxmox VE host
- Debian Docker VM
- Home Assistant OS VM
- Google Calendar integration
- Donetick integration
- Tailscale private access
- Homepage dashboard
- Uptime Kuma monitoring

Hermes will be built on top of this existing infrastructure rather than replacing it.

## Why This Project Exists

Home Assistant is powerful, but most automation systems still expect the user to think in terms of entities, dashboards, scripts, services, and exact commands.

What I want is a more natural household interface.

Examples of the kind of interaction I want to support:

- “What does tomorrow look like?”
- “What chores are still active today?”
- “What changed around the house since this morning?”
- “Add a reminder or task for later.”
- “Summarize today for the family.”
- “Is anything down in the homelab?”
- “What needs attention?”

The long-term idea is to make the homelab feel less like a collection of dashboards and more like a practical household operating system.

## Design Philosophy

The design goal is local-first and controlled.

Hermes should not have direct uncontrolled access to everything. It should interact with the environment through defined APIs, scripts, and Home Assistant services.

The assistant layer should be able to interpret requests, but actual actions should stay bounded and reviewable.

Important design principles:

- Home Assistant remains the source of truth for home automation
- Hermes acts as the natural language and reasoning layer
- Actions should be explicit and controlled
- Sensitive data should not be dumped into every prompt
- Context should be retrieved only when needed
- Voice integration should come later, after typed/local access works
- The system should be useful before it is flashy

## Planned Architecture

The first version of the project will run on the Debian Docker VM, separate from Home Assistant OS.

Planned structure:

```text
User request
↓
Hermes / House Brain service
↓
Controlled context retrieval
↓
Home Assistant API / selected service APIs
↓
Response or approved action
