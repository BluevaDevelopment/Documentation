# Commands and Permissions

## User Commands

Commands for regular players to join and interact with arenas.

**Note:** `[]` Required --- `()` Optional

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba` | `bluearcade.info` | Displays plugin information. |
| `/ba help` | `bluearcade.help` | Shows available player commands. |
| `/ba join [arena_id] (player)` | `bluearcade.join`<br>`bluearcade.join.others` (for player arg) | Join a specific arena by ID. |
| `/ba leave (player)` | `bluearcade.leave`<br>`bluearcade.leave.others` (for player arg) | Leave the current arena. |
| `/ba quickjoin (player)` | `bluearcade.quickjoin`<br>`bluearcade.quickjoin.others` (for player arg) | Join any available arena quickly. |
| `-` | `bluearcade.votes.[number]` | Number of times a player can vote per arena. |

### User Command Aliases
- `/bluearcade`
- `/partygames`
- `/arcade`
- `/ba`
- `/pg`

---

## Admin Commands

Commands for server administrators to manage arenas and minigames.

**Note:** `[]` Required --- `()` Optional --- `<>` Choose one

### Basic Arena Management

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa` | `bluearcade.admin` | Shows admin command help. |
| `/baa help` | `bluearcade.admin` | Displays all admin commands. |
| `/baa create [id] <standalone\|party>` | `bluearcade.admin` | Create a new arena with specified mode. |
| `/baa delete [id] (confirm)` | `bluearcade.admin` | Delete an existing arena. |
| `/baa enable [id]` | `bluearcade.admin` | Enable an arena for players to join. |
| `/baa disable [id]` | `bluearcade.admin` | Disable an arena temporarily. |
| `/baa list` | `bluearcade.admin` | List all created arenas. |
| `/baa info [id]` | `bluearcade.admin` | Display detailed arena information. |

### Selection Tools

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa stick` | `bluearcade.admin` | Get the Magic Stick selection tool. |
| `/baa pos1` | `bluearcade.admin` | Set position 1 at current location. |
| `/baa pos2` | `bluearcade.admin` | Set position 2 at current location. |
| `/baa clearselection` | `bluearcade.admin` | Clear your current selection. |

### Arena Configuration

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa arena [id] setlobby` | `bluearcade.admin` | Set waiting lobby at your position. |
| `/baa arena [id] setname [name]` | `bluearcade.admin` | Set arena display name (supports colors). |
| `/baa arena [id] setmode <standalone\|party>` | `bluearcade.admin` | Change arena mode. |
| `/baa arena [id] setrounds [number]` | `bluearcade.admin` | Set number of rounds (3-15, party mode only). |
| `/baa arena [id] minplayers [amount]` | `bluearcade.admin` | Set minimum players (2-64). |
| `/baa arena [id] maxplayers [amount]` | `bluearcade.admin` | Set maximum players (2-64). |

### Minigame Management

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] add [minigame]` | `bluearcade.admin` | Add a minigame to the arena. |
| `/baa game [id] remove [minigame] (confirm)` | `bluearcade.admin` | Remove a minigame from the arena. |
| `/baa game [id] list` | `bluearcade.admin` | List all minigames in the arena. |
| `/baa game [id] [minigame] enable` | `bluearcade.admin` | Enable a configured minigame. |
| `/baa game [id] [minigame] disable` | `bluearcade.admin` | Disable a minigame. |

### Minigame Configuration (Common)

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] [minigame] bounds set` | `bluearcade.admin` | Set game boundaries using selection. |
| `/baa game [id] [minigame] spawn add` | `bluearcade.admin` | Add spawn point at your position. |
| `/baa game [id] [minigame] spawn remove [#]` | `bluearcade.admin` | Remove a spawn point by number. |
| `/baa game [id] [minigame] spawn list` | `bluearcade.admin` | List all spawn points. |
| `/baa game [id] [minigame] spawn teleport [#]` | `bluearcade.admin` | Teleport to a spawn point. |
| `/baa game [id] [minigame] time [minutes]` | `bluearcade.admin` | Set game duration (1-60 minutes). |
| `/baa game [id] [minigame] deathblock [material]` | `bluearcade.admin` | Set death block material. |

### Minigame-Specific Configuration

#### Race, Traffic Light, Mine Field, Fast Zone
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] [minigame] finishline set` | `bluearcade.admin` | Set finish line using selection. |

#### Spleef, Red Alert, Mine Field, Block Party
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] [minigame] floor set` | `bluearcade.admin` | Set game floor using selection. |

#### Block Party
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] block_party pattern add` | `bluearcade.admin` | Add a new floor pattern. |
| `/baa game [id] block_party pattern remove [#]` | `bluearcade.admin` | Remove a pattern by number. |
| `/baa game [id] block_party pattern list` | `bluearcade.admin` | List all patterns. |
| `/baa game [id] block_party pattern view [#]` | `bluearcade.admin` | Preview a pattern. |
| `/baa game [id] block_party musictime [seconds]` | `bluearcade.admin` | Set music duration (5-30 seconds). |
| `/baa game [id] block_party searchtime [seconds]` | `bluearcade.admin` | Set search time (3-20 seconds). |
| `/baa game [id] block_party decreasetime [seconds]` | `bluearcade.admin` | Set time decrease per round (0.1-3.0). |

#### Run From The Beast
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] run_from_the_beast beastspawn` | `bluearcade.admin` | Set beast spawn at your position. |
| `/baa game [id] run_from_the_beast beastzone set` | `bluearcade.admin` | Set beast zone using selection. |

#### Exploding Sheep
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] exploding_sheep explosionlevel [level]` | `bluearcade.admin` | Set explosion power (0.5-10.0). |

#### One In The Chamber
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] one_in_the_chamber removearrows <true\|false>` | `bluearcade.admin` | Toggle arrow removal on hit. |

#### Red Alert
| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa game [id] red_alert particles <true\|false>` | `bluearcade.admin` | Toggle floor particles. |
| `/baa game [id] red_alert colorchance [percentage]` | `bluearcade.admin` | Set color change chance (0.1-10.0). |
| `/baa game [id] red_alert increasechance [amount]` | `bluearcade.admin` | Set chance increase per second (0-5). |

### Utilities

| Command | Permission | Description |
|---------|-----------|-------------|
| `/baa goto [id] [minigame]` | `bluearcade.admin` | Teleport to minigame spawn #1. |
| `/baa setmainlobby` | `bluearcade.admin` | Set main lobby at your position. |
| `/baa forcestart (id)` | `bluearcade.admin` | Force start an arena immediately. |
| `/baa forcestop (id)` | `bluearcade.admin` | Force stop a running arena. |
| `/baa reload <all\|lang\|arenas\|settings\|rewards\|signs\|global\|games\|users\|data>` | `bluearcade.admin` | Reload configuration files. |

### Admin Command Aliases
- `/bluearcadeadmin`
- `/baa`