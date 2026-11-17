# DockerMC

A Fabric Minecraft server for **Minecraft 26.3**, run with Docker Compose.

It uses the [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) image, built locally on **Amazon Corretto 25**. Mods download automatically from [Modrinth](https://modrinth.com/), and only whitelisted players can join.

## Setup

1. Create your mod list from the example:
   ```sh
   cp mods.txt.example mods.txt
   ```
2. Add player names to the `WHITELIST` block in `docker-compose.yml`. Until you do, nobody can join.
3. Build and start the server:
   ```sh
   docker compose up -d --build
   ```

Starting the server accepts the [Minecraft EULA](https://aka.ms/MinecraftEULA) (`EULA: "TRUE"` in `docker-compose.yml`).

The server listens on port `25565`. World, config and mod files are kept in `./data`, which git ignores.

## Common tasks

| Task | Command |
|------|---------|
| Follow logs | `docker compose logs -f minecraft` |
| Run a console command | `docker compose exec minecraft rcon-cli <command>` |
| Apply changes to `mods.txt` | `docker compose restart minecraft` |
| Apply changes to `docker-compose.yml` | `docker compose up -d` |
| Stop the server | `docker compose down` |

## Mods

`mods.txt` lists Modrinth project slugs, one per line. See `mods.txt.example` for the supported formats. On every start the server fetches the newest 26.3 build of each listed mod and removes mods that are no longer listed. `mods.txt` is gitignored, so each deployment keeps its own list.

## Whitelist

Players listed under `WHITELIST` in `docker-compose.yml` can join. With `EXISTING_WHITELIST_FILE: "SYNCHRONIZE"`, that list is the master copy:

- Names removed from the list are taken off the whitelist on the next start, and kicked if they are online.
- Players added in-game with `/whitelist add` are removed on the next restart unless they are also in the list.
