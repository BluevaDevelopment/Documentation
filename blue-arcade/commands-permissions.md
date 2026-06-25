# Commands and Permissions

Complete reference for all Blue Arcade commands and permissions.

**Notation:**
- `[required]` — Required parameter
- `(optional)` — Optional parameter
- `<option1|option2>` — Choose one option

---

## Player Commands

Main command: `/arcade` (aliases: `/ba`, `/barcade`, `/bluearcade`, `/pg`, `/partygames`)

Base permission for `/ba`: `bluearcade.info` or `bluearcade.*`

### General

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba` | `bluearcade.info` | Display plugin information |
| `/ba help` | `bluearcade.help` | Show available player commands |

### Arena

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba join [arena_id \| filter \| server:arena_id] (player)` | `bluearcade.join`<br>`bluearcade.join.others` | Join a specific arena, remote proxy arena, or open the join menu |
| `/ba leave (confirm \| cancel \| player)` | `bluearcade.leave`<br>`bluearcade.leave.others` | Leave current arena |
| `/ba quickjoin (selector)` | `bluearcade.quickjoin`<br>`bluearcade.quickjoin.others` | Join the best available arena. Optional selector: `party` or a module ID |
| `/ba vote [minigame \| page <n>]` | `bluearcade.vote` | Vote for a minigame or open the vote menu |
| `/ba team (join <team_id>)` | `bluearcade.team` | Team management in team-based games |

### Party

All party commands require `bluearcade.party`.

| Command | Description |
|---------|-------------|
| `/ba party` or `/ba party menu` | Open the party main menu |
| `/ba party create` | Create a new private party |
| `/ba party invite [player]` | Invite a player |
| `/ba party accept [leader]` | Accept a party invitation |
| `/ba party reject [leader]` | Reject a party invitation |
| `/ba party kick [player]` | Remove a member (leader only) |
| `/ba party leader [player]` | Transfer leadership |
| `/ba party type <public\|private>` | Set party visibility |
| `/ba party leave` | Leave the party |
| `/ba party list` | List party members in chat |
| `/ba party members (page)` | Open the members menu |
| `/ba party search (page)` | Search public parties |
| `/ba party join [leader]` | Join a public party |

### Store

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba store` | `bluearcade.store` | Open the store menu |
| `/ba store page [number]` | `bluearcade.store` | Navigate store pages |
| `/ba store module [module_id]` | `bluearcade.store` | Open store for a specific module's cosmetics |
| `/ba store category [id] (page)` | `bluearcade.store` | View a category |
| `/ba store buy [category] [item]` | `bluearcade.store` | Purchase an item |
| `/ba store select [category] [item]` | `bluearcade.store` | Select an owned item |
| `/ba credits (pay <player> <amount>)` | `bluearcade.credits` | View balance or pay credits to another player |

### Stats & Progress

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba stats (category) (period) (page)` | `bluearcade.stats` | View your statistics |
| `/ba tops (category \| stat) (period) (page)` | `bluearcade.stats` | View leaderboards |
| `/ba achievements (category) (achievement) (page)` | `bluearcade.achievements` | View achievements |
| `/ba achievements progress` | `bluearcade.achievements` | Show queued achievement summary |
| `/ba level` | `bluearcade.level` | View your level and XP |

**Stat Periods:** `alltime`, `daily`, `weekly`, `monthly`, `yearly`

---

## Admin Commands

Main command: `/arcadeadmin` (aliases: `/baa`, `/barcadea`, `/bluearcadeadmin`)

All admin commands require the `bluearcade.admin` or `bluearcade.admin.*` permission.

### Arena Management

| Command | Description |
|---------|-------------|
| `/baa` | Show admin help |
| `/baa help` | Display all admin commands |
| `/baa create [id] <standalone\|party> [dynamic\|static]` | Create a new arena (default: dynamic) |
| `/baa delete [id] (confirm)` | Delete an arena |
| `/baa enable [id]` | Enable an arena |
| `/baa disable [id] (confirm)` | Disable an arena |
| `/baa list` | List all arenas |
| `/baa info [id]` | Display arena information |
| `/baa setmainlobby` | Set main lobby location |
| `/baa mainlobby <set\|clear\|goto\|dedicated <true\|false>>` | Manage the dedicated main lobby |

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
| `/baa arena [id] setlobby [skip]` | Set waiting lobby (`skip` puts it inside the game world) |
| `/baa arena [id] setname [name]` | Set display name |
| `/baa arena [id] setmode <standalone\|party>` | Change arena mode |
| `/baa arena [id] settype <static\|dynamic>` | Change arena type |
| `/baa arena [id] setrounds [number]` | Set rounds (3–15) |
| `/baa arena [id] minplayers [amount]` | Set min players (0 = admin-managed, otherwise 2+) |
| `/baa arena [id] maxplayers [amount]` | Set max players (2+) |
| `/baa arena [id] edit` | Reopen a dynamic arena lobby template for editing |
| `/baa arena [id] ondemand <true\|false>` | Enable Arena On Demand instances |
| `/baa arena [id] disablevoting <true\|false>` | Disable minigame voting |
| `/baa arena [id] setgameorder <game1,game2,...>` | Fixed rotation when voting is disabled |
| `/baa arena [id] cleargameorder` | Clear fixed rotation |

### Minigame Management

| Command | Description |
|---------|-------------|
| `/baa game [id] add <micro\|mini> <minigame\|list>` | Add a minigame |
| `/baa game [id] remove [minigame] (confirm)` | Remove a minigame |
| `/baa game [id] list` | List arena minigames |
| `/baa game [id] [minigame] enable` | Enable a minigame and save dynamic template |
| `/baa game [id] [minigame] disable (confirm)` | Disable a minigame |
| `/baa game [id] [minigame] edit` | Reopen a dynamic minigame template for editing |

### Common Minigame Setup

| Command | Description |
|---------|-------------|
| `/baa game [id] [minigame] bounds set` | Set play area boundaries |
| `/baa game [id] [minigame] spawn add` | Add spawn point |
| `/baa game [id] [minigame] spawn remove [#]` | Remove spawn point |
| `/baa game [id] [minigame] spawn list` | List spawn points |
| `/baa game [id] [minigame] spawn teleport [#]` | Teleport to spawn |
| `/baa game [id] [minigame] time [minutes]` | Set duration (1–60) |
| `/baa game [id] [minigame] deathblock [material]` | Set death block |

Module-specific setup commands are provided by each minigame module and also appear in tab completion and the setup scoreboard.

### Signs

You must be looking at an empty sign block when using these commands.

| Command | Description |
|---------|-------------|
| `/baa signs set join [arena_id\|server:arena_id]` | Register a join sign |
| `/baa signs set quickjoin` | Register a quickjoin sign |
| `/baa signs set auto [all\|party\|standalone\|game_id]` | Register an auto-join sign |
| `/baa signs set top [stat\|module:stat] [position] [alltime\|monthly\|yearly]` | Register a top leaderboard sign |

### Module Management

| Command | Description |
|---------|-------------|
| `/baa module list` | List loaded modules |
| `/baa module info [module_id]` | Show module details |
| `/baa module load [file]` | Load a module JAR |
| `/baa module unload [module_id]` | Unload a module |
| `/baa module reload` | Reload all modules |
| `/baa module delete [module_id] (confirm)` | Delete a module permanently |

### Module Store

| Command | Description |
|---------|-------------|
| `/baa module store search (query) (page <n>)` | Search modules in the store |
| `/baa module store originals` | List official Blueva modules |
| `/baa module store download [module_id]` | Download and install a module |
| `/baa module store update [module_id] (--force)` | Update a module to latest version |

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
| `/baa upgradefiles confirm` | Run data/file upgrade migration |

### Reload Targets

- `all` — Reload everything
- `settings` — Plugin settings
- `rewards` — Reward configurations
- `sounds` — Sound configurations
- `lang` — Language files
- `menus` — Menu definitions
- `achievements` — Achievement configurations
- `global` — Global settings
- `signs` — Sign configurations
- `arenas` — Arena configurations
- `games` — Game configurations
- `users` — User data
- `store` — Store configurations
- `data` — All data files

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
| `bluearcade.vote` | Vote for minigames |
| `bluearcade.party` | Access party system |
| `bluearcade.store` | Access the store |
| `bluearcade.credits` | View/pay credits |
| `bluearcade.stats` | View statistics and leaderboards |
| `bluearcade.achievements` | View achievements |
| `bluearcade.level` | View level and XP |
| `bluearcade.team` | Use team commands |
| `bluearcade.votes.[number]` | Number of votes per arena |
| `bluearcade.*` | Full player access |

### Admin Permissions

| Permission | Description |
|------------|-------------|
| `bluearcade.admin` | All admin commands and in-arena command bypass |
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
- `/barcade`
- `/ba`
- `/pg`
- `/partygames`

### Admin Command Aliases
- `/arcadeadmin`
- `/bluearcadeadmin`
- `/baa`
- `/barcadea`
