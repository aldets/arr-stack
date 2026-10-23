<div align="center">
  <h1>Arr-Stack: Automated Media Server</h1>
  <p><i>A modular, Docker-based media automation suite.</i></p>
</div>

---

## Repository Structure

The project is split into two logical **stacks**.

| Folder | Purpose | Services |
| :--- | :--- | :--- |
| stacks/core | Infrastructure | Jellyfin, qBit, Portainer, Dozzle, Jellystat |
| stacks/arr | Automation | Sonarr, Radarr, Prowlarr, etc. |
---

## Service Descriptions

### Core Stack (The Foundation)
- **Jellyfin**: Your personal media library. It handles the metadata, posters, and streaming to your devices.
- **qBittorrent**: The "engine room." A lightweight client that handles all file downloads.
- **Portainer**: A visual dashboard to manage, restart, and monitor all these Docker containers without using the terminal.
- **Dozzle**: The "Monitor." A simple, real-time web interface to view logs for all your running containers.
- **Jellystat**: The "Statistician." A self-hosted watch history, statistics, and analytics tracker for Jellyfin backed by PostgreSQL.

### Arr Stack (The Brains)
- **Sonarr / Radarr**: The "Watchmen." You tell them what TV shows or Movies you want, and they automatically find, download, and organize them.
- **Prowlarr**: The "Librarian." It manages all your torrent trackers in one place and syncs them to Sonarr/Radarr.
- **Bazarr**: The "Translator." It automatically searches for and downloads subtitles for your media.
- **Overseerr / Seerr**: The "Front Desk." A clean interface for users to request new content.
- **FlareSolverr**: The "Keymaker." It helps your stack bypass Cloudflare verification on certain websites.
- **Maintainerr / Cleanuparr**: The "Janitors." They delete watched content or unneeded files to save disk space.
---

## Installation & Setup

### 1. Networking
Both stacks use an external network. You must create it once manually:<br>
<code>docker network create arr-network</code>

### 2. Environment Configuration
Create a file named `.env` in the root directory to manage your settings:
- PUID/PGID: Usually 1000 on Raspberry Pi.
- TZ: Your timezone
- Paths: Your mount points for paths and volumes.
- Jellystat: `JELLYSTAT_DB_PASSWORD` and `JELLYSTAT_JWT_SECRET`.

### 3. Deployment
First, ensure you are in the root of the arr-stack directory.

**Start Infrastructure:**<br>
<code>cd stacks/core && docker compose --env-file ../../.env up -d</code>

**Start Automation Suite:**<br>
<code>cd ../arr && docker compose --env-file ../../.env up -d</code>

---

## Port Reference

| Service | Port | Description |
| :--- | :--- | :--- |
| Portainer | 9000 | Container management |
| Jellyfin | 8096 | Media Streaming |
| qBittorrent | 8080 | Torrent Web UI |
| Dozzle | 8081 | Real-time Container Logs |
| Jellystat | 3000 | Jellyfin Watch Statistics |
| Prowlarr | 9696 | Indexer Manager |
| Radarr | 7878 | Movie Automation |
| Sonarr | 8989 | TV Automation |
| Seerr | 5055 | Requests Interface |
| Bazarr | 6767 | Subtitles |
| Maintainerr | 6246 | Janitation |

---

## Configuration Notes
- Transcoding: Jellyfin uses /dev/dri for hardware acceleration.
- Permissions: Ensure user 1000 owns /srv and /mnt.

<div align="center">
  <sub>Built with ❤️ on a Raspberry Pi.</sub>
</div>