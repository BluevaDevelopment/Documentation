# Commands and Permissions

Complete reference for all BlueSpoof 3.0 commands and permissions.

**Notation:**
- `[required]` - Required parameter
- `(optional)` - Optional parameter
- `<option1|option2>` - Choose one option

---

## Main Command

Main command: `/bluespoof` (aliases: `bs`, `bspoof`)

### General

| Command | Permission | Description |
|---------|-----------|-------------|
| `/bs` | `bluespoof.info` | Opens the admin GUI (if enabled) or shows plugin info |
| `/bs help` | `bluespoof.help` | Show the help message |
| `/bs reload` | `bluespoof.reload` | Reload all configurations (disconnects all fake players) |

### Fake Player Management

| Command | Permission | Description |
|---------|-----------|-------------|
| `/bs connect [amount\|name]` | `bluespoof.connect` | Connect fake players by amount or specific name |
| `/bs disconnect [amount\|all\|name]` | `bluespoof.disconnect` | Disconnect fake players |
| `/bs list (fake\|real)` | `bluespoof.list` | List online fake or real players |
| `/bs check [player]` | `bluespoof.check` | Check whether a player is fake or real |

### Chat & Control

| Command | Permission | Description |
|---------|-----------|-------------|
| `/bs chat [name] [message]` | `bluespoof.chat` | Send a chat message as a fake player |
| `/bs sudo [name] [command]` | `bluespoof.sudo` | Force a fake player to execute a command |
| `/bs control [name]` | `bluespoof.control` | Take direct control of a fake player |
| `/bs control stop` | `bluespoof.control` | Stop controlling a fake player |

### Trace Module

| Command | Permission | Description |
|---------|-----------|-------------|
| `/bs trace stick` | `bluespoof.trace` | Get the Magic Stick for region selection |
| `/bs trace region add [name] (x1 y1 z1 x2 y2 z2)` | `bluespoof.trace` | Add a trace region (use stick or coords) |
| `/bs trace region remove [name]` | `bluespoof.trace` | Remove a trace region |
| `/bs trace region list` | `bluespoof.trace` | List all trace regions |
| `/bs trace recordings list` | `bluespoof.trace` | List all stored recordings |
| `/bs trace recordings delete [id]` | `bluespoof.trace` | Delete a recording by ID |
| `/bs trace play [bot] [id] (loop)` | `bluespoof.trace` | Make a bot replay a recording |
| `/bs trace stop [bot]` | `bluespoof.trace` | Stop a bot's playback |

### Movement Module

| Command | Permission | Description |
|---------|-----------|-------------|
| `/bs movement walk [bot] here\|<x> <y> <z>` | `bluespoof.movement` | Walk a bot to a location |
| `/bs movement sprint [bot] here\|<x> <y> <z>` | `bluespoof.movement` | Sprint a bot to a location |
| `/bs movement sneak [bot] here\|<x> <y> <z>` | `bluespoof.movement` | Sneak a bot to a location |
| `/bs movement roam [bot]` | `bluespoof.movement` | Start autonomous roaming |
| `/bs movement stop [bot]` | `bluespoof.movement` | Stop all movement for a bot |
| `/bs movement status (bot)` | `bluespoof.movement` | Show controller state(s) |
| `/bs movement roamall` | `bluespoof.movement` | Start roaming for all online fake players |
| `/bs movement stopall` | `bluespoof.movement` | Stop movement for all active controllers |
| `/bs movement isolate [bot]` | `bluespoof.movement` | Move a fake player to the isolation world |
| `/bs movement autoroam [on\|off\|reset]` | `bluespoof.movement` | Toggle auto-roam globally |
| `/bs movement autoroam [bot] [on\|off\|reset]` | `bluespoof.movement` | Toggle auto-roam for a specific bot |

---

## Permissions

### Player Permissions

| Permission | Description |
|------------|-------------|
| `bluespoof.info` | Use `/bs` command |
| `bluespoof.help` | Use `/bs help` |
| `bluespoof.connect` | Connect fake players |
| `bluespoof.disconnect` | Disconnect fake players |
| `bluespoof.list` | List fake/real players |
| `bluespoof.check` | Check if a player is fake |
| `bluespoof.chat` | Send chat as a fake player |
| `bluespoof.sudo` | Force fake players to run commands |
| `bluespoof.control` | Take control of fake players |
| `bluespoof.trace` | Use all Trace module commands |
| `bluespoof.movement` | Use all Movement module commands |

### Admin Permissions

| Permission | Description |
|------------|-------------|
| `bluespoof.admin` | All admin commands |
| `bluespoof.reload` | Reload the plugin |
| `bluespoof.*` | Full plugin access |

---

## Command Aliases

- `/bluespoof`
- `/bs`
- `/bspoof`
