# Getting Started

## Purchase the Plugin

Download BlueSpoof from one of the official stores:

- [Blueva Store](https://blueva.net/store)

## Server Requirements

Before installing, ensure your server meets these requirements:

- **Spigot** or any fork (Paper, Purpur, Folia, etc.)
- **Minecraft 1.21.11 or 26.1**
- **Java 21** or higher
- **1-2 GB RAM** minimum (excluding other plugins and players)

## Installation

1. **Stop your server** if it is running
2. **Download** the plugin JAR from your purchase location
3. **Place** the JAR file in your server's `plugins` folder
4. **Start** the server
5. The plugin will generate all configuration files automatically

## First Fake Player Connection

### 1. Open the Admin GUI (recommended)

If the Admin GUI is enabled (default), simply run:

```
/bs
```

This opens an interactive menu where you can connect, disconnect, and manage fake players without memorizing commands.

### 2. Connect a Fake Player via Command

```
/bs connect 1
```

This connects one fake player to the server using a random nick from `nicks.yml`.

### 3. Check That It Worked

```
/bs list
```

You should see the fake player's name in the list.

### 4. Disconnect

```
/bs disconnect all
```

This removes all fake players from the server.

## Configuration Files

After the first start, BlueSpoof creates the following files inside `plugins/BlueSpoof/`:

| File | Purpose |
|------|---------|
| `settings.yml` | Global settings — language, cache, proxy sync, GUI toggle |
| `nicks.yml` | Pool of names used for fake players |
| `modules/connection.yml` | Ping spoofing, IP spoofing, batch connect behavior |
| `modules/movement.yml` | Human-like roaming, sprinting, jumping, idle pauses |
| `modules/chat.yml` | AI-generated and configurable messages |
| `modules/fluctuation.yml` | Time-based automatic join/leave schedules |
| `modules/events.yml` | Commands triggered on join, quit, and timed tasks |
| `modules/trace.yml` | Recording regions and playback settings |
| `modules/ranks.yml` | Weighted rank assignment for fake players |
| `modules/discretion.yml` | Hide BlueSpoof branding |
| `languages/en_UK.yml` | All plugin messages and GUI text |

## Quick Command Reference

| Action | Command |
|--------|---------|
| Open GUI | `/bs` |
| Connect fake players | `/bs connect [amount]` |
| Disconnect fake players | `/bs disconnect [amount\|all\|name]` |
| List fake/real players | `/bs list (fake\|real)` |
| Check if a player is fake | `/bs check [player]` |
| Send chat as a fake player | `/bs chat [name] [message]` |
| Force a fake player to run a command | `/bs sudo [name] [command]` |
| Take control of a fake player | `/bs control [name]` |
| Stop controlling | `/bs control stop` |
| Reload plugin | `/bs reload` |

## Enabling Modules

All modules are controlled through their respective YAML files. To enable one:

1. Open the module's config file in `plugins/BlueSpoof/modules/`
2. Set `enabled: true` (or the equivalent key for that module)
3. Save the file
4. Run `/bs reload` or restart the server

The **Connection** module is always active because it handles core fake-player connection behavior.

## Troubleshooting

**"No fake players connected" after running `/bs connect`**
- Check the console for errors
- Ensure your server is in offline mode if you are not using a proxy
- Verify that `nicks.yml` is not empty

**"Insufficient permissions"**
- You need `bluespoof.admin` or OP to use most commands
- Individual permissions are listed in [Commands & Permissions](commands-permissions.md)

**Fake players disconnect immediately**
- Check for authentication plugin conflicts (AuthMe, etc.)
- Ensure the server is not in online mode without a valid proxy setup
- Review the Events module; some example commands are disabled by default

**High CPU usage with many fake players**
- Disable the Movement module or reduce the number of roaming bots
- Lower the `max_frames` value in `trace.yml` if using Trace recordings
- Enable isolation in `movement.yml` to keep bots in a hidden world
