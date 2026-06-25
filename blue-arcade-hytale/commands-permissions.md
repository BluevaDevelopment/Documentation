# Commands and Permissions

Complete reference for all Blue Arcade 3.0 commands and permissions.

**Notation:**
- `[required]` - Required parameter
- `(optional)` - Optional parameter
- `<option1|option2>` - Choose one option

---

## Player Commands

Main command: `/arcade` (aliases: `/ba`, `/bluearcade`)

### General

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba` | `bluearcade.info` | Display plugin information |
| `/ba help` | `bluearcade.help` | Show available commands |

### Arena

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba join [arena_id] (player)` | `bluearcade.join`<br>`bluearcade.join.others` | Join a specific arena |
| `/ba leave (player)` | `bluearcade.leave`<br>`bluearcade.leave.others` | Leave current arena |
| `/ba quickjoin (player)` | `bluearcade.quickjoin`<br>`bluearcade.quickjoin.others` | Join any available arena |
| `/ba vote [minigame]` | `bluearcade.vote` | Vote for a minigame |
| `/ba team` | `bluearcade.team` | Team management |

### Party

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba party` | `bluearcade.party` | Open party menu |
| `/ba party create` | `bluearcade.party` | Create a new party |
| `/ba party invite [player]` | `bluearcade.party` | Invite a player |
| `/ba party accept [leader]` | `bluearcade.party` | Accept party invitation |
| `/ba party reject [leader]` | `bluearcade.party` | Reject party invitation |
| `/ba party kick [player]` | `bluearcade.party` | Remove a member (leader only) |
| `/ba party leader [player]` | `bluearcade.party` | Transfer leadership |
| `/ba party type <public\|private>` | `bluearcade.party` | Set party visibility |
| `/ba party leave` | `bluearcade.party` | Leave the party |
| `/ba party list` | `bluearcade.party` | List party members |
| `/ba party members (page)` | `bluearcade.party` | View members menu |
| `/ba party search (page)` | `bluearcade.party` | Search public parties |
| `/ba party join [leader]` | `bluearcade.party` | Join a public party |

### Store

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba credits` | - | View your credits balance |

### Stats & Progress

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba stats (category) (period) (page)` | `bluearcade.stats` | View your statistics |
| `/ba tops (category) (period) (page)` | `bluearcade.stats` | View leaderboards |
| `/ba tops [stat] [period] (page)` | `bluearcade.stats` | View specific stat ranking |
| `/ba level` | `bluearcade.level` | View your level and XP |
| `/ba achievements (category) (page)` | `bluearcade.achievements` | View achievements |

**Stat Periods:** `alltime`, `daily`, `weekly`, `monthly`, `yearly`

---

## Admin Commands

Main command: `/arcadeadmin` (aliases: `/baa`, `/bluearcadeadmin`)

All admin commands require the `bluearcade.admin` permission.

### Arena Management

| Command | Description |
|---------|-------------|
| `/baa` | Show admin help |
| `/baa help` | Display all admin commands |
| `/baa create [id] <standalone\|party>` | Create a new arena |
| `/baa delete [id] (confirm)` | Delete an arena |
| `/baa enable [id]` | Enable an arena |
| `/baa disable [id]` | Disable an arena |
| `/baa list` | List all arenas |
| `/baa info [id]` | Display arena information |
| `/baa setmainlobby` | Set main lobby location |
| `/baa goto [id] [minigame]` | Teleport to minigame spawn |

### Selection Tools

| Command | Description |
|---------|-------------|
| `/baa stick` | Get the Magic Stick tool |
| `/baa pos1` | Set position 1 at current location |
| `/baa pos2` | Set position 2 at current location |
| `/baa clearselection` | Clear your selection |

### Arena Configuration

| Command | Description |
|---------|-------------|
| `/baa arena [id] setlobby` | Set waiting lobby |
| `/baa arena [id] setname [name]` | Set display name |
| `/baa arena [id] setmode <standalone\|party>` | Change arena mode |
| `/baa arena [id] setrounds [number]` | Set rounds (3-15) |
| `/baa arena [id] minplayers [amount]` | Set min players (2-64) |
| `/baa arena [id] maxplayers [amount]` | Set max players (2-64) |

### Minigame Management

| Command | Description |
|---------|-------------|
| `/baa game [id] add [minigame]` | Add a minigame |
| `/baa game [id] remove [minigame] (confirm)` | Remove a minigame |
| `/baa game [id] list` | List arena minigames |
| `/baa game [id] [minigame] enable` | Enable a minigame |
| `/baa game [id] [minigame] disable` | Disable a minigame |

### Common Minigame Setup

| Command | Description |
|---------|-------------|
| `/baa game [id] [minigame] bounds set` | Set play area boundaries |
| `/baa game [id] [minigame] spawn add` | Add spawn point |
| `/baa game [id] [minigame] spawn remove [#]` | Remove spawn point |
| `/baa game [id] [minigame] spawn list` | List spawn points |
| `/baa game [id] [minigame] spawn teleport [#]` | Teleport to spawn |
| `/baa game [id] [minigame] time [minutes]` | Set duration (1-60) |
| `/baa game [id] [minigame] deathblock [material]` | Set death block |

### Economy & XP

| Command | Description |
|---------|-------------|
| `/baa credits give [player] [amount] (-s)` | Give credits to player |
| `/baa xp give [player] [amount] (-s)` | Give XP to player |

**Note:** Use `-s` flag for silent mode (no notification to target).

### Utilities

| Command | Description |
|---------|-------------|
| `/baa forcestart (id)` | Force start an arena |
| `/baa forcestop (id)` | Force stop a running arena |
| `/baa reload <target>` | Reload configuration |

### Signs

> **Not available in Hytale Edition.** Hytale does not support block signs. This feature is Minecraft-only.

**Reload Targets:**
- `all` - Reload everything
- `lang` - Language files
- `arenas` - Arena configurations
- `settings` - Plugin settings
- `rewards` - Reward configurations
- `signs` - Sign configurations
- `global` - Global settings
- `games` - Game configurations
- `users` - User data
- `data` - All data files

### Module Management

| Command | Description |
|---------|-------------|
| `/baa module list` | List all available modules |
| `/baa module info [module_id]` | Show module details |
| `/baa module load [module_id]` | Load a module |
| `/baa module unload [module_id]` | Unload a module |
| `/baa module reload [module_id]` | Reload a module |
| `/baa module delete [module_id] (confirm)` | Delete a module permanently |

### Module Store

| Command | Description |
|---------|-------------|
| `/baa module store search (query)` | Search modules in the store |
| `/baa module store originals (query)` | List official Blueva modules |
| `/baa module store download [module_id]` | Download and install a module |
| `/baa module store update [module_id] (--force)` | Update a module to latest version |

---

## Permissions

### Player Permissions

| Permission | Description |
|------------|-------------|
| `bluearcade.info` | Use `/ba` command |
| `bluearcade.help` | Use `/ba help` |
| `bluearcade.join` | Join arenas |
| `bluearcade.join.others` | Force other players to join |
| `bluearcade.leave` | Leave arenas |
| `bluearcade.leave.others` | Force other players to leave |
| `bluearcade.quickjoin` | Use quick join |
| `bluearcade.quickjoin.others` | Quick join for others |
| `bluearcade.party` | Access party system |
| `bluearcade.stats` | View statistics and leaderboards |
| `bluearcade.achievements` | View achievements |
| `bluearcade.level` | View level and XP |
| `bluearcade.vote` | Vote for minigames |
| `bluearcade.team` | Use team commands |
| `bluearcade.votes.[number]` | Number of votes per arena |

### Admin Permissions

| Permission | Description |
|------------|-------------|
| `bluearcade.admin` | All admin commands |
| `bluearcade.admin.*` | All admin subcommands |
| `bluearcade.*` | Full plugin access |

### Vote Permissions

Control how many times a player can vote per arena:

```
bluearcade.votes.1   # 1 vote
bluearcade.votes.3   # 3 votes
bluearcade.votes.5   # 5 votes
```

---

## Command Aliases

### Player Command Aliases
- `/arcade`
- `/bluearcade`
- `/ba`

### Admin Command Aliases
- `/arcadeadmin`
- `/bluearcadeadmin`
- `/baadmin`
- `/barcadea`
- `/baa`
