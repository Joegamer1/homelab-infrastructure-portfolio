# Sanitized Examples

This directory contains sanitized example configuration files for the homelab infrastructure portfolio.

The purpose of these examples is to demonstrate structure, design patterns, and integration approaches without exposing production secrets, private operational data, or raw live configuration files.

## Purpose

The example files in this directory are intended to show how services are organized and integrated at a high level.

They may include examples for:

- Homepage service definitions
- Homepage Docker integration
- Donetick Docker Compose structure
- Home Assistant dashboard snippets
- Backup script structure

These examples are not intended to be copied directly into a production environment without review and modification.

## What Should Not Be Included

Example files should not contain:

- Real API tokens
- Passwords
- Private keys
- `.env` values
- Home Assistant `secrets.yaml` values
- Donetick API tokens
- Plex claim tokens
- Tailscale auth keys
- Cloudflare tokens
- Personal calendar data
- Raw production configuration files
- Private screenshots
- Sensitive hostnames
- Public IP addresses
- Personal email addresses
- Family-specific personal information

## Placeholder Values

Use placeholder values instead of real production values.

Recommended placeholders include:

- `CHANGEME`
- `LOCAL_IP`
- `TAILSCALE_IP`
- `HOMELAB_HOST`
- `HOME_ASSISTANT_URL`
- `DONETICK_HOST`
- `API_TOKEN_HERE`
- `USERNAME_HERE`
- `PASSWORD_HERE`
- `PRIVATE_HOSTNAME`
- `SERVICE_URL`

## Example Philosophy

These files should show the general pattern of the environment, not expose the exact production implementation.

When creating examples:

1. Start from the purpose of the configuration.
2. Rewrite the example manually.
3. Replace all sensitive values with placeholders.
4. Remove personal data.
5. Remove tokens, secrets, and passwords.
6. Review the file before committing it publicly.

## Planned Example Files

Planned sanitized examples include:

- `homepage/services.example.yaml`
- `homepage/docker.example.yaml`
- `donetick/docker-compose.example.yml`
- `home-assistant/donetick-dashboard.example.yaml`
- `backups/homelab-backup.example.sh`

## Review Checklist

Before committing an example file, confirm:

- No real secrets are present
- No API tokens are present
- No private keys are present
- No real passwords are present
- No personal calendar data is present
- No production `.env` files are included
- No raw application database files are included
- No unnecessary private IPs or hostnames are exposed
- The file uses placeholder values where appropriate

## Notes

The examples in this directory are documentation artifacts. They are meant to demonstrate technical organization, service integration, and operational thinking.

The live production configuration is intentionally not stored in this repository.
