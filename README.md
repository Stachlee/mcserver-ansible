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
- `mc_version`: Minecraft version to use (default: `LATEST`)
- `mc_difficulty`: Difficulty level of the game (default: `normal`)
- `mc_motd`: Message of the Day for the server (default: `Willkommen auf dem Server`)
- `mc_view_distance`: View distance setting (default: `16`)
- `mc_whitelist_enabled`: Enable whitelist (default: `TRUE`)
- `mc_online_mode`: Enable online mode (default: `TRUE`)

## Dependencies

This role depends on the following roles:
- `geerlingguy.docker`: Role for managing Docker installation and configuration.

## Example Playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: your_username.minecraft
      mc_overwrite_world: true
      local_world_archive: "/path/to/your/world.zip"
```

## License

BSD-3-Clause

## Author Information

This role was created in 2026 by Juan.
