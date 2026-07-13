---
name: migrate-server
description: Backup the Minecraft server data and regenerate the world/server by restarting the Docker container. Make sure to use this skill whenever the user asks to "migrate the server", "backup and regenerate", or when updating mods requires wiping the old world to prevent corruption.
---

# Server Migration and Backup

When the user wants to migrate the server, backup the world, or regenerate the server, follow these steps strictly:

1. **Backup the Data Directory**: 
   - Rename the local `data` directory in the workspace to a timestamped backup directory (e.g., `data_backup_YYYYMMDD_HHMMSS`).
   - Use standard terminal commands like `mv data data_backup_$(date +%Y%m%d_%H%M%S)`.

2. **Restart the Server Container**:
   - By default, assume the server is running on the local machine. Run `docker compose down` followed by `docker compose up -d` (or `docker compose restart`) in the project directory.
   - If the user specifies that the server is running on the Raspberry Pi, use SSH to restart the remote container. (Read the `.env` file for the Raspberry Pi credentials and host, and use `sshpass` to execute the restart command remotely).

3. **Wait for User Feedback**:
   - After running the restart command, stop and explicitly ask the user to check if the server is working and successfully generated the new data. 
   - Do not proceed with any other actions until the user confirms the server is up and running.
