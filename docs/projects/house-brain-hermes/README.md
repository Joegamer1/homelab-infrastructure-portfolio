# House Brain / Hermes Assistant

This project documents the planned local assistant layer for the homelab and Home Assistant environment.

The goal is to build a practical household assistant that can understand plain-language questions and interact with the systems already running in the home.

## Purpose

- Provide a local-first assistant layer.
- Connect typed assistant requests to Home Assistant data.
- Start with schedule and calendar summaries.
- Keep Home Assistant isolated from experimental assistant code.
- Build toward voice access later.

## Current Direction

The assistant project is planned to run separately from Home Assistant on the Debian Docker VM.

This keeps Home Assistant stable while leaving room to experiment with AI workflows, local APIs, and future voice integrations.

## First Useful Test

The first useful test should answer a question like:

```text
What does tomorrow look like?
```

That test should use Home Assistant calendar data and return a clear household summary.

## Future Ideas

- Calendar summaries
- Household status summaries
- Chore summaries
- Service health summaries
- Voice bridge integration
- Safer intent handling before making changes

## Privacy

Public documentation should avoid exposing private schedule details, names, addresses, tokens, API keys, and raw Home Assistant data.
