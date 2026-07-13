---
name: migrate-server
description: Backup the Minecraft server data and regenerate the world/server by restarting the Docker container. Make sure to use this skill whenever the user asks to "migrate the server", "backup and regenerate", or when updating mods requires wiping the old world to prevent corruption.
---

# Server Migration and Backup

When the user wants to migrate the server, backup the world, or regenerate the server, follow these steps strictly:

1. **Backup the Data Directory**: 
   - Since the server uses a Docker named volume (`minecraft_data`), we must back it up using Docker rather than renaming a local directory.
   - For local setups: run `docker run --rm -v minecraft_data:/data -v $(pwd):/backup busybox tar cvf /backup/data_backup_$(date +%Y%m%d_%H%M%S).tar /data`.
   - For the Raspberry Pi: SSH in and run `docker run --rm -v minecraft-server-instance-ilkv82_minecraft_data:/data -v /opt/minecraft-backups:/backup busybox tar cvf /backup/data_backup_$(date +%Y%m%d_%H%M%S).tar /data` (Note: adjust the volume prefix based on the `docker-compose` project name on the Pi, typically it's `<project_name>_minecraft_data`).

2. **Regenerate the Server Container and Wipe Old Data**:
   - Run `docker compose down -v` to stop the server AND delete the `minecraft_data` volume, effectively wiping the world.
   - Run `docker compose up -d` to restart the server and generate a fresh data volume.
   - If the server runs on the Raspberry Pi, execute these docker commands over SSH using `sshpass` and the credentials in `.env`.

3. **Wait for User Feedback**:
   - After running the restart command, stop and explicitly ask the user to check if the server is working and successfully generated the new data. 
   - Do not proceed with any other actions until the user confirms the server is up and running.
