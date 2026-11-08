# Commands and Permissions

Complete reference for all Blue Arcade commands and permissions.

**Notation:**
- `[required]` - Required parameter
- `(optional)` - Optional parameter
- `<option1|option2>` - Choose one option

---

## Player Commands

Main command: `/arcade` (aliases: `/ba`, `/barcade`, `/bluearcade`, `/pg`, `/partygames`)

Base permission for `/ba`: `bluearcade.info` or `bluearcade.*`

### General

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba` | `bluearcade.info` | Display plugin information |
| `/ba help` | `bluearcade.help` | Show available player commands |
| `/ba lang (locale)` | `bluearcade.info` | Show or change your language |

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

### Replays

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ba replay` | `bluearcade.replay.view.own` | Open the replay browser, or the replay of your current arena |
| `/ba replay [category]` | `bluearcade.replay.view.own` | List the replays of a category |
| `/ba replay [category] [replay_id]` | `bluearcade.replay.view.own` | Watch a replay |
| `/ba replay leave` | `bluearcade.replay.view.own` | Leave the replay you are watching |

See [Replay System](replay-system.md).

---

## Admin Commands

Main command: `/arcadeadmin` (aliases: `/baa`, `/barcadea`, `/bluearcadeadmin`)

All admin commands require the `bluearcade.admin` or `bluearcade.admin.*` permission.

### Setup Syntax (3.6.0)

Setup commands come in two syntaxes:

- **Contextual (new, recommended)**: select the arena (and game) once, then run actions without repeating IDs. `/baa help` and tab completion use this syntax. The selection is per-player and is cleared with `/baa arena clear` or on logout.
- **Classic**: every command carries the arena and minigame IDs. Still fully supported; `/baa help classic` lists it.

| Contextual | Classic equivalent |
|------------|--------------------|
| `/baa arena select 3` | (no equivalent; the ID is repeated per command) |
| `/baa arena name Party` | `/baa arena 3 setname Party` |
| `/baa game select spleef` + `/baa game time 120` | `/baa game 3 spleef time 2` |

Any command whose second argument is a number is treated as classic. Explicit targets can also be passed with `--arena <id>` / `--game <id>` flags in the contextual syntax.

> Note: `/baa game time` takes **seconds** (1-3600) in the contextual syntax; the classic `time` takes **minutes** (1-60).

### Arena Management

| Command | Description |
|---------|-------------|
| `/baa` | Show admin help |
| `/baa help` | Display the contextual setup help |
| `/baa help classic` | Display the classic admin command list |
| `/baa create [id] <standalone\|party> [dynamic\|virtual]` | Create a new arena (default type: dynamic) |
| `/baa delete [id] (confirm)` | Delete an arena |
| `/baa enable [id]` | Enable an arena |
| `/baa disable [id] (confirm)` | Disable an arena |
| `/baa clone [original_id] [new_id]` | Copy a dynamic arena under a new id (not for static arenas) |
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

### Arena Configuration (Contextual)

Select the arena first with `/baa arena select [id]` (selection only) or `/baa arena edit [id]` (opens the lobby template for editing).

| Command | Description |
|---------|-------------|
| `/baa arena` | Show your current selection and the arena help |
| `/baa arena select [id]` / `/baa arena clear` | Select or clear the current arena |
| `/baa arena edit (id)` / `save` / `exit` | Open, save or leave the lobby template editor |
| `/baa arena status (auto <on\|off>)` | Setup checklist, optionally self-refreshing |
| `/baa arena create [id] <standalone\|party> [dynamic\|virtual]` | Create an arena |
| `/baa arena delete [id] (confirm)` | Delete an arena |
| `/baa arena migrate [id] [dynamic\|virtual] (confirm)` | Convert static → dynamic or dynamic ↔ virtual (console only) |
| `/baa arena name [text]` | Set display name |
| `/baa arena mode <standalone\|party>` | Change arena mode |
| `/baa arena players <min\|max> [n]` | Set min/max players |
| `/baa arena rounds [n]` | Set rounds (3–15) |
| `/baa arena lobby <set\|skip>` | Set waiting lobby (`skip` puts it inside the game world; dynamic only) |
| `/baa arena voting <on\|off>` | Enable or disable minigame voting |
| `/baa arena order set <game1> <game2> ...` / `order clear` | Fixed rotation when voting is off |
| `/baa arena ondemand <on\|off>` | Arena On Demand instances (dynamic only) |
| `/baa arena enable` / `disable` | Enable or disable the selected arena |

### Podium & Tie-Break (Contextual)

See [Podiums & Tie-Breaks](podiums.md) for details. The arena must be selected and disabled; static arenas are rejected.

| Command | Description |
|---------|-------------|
| `/baa arena podium set <between_games\|final> <enabled\|fixed_camera\|fireworks\|winner_effects> <on\|off\|default>` | Per-arena podium option override |
| `/baa arena podium edit` / `save` / `exit` / `reset` | Custom podium map editor |
| `/baa arena podium npc <1-3>` / `camera` | Capture podium statue / camera positions |
| `/baa arena podium <round\|final> npc <1-3>` / `camera` / `reset` | Per-ceremony position overrides |
| `/baa arena tiebreak games <id...\|default>` | Tie-break minigame pool for this arena |
| `/baa arena tiebreak rounds <n\|default>` | Max extra rounds before the reaction challenge |

### Minigame Management (Contextual)

Select the game with `/baa game select [minigame]`, or open its template with `/baa game edit [minigame]`.

| Command | Description |
|---------|-------------|
| `/baa game` | Show your current selection and the game help |
| `/baa game list` | List the arena's minigames |
| `/baa game available (microgame\|minigame)` | List installable minigames |
| `/baa game add [minigame]` | Add a minigame (type auto-detected; virtual arenas filter to universal modules) |
| `/baa game remove [minigame] (confirm)` | Remove a minigame |
| `/baa game select [minigame]` / `edit (minigame)` | Select or open the minigame template |
| `/baa game enable` / `disable (confirm)` | Enable (saves template) or disable |
| `/baa game save` / `exit` / `status` | Save, leave or inspect the setup session |
| `/baa game bounds set` | Set play area boundaries |
| `/baa game spawn <add\|remove\|list\|teleport> (#)` | Manage spawn points |
| `/baa game time [seconds]` | Set duration in seconds (1–3600) |
| `/baa game deathblock [material]` | Set death block |
| `/baa game setup [module command] ...` | Pass arguments to the module's own setup commands |

Module-specific setup commands are provided by each minigame module and also appear in tab completion and the setup scoreboard.

### Classic Setup Syntax

Every classic command still works. The ID in second position selects this syntax automatically.

| Command | Description |
|---------|-------------|
| `/baa arena [id] setlobby (skip)` | Set waiting lobby |
| `/baa arena [id] setname [name]` | Set display name |
| `/baa arena [id] setmode <standalone\|party>` | Change arena mode |
| `/baa arena [id] setrounds [number]` | Set rounds (3–15) |
| `/baa arena [id] minplayers\|maxplayers [amount]` | Set player limits |
| `/baa arena [id] edit` / `save` / `exit` / `status` | Lobby template editor |
| `/baa arena [id] ondemand <true\|false>` | Arena On Demand |
| `/baa arena [id] disablevoting <true\|false>` | Disable voting |
| `/baa arena [id] setgameorder <game1,game2,...>` / `cleargameorder` | Fixed rotation |
| `/baa game [id] add <micro\|mini> <minigame\|list>` | Add a minigame |
| `/baa game [id] remove [minigame] (confirm)` / `list` | Remove or list minigames |
| `/baa game [id] [minigame] enable\|disable\|edit\|save\|exit\|status` | Minigame lifecycle |
| `/baa game [id] [minigame] bounds <set\|automatic>` | Boundaries |
| `/baa game [id] [minigame] spawn <add\|remove\|list\|teleport> (#)` | Spawns |
| `/baa game [id] [minigame] time [minutes]` | Duration in **minutes** (1–60) |
| `/baa game [id] [minigame] deathblock [material]` | Death block |

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
| `/baa module load [file]` | Load a module file, legacy `.jar` or universal `.bamodule` |
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

### Replays

| Command | Description |
|---------|-------------|
| `/baa replay list` | List every recorded replay |
| `/baa replay open [replay_id]` | Open any replay |
| `/baa replay delete [replay_id]` | Delete a replay |
| `/baa replay cleanup` | Delete replays older than the configured retention |

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
| `/baa arena migrate [id] [dynamic\|virtual] (confirm)` | Convert static → dynamic or dynamic ↔ virtual (console only; `/baa migrate` is a legacy alias) |
| `/baa upgradefiles confirm` | Run data/file upgrade migration |

### Reload Targets

- `all` - Reload everything
- `settings` - Plugin settings
- `actions` - Rewards and end of round/game commands
- `sounds` - Sound configurations
- `lang` - Language files
- `menus` - Menu definitions
- `achievements` - Achievement configurations
- `global` - Global settings
- `signs` - Sign configurations
- `arenas` - Arena configurations
- `games` - Game configurations
- `users` - User data
- `store` - Store configurations
- `data` - All data files

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
| `bluearcade.replay.view.own` | Watch replays of your own matches |
| `bluearcade.replay.view.all` | Watch any replay |
| `bluearcade.replay.admin` | Manage replays and use `/baa replay` |
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
