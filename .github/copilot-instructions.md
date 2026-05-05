# Copilot Instructions for Guacamole Raspberry Pi Setup

This repository is a deployment guide and Docker Compose config for self-hosting Apache Guacamole on a Raspberry Pi. There is no application source code in this repo; the main files are:
- `readme.md` — step-by-step Raspberry Pi setup and Guacamole connection workflow
- `compose.yaml` — Docker Compose service definition for `flcontainers/guacamole:latest`

## What this repo contains
- A minimal Docker Compose manifest that uses `network_mode: host`
- A local persistent volume named `guac-config`
- A README that documents Raspberry Pi prerequisites, xrdp setup, and how to add the Pi as a Guacamole connection

## Important project patterns
- The Compose file is intentionally minimal: no explicit ports are published because Guacamole runs on host networking
- The configured service is `guacamole`, using the published Docker image `flcontainers/guacamole:latest`
- Timezone is set via `TZ`; keep this consistent if editing the deployment

## What to edit
- Update `readme.md` when setup steps change, especially Pi-specific instructions such as X11 mode or xrdp configuration
- Update `compose.yaml` only when changing deployment details such as image version, volume mapping, or network mode

## Developer workflows
- There is no build or test pipeline in this repository
- The main deployment command is:
  - `docker compose up -d`
- On the Raspberry Pi, the guide expects commands such as:
  - `sudo raspi-config` to enable X11
  - `sudo apt install xrdp -y`
  - `sudo systemctl enable xrdp && sudo systemctl start xrdp`

## What not to assume
- This repository is not a codebase for Apache Guacamole itself; it is a configuration/deployment repo
- There are no source files to compile or tests to run here
- Do not invent missing scripts, CI config, or application files unless the user explicitly requests them

## Suggested edits
- Keep guidance concise and directly tied to `readme.md` and `compose.yaml`
- Prefer specific instructions over generic DevOps advice
- If adding new documentation, reference the Raspberry Pi and Docker Compose context clearly
