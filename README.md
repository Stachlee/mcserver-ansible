# Minecraft Server Ansible Role

This Ansible role sets up and manages a Minecraft server using Docker. It handles the installation of necessary packages, configuration of the server, and management of backups.

## Requirements

- Ansible 2.9 or higher
- Docker and Docker Compose installed on the target machine
- Access to the server with appropriate permissions

## Role Variables

The following variables can be set in `defaults/main.yml` or overridden in your playbook:

- `mc_root_dir`: The root directory for Minecraft files (default: `/opt/minecraft`)
- `mc_data_dir`: The directory where Minecraft data is stored (default: `{{ mc_root_dir }}/data`)
- `local_world_archive`: Path to a local world archive for upload (default: `""`)
- `mc_overwrite_world`: Whether to overwrite the existing world (default: `false`)
- `mc_rcon_password`: RCON password for server management
- `mc_ram_max`: Maximum RAM allocated to Minecraft (default: `6G`)
- `mc_ram_init`: Initial RAM allocated to Minecraft (default: `2G`)
- `mc_server_type`: Type of Minecraft server (default: `PAPER`)
- `mc_world_name`: Name of the Minecraft world (default: `world`)
- `mc_version`: Minecraft version to use (default: `LATEST`, also: `SNAPSHOT` or `x.xx.x`)
- `mc_difficulty`: Difficulty level of the game (default: `normal`)
- `mc_motd`: Message of the Day for the server (default: `Willkommen auf dem Server`)
- `mc_view_distance`: View distance setting (default: `16`)
- `mc_whitelist_enabled`: Enable whitelist (default: `TRUE`)
- `mc_enforce_whitelist`: Kick non-whitelisted players when whitelist is enabled (default: `TRUE`)
- `mc_whitelist_players`: List of player names to add to the whitelist (default: `[]`), e.g. `["Steve", "Alex"]`
- `mc_online_mode`: Enable online mode (default: `TRUE`)
- `mc_plugins_url`: List of direct plugin `.jar` URLs to install into `/data/plugins` (default: `[]`)

### Whitelist players

Use `mc_whitelist_players` to define which player names are allowed to join.
When `mc_whitelist_enabled: "TRUE"` and `mc_enforce_whitelist: "TRUE"`, only these players can connect.

Example:

```yaml
mc_whitelist_enabled: "TRUE"
mc_enforce_whitelist: "TRUE"
mc_whitelist_players:
  - Steve
  - Alex
```

### Plugins

Plugins are managed via `mc_plugins_url` and synchronized on every role run.

- Add a URL to `mc_plugins_url`: the plugin `.jar` is downloaded to `/data/plugins`.
- Remove a URL from `mc_plugins_url`: the corresponding `.jar` is removed from `/data/plugins`.
- Re-running the role applies only the delta (new/removed plugins) and keeps world data in `{{ mc_data_dir }}`.

Use a plugin-capable server type such as `PAPER` (recommended), `SPIGOT`, or `PURPUR`.
`VANILLA` does not support Bukkit/Paper plugins.

Example:

```yaml
mc_server_type: "PAPER"
mc_plugins_url:
  - "https://example.org/plugins/LuckPerms-Bukkit-5.4.158.jar"
  - "https://example.org/plugins/Vault.jar"
```

## Dependencies

This role depends on the following roles:
- `geerlingguy.docker`: Role for managing Docker installation and configuration.

## Example Playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: your_username.minecraft
      mc_server_type: "PAPER"
      mc_version: "1.21.5"
      mc_ram_max: "6G"
      mc_ram_init: "2G"
      mc_rcon_password: "CHANGE_ME"

      # Keep existing world/data unless you explicitly migrate a world archive
      mc_overwrite_world: false
      local_world_archive: ""

      # Whitelist
      mc_whitelist_enabled: "TRUE"
      mc_enforce_whitelist: "TRUE"
      mc_whitelist_players:
        - Steve
        - Alex

      # Plugins (direct .jar URLs)
      mc_plugins_url:
        - "https://example.org/plugins/LuckPerms-Bukkit-5.4.158.jar"
        - "https://example.org/plugins/Vault.jar"
```

## License

BSD-3-Clause

## Author Information

This role was created in 2026 by Mattis.
