# Remote access

I use Tailscale to reach the lab while I'm away from home. Proxmox, Portainer, Home Assistant, Uptime Kuma, Homepage, and Pi-hole administration stay on LAN or Tailscale access.

Plex uses a router port forward for remote playback. It is the only service I deliberately expose that way.

## Administration

At home I use the LAN. Remotely I use Tailscale and SSH to the Debian VM as a non-root user. Root SSH login is disabled, and SSH access is restricted to the expected account.

Tailscale gives my trusted devices a private route to the services without separate public port forwards for each management page.

## Still to do

- Add and test host firewall rules on Debian.
- Document the allowed LAN and Tailscale paths.
- Review router port forwards.
- Complete recovery testing.
- Improve service availability alerts.

I need a console recovery path before applying firewall changes that could interrupt SSH or Tailscale access.
