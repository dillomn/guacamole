# Guacamole Remote Desktop — Raspberry Pi Setup Guide
 
## Prerequisites
 
- Raspberry Pi 4 or 5 running Raspberry Pi OS
- Docker and Docker Compose installed
- Pi booted into **X11 mode** (not Wayland)

---
 
## Step 1 — Install Docker
 
```bash
curl -sSL https://get.docker.com | sh
```

```bash
sudo usermod -aG docker $USER
```

---
 
## Step 2 — Switch Pi to X11
 
```bash
sudo raspi-config
```
 
Navigate to: **Advanced Options → Wayland → X11** then reboot.
 
---
 
## Step 3 — Install xrdp
 
```bash
sudo apt update && sudo apt install xrdp -y
sudo systemctl enable xrdp && sudo systemctl start xrdp
```
 
Edit the config for xrdp:
```bash
sudo nano /etc/xrdp/xrdp.ini
```
Change the line ``security_layer=negotiate`` to ``security_layer=rdp``

Then restart xrdp
```bash
sudo systemctl restart xrdp
```
 
---
 
## Step 4 — Deploy Guacamole
 
Create or copy the docker `compose.yaml` file:
 
```yaml
version: "3"
 
services:
  guacamole:
    image: flcontainers/guacamole:latest
    container_name: guacamole
    restart: unless-stopped
    network_mode: host
    volumes:
      - guac-config:/config
      - /etc/localtime:/etc/localtime:ro
    environment:
      TZ: "Australia/Sydney"
 
volumes:
  guac-config:
    driver: local
```
 
Start it:
 
```bash
docker compose up -d
```
 
---
 
## Step  — Add the Pi as a Connection
 
1. Open `http://<pi-ip>:8080/guacamole` and log in with `guacadmin` / `guacadmin`
2. Go to **Settings → Connections → New Connection** and fill in:
 
| Field | Value |
|-------|-------|
| Protocol | RDP |
| Hostname | `127.0.0.1` |
| Port | `3389` |
| Username | your Pi username |
| Password | your Pi login password |
| Security mode | RDP |
| Ignore server certificate | ✅ |
 
3. Save, go to the home screen, click the connection name to connect.
 
> **Change the default password** immediately via top-right menu → Settings → Preferences.
 