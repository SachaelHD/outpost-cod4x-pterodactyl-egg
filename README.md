# Outpost Gaming – Call of Duty 4X (CoD4X) Pterodactyl Egg

This repository contains an Outpost Gaming–maintained Pterodactyl egg for running a Call of Duty 4X (CoD4X) dedicated server on Linux nodes.

The egg automatically:

- Downloads and extracts the latest CoD4X Linux server build (by default from the official CoD4X download URL).
- Flattens the archive so all server files live directly in `/home/container`.
- Generates a minimal `main/server.cfg` on first install.
- Exposes sane environment variables for hostname, slots, passwords, gametype, map rotation, mods (`fs_game`), and startup map.

## ⚠️ You still need your own CoD4 game files

This egg **does not** ship any Infinity Ward / Activision assets.

After installation, you **must upload** your own data from a legitimate copy of Call of Duty 4:

- `main/*.iwd`
- `zone/english/*.ff`

Drop those into the server's `/home/container/main` and `/home/container/zone/english` directories (matching your CoD4 install). The server will not start properly without them.

## Features

- CoD4X Linux server, installed automatically via `curl + unzip`.
- Simple, proven startup command:
  ```bash
  ./cod4x18_dedrun \
    +developer 0 \
    +set dedicated 2 \
    +set net_port {SERVER_PORT} \
    +set fs_game {FS_GAME} \
    +set sv_hostname {SERVER_HOSTNAME} \
    +set sv_maxclients {MAX_SLOTS} \
    +set g_gametype {SERVER_GAMETYPE} \
    +map {START_MAP}
  ```
- Mod support via `fs_game` (e.g. `mods/zom_db`, `mods/deathrun`, `mods/promodlive`).
- Map rotation managed in `server.cfg` via the `MAP_ROTATION` variable.
- Optional master server auth token support for public listing.

## Environment variables (high level)

The egg exposes the following key variables in the panel:

- **SERVER_HOSTNAME** – Name shown in the in‑game browser.
- **SERVER_PASSWORD** – Optional join password (leave blank for public).
- **RCON_PASSWORD** – RCON password for remote management.
- **MAX_SLOTS** – Player slots (1–64).
- **MAP_ROTATION** – Raw CoD4 map rotation string (e.g. `gametype war map mp_crossfire`).
- **SERVER_NETWORK_TYPE** – `1` = LAN only, `2` = Public (currently informational).
- **SERVER_AUTH_TOKEN** – CoD4X masterserver token (for public listing).
- **FS_GAME** – Mod path, e.g. `mods/zom_db` (blank = vanilla).
- **SERVER_GAMETYPE** – Gametype (`war`, `dm`, `sab`, `sd`, `hq`, `koth`, `dom`, `ctf`, or mod-specific).
- **START_MAP** – Startup map (e.g. `mp_crash`, `mp_backlot`, `mp_shipment`).
- **DOWNLOAD_URL** – Override the CoD4X download URL; defaults to the official CoD4X link.

You can tweak these per-server from the Pterodactyl panel.

## Installation (Pterodactyl)

1. Download the `outpost-cod4x-egg.json` file from this release.
2. In your Pterodactyl admin panel, go to **Nests → Import Egg** and upload the JSON.
3. Create a new server using this egg.
4. (Optional) Change **DOWNLOAD_URL** if you want to use your own mirror or a different CoD4X build.
5. Start the server once to let the installer run.
6. Upload your **CoD4 game data** (`main` and `zone`) into the server's file system.
7. Adjust hostname, passwords, rotation, gametype, and startup map to taste.
8. Restart the server.

## Notes

- This egg is built for Linux-based Pterodactyl nodes.
- The install script runs in an Alpine container and uses `curl` and `unzip`.
- The default CoD4X URL can be changed via the `DOWNLOAD_URL` variable.

---

Maintained by **Outpost Gaming**. If you modify or rebrand this egg, please leave credit somewhere in your README or panel description.
