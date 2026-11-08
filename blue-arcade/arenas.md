# Creating Arenas

This guide covers arena creation and configuration in detail. Blue Arcade uses **dynamic arenas by default**, and this documentation focuses on the modern dynamic workflow. 3.6.0 also adds an experimental [virtual arena](virtual-arenas.md) type. Static arenas are legacy: since 3.6.0 they can no longer be edited or cloned, and they will stop working in **3.7.0**. Migrate them with `/baa arena migrate [id] dynamic` from the console.

## Before You Start

We recommend:

- **Enable your server whitelist** while configuring.
- **Plan your arenas**: decide which minigames go in each arena, max player counts, and visual theme.
- **Use void worlds** for dynamic setup worlds. The plugin creates them automatically; you only build what the players will see.
- **Install WorldEdit** if you want to import schematics or copy large structures into setup worlds.

## Admin Command Syntax

Since 3.6.0 there are two ways to run setup commands:

- **New syntax (recommended)**: select the arena once, then configure it without repeating IDs:
  ```
  /baa arena select 3
  /baa arena name Mega Arena
  /baa game add spleef
  /baa game select spleef
  /baa game time 120
  ```
  Help (`/baa help`) and tab completion use this syntax. `/baa arena` or `/baa game` alone prints your current selection.
- **Classic syntax**: every command carries the arena (and minigame) ID, exactly as before:
  ```
  /baa arena 3 setname Mega Arena
  /baa game 3 add micro spleef
  /baa game 3 spleef time 2
  ```
  Old commands still work everywhere; `/baa help classic` lists them.

This guide shows the new syntax. One difference to remember: in the new syntax `/baa game time` takes **seconds**, while the classic `/baa game [id] [minigame] time` takes **minutes**.

## Arena Types

### Dynamic Arena (Recommended, Default)

- Each match runs in a **fresh temporary world** copied from a saved template.
- The waiting lobby and each minigame have their own template ZIP.
- No regeneration is needed - the world is deleted and recreated for every match.
- Supports **Arena On Demand** for automatic extra instances.
- Supports **AdvancedSlimePaper** for faster in-memory runtime worlds.
- Supports the [replay system](replay-system.md).

### Virtual Arena (Experimental)

- Matches are rendered on the client with packets: no world is loaded per match.
- Requires a compatible **Paper** server and **universal modules** for every minigame.
- Convertible to and from dynamic with `/baa arena migrate [id] <dynamic|virtual>`.
- Performance is still being improved; **dynamic arenas remain the recommendation**. See [Virtual Arenas](virtual-arenas.md).

### Static Arena (Legacy, Read-Only)

- Uses a persistent world you built or imported yourself.
- Only the configured game region is restored between matches using block snapshots.
- **New static arenas can no longer be created since 3.5.0.**
- **Since 3.6.0 they cannot be edited or cloned either**: every setup command on a static arena is rejected until you migrate it to dynamic. Existing matches still work.
- **Support ends in 3.7.0.** Migrate them from the console with `/baa arena migrate [id] dynamic`.

## Arena Modes

### Standalone Mode

- Single minigame per arena.
- Players play the same game repeatedly.
- Best for dedicated servers (for example, a "Spleef Server").

### Party Mode

- Multiple minigames with rounds.
- Players vote for the next minigame or follow a fixed order.
- Players earn stars across rounds.
- Winner is determined by total stars.

## Creating an Arena

### Step 1: Create

```
/baa arena create [id] <standalone|party> [dynamic|virtual]
```

**Examples:**
```
/baa arena create 1 party
/baa arena create 2 standalone
/baa arena create 3 party virtual
```

Without a type the arena is **dynamic**. The type can only be `dynamic` or `virtual`; static arenas can no longer be created. (Classic form: `/baa create [id] <standalone|party> [dynamic|virtual]`.)

**Tips:**
- Use sequential numbers (1, 2, 3...).
- IDs must be unique.
- The ID cannot be changed later.
- For dynamic arenas, the plugin creates a temporary lobby world and teleports you into it in Creative mode.

### Step 2: Set Display Name

Select the arena once; the selection is per-player and kept until you clear it or log out:

```
/baa arena select [id]
/baa arena name [name]
```

**Examples:**
```
/baa arena select 1
/baa arena name Party Games Arena
/baa arena name <green><bold>Mega Arena</bold></green>
```

Names use **MiniMessage** formatting. Examples:
- `<green>Green Text</green>`
- `<bold>Bold</bold>`
- `<gradient:red:blue>Gradient</gradient>`

Legacy `&` color codes are also accepted in most places.

### Step 3: Set Waiting Lobby

In a dynamic arena, stand in the temporary lobby world at your desired location and run:

```
/baa arena lobby set
```

**Important:**
- Your position **and** viewing direction are saved.
- Players spawn facing the same direction.

If you want the waiting lobby to be inside the minigame world instead of a separate world (useful for standalone arenas), use:

```
/baa arena lobby skip
```

### Step 4: Configure Player Limits

```
/baa arena players min [amount]
/baa arena players max [amount]
```

**Requirements:**
- Minimum: `0` (admin-managed) or `2+`.
- Maximum: `2+`.
- Minimum cannot exceed maximum.
- There is no hard 64-player cap; set limits that make sense for your hardware.

### Step 5: Set Rounds (Party Mode Only)

```
/baa arena rounds [number]
```

**Requirements:**
- Between 3–15 rounds.
- Default: configured in `settings.yml` (`game.global.default_rounds_per_arena`, default 8).

## Dynamic Arena Setup Flow

A typical dynamic arena setup looks like this:

1. `/baa arena create 3 party`
   - Teleports you to `ba-temporal-3-lobby` in Creative mode.
2. Build the waiting lobby.
3. `/baa arena select 3`, then `/baa arena lobby set`
   - Saves the waiting-lobby spawn location.
4. `/baa game add spleef`
   - Saves the lobby template, creates `ba-temporal-3-spleef`, teleports you in.
5. Build the spleef map.
6. Use `/baa stick` + `/baa pos1`/`/baa pos2` to select the play area, then:
   - `/baa game bounds set`
7. `/baa game spawn add` (repeat for all spawns).
8. `/baa game time 300` (seconds; the classic command uses minutes)
9. `/baa game enable`
   - Saves the spleef world ZIP and returns you to the lobby.
10. Repeat steps 4–9 for more minigames (party mode).
11. `/baa arena enable`
    - Saves the final lobby template and enables the arena.

### Setup Scoreboard and Boss Bar

While editing a dynamic arena, admins see:

- A **boss bar** showing the current phase (lobby, game, or ready).
- A **sidebar scoreboard** updated every second showing:
  - Arena ID and current phase.
  - Required setup checklist: name, player limits, lobby, configured games.
  - For the current minigame: bounds, spawns, time, and module-specific requirements.
  - Done/required/optional status.

These are cleared when the session ends (arena enabled, disabled, or you quit).

### Editor Markers

Since 3.6.0, while an editor session is open (arena lobby, minigame, podium, or the main lobby), every configured point (spawns, cameras and other saved positions) shows a **private hologram label** with its name, coordinates and facing, plus a **colored beacon beam** to spot it from a distance. Both are packet-only and private to you: no real blocks or entities are placed, other players see nothing, and the exported template stays clean. Markers within 96 blocks are shown; holograms need Minecraft 1.19.4+ (older servers show only the beacon).

### Editing Restrictions

- While in a setup session you **cannot teleport out of the temporal world**.
- You are forced into Creative mode inside the temporal world.
- Disconnecting ends the session.
- Only one player can edit an arena at a time.

### Editing an Existing Dynamic Arena

To reopen a dynamic arena or minigame template:

```
/baa arena edit [id]        # Reopens the lobby template
/baa game edit [minigame]   # Reopens the minigame template (with the arena selected)
```

Make your changes, then save (`/baa arena save`, `/baa game save`) or re-enable the game or arena to save the updated ZIP. Static arenas cannot be edited; migrate them to dynamic first.

## Arena States

| State | Description |
|-------|-------------|
| WAITING | Waiting for players to join |
| STARTING | Enough players joined, countdown running |
| RUNNING | Game in progress |
| FINISHING | Match ended, showing results |
| RESTARTING | Resetting for the next game |

## Arena Status

| Status | Description |
|--------|-------------|
| ENABLED | Arena can be joined |
| DISABLED | Arena is not accessible |

## Managing Arenas

**List all arenas:**
```
/baa list
```

**Enable arena:**
```
/baa enable [id]
```

**Disable arena:**
```
/baa disable [id] (confirm)
```

**Delete arena:**
```
/baa delete [id] confirm
```

This deletes the arena folder (`plugins/BlueArcade3/data/arenas/<id>/`) including all templates and ZIPs.

## Changing Arena Settings

With the arena selected (`/baa arena select [id]`):

### Change Mode

```
/baa arena mode <standalone|party>
```

**Restrictions:**
- Standalone can only have one minigame.
- Remove extra minigames before switching to standalone.

### Arena On Demand

Enable automatic extra instances for busy dynamic arenas:

```
/baa arena ondemand <on|off>
```

- The base arena is instance `0`.
- When a join request cannot fit, a new virtual instance is created (`3-1`, `3-2`, etc.).
- Each instance uses the same templates and gets its own runtime world.
- Empty instances are destroyed automatically.
- Target instances in admin commands using the full key: `/baa info 3-1`, `/baa forcestop 3-2`.
- Virtual arenas do not support Arena On Demand.

### Voting and Game Order (Party Mode)

Turn voting on or off:
```
/baa arena voting <on|off>
```

Set a fixed minigame rotation when voting is off:
```
/baa arena order set <game1> <game2> ...
```

Clear the fixed order and return to random:
```
/baa arena order clear
```

Classic equivalents: `/baa arena [id] disablevoting <true|false>`, `setgameorder <game1,game2,...>`, `cleargameorder`.

### Podiums and Tie-Breaks

Party matches can run physical podium ceremonies between minigames and at match end, and ties for the top three can be broken with extra rounds:

```
/baa arena podium set <between_games|final> <option> <on|off|default>
/baa arena tiebreak games <id...|default>
/baa arena tiebreak rounds <n|default>
```

See [Podiums & Tie-Breaks](podiums.md) for the full reference, including the custom podium map editor.

## Verify Configuration

```
/baa arena status       # Setup checklist of the selected arena (self-refreshing with: /baa arena status auto on)
/baa info [id]          # Runtime information of any arena
```

`/baa arena status` shows the setup progress: name, player limits, lobby, configured games and what is still missing. `/baa info [id]` shows:
- Arena name, mode, and type
- Status (ENABLED/DISABLED) and state
- Player limits and rounds
- Configured minigames
- Lobby location
- On-demand status

## Adding Minigames

After creating and selecting an arena, add minigames:

```
/baa game add [minigame]
```

The module type (microgame or minigame) is detected automatically. To see what you can install:

```
/baa game available
```

Virtual arenas only offer [universal modules](universal-modules.md). See the module's store page for minigame-specific setup instructions.

## Common Minigame Setup

All minigames share these common configuration steps. Each module may register additional steps. Select the game you are configuring once with `/baa game select [minigame]`.

### Selection Tools

**Get the Magic Stick:**
```
/baa stick
```

**Set positions manually:**
```
/baa pos1
/baa pos2
```

**Clear selection:**
```
/baa clearselection
```

### Set Boundaries

Every minigame needs a play area:

1. Use the Magic Stick to select opposite corners.
2. Save with: `/baa game bounds set`.

### Add Spawn Points

```
/baa game spawn add      # Add at current position
/baa game spawn list     # View all spawns
/baa game spawn remove [#] # Remove spawn
/baa game spawn teleport [#] # Teleport to spawn
```

**Requirement:** Add spawns equal to or greater than max players.

Saved spawns show a private hologram and a colored beacon while you edit the map (see Editor Markers above).

### Set Duration

```
/baa game time [seconds]
```

Range: 1–3600 seconds (default: 300). The classic command `/baa game [id] [minigame] time [minutes]` still takes minutes.

### Set Death Block (Optional)

```
/baa game deathblock [material]
```

Common options: `LAVA`, `WATER`, `AIR`, `BARRIER`.

### Module-Specific Setup

Arguments that belong to the module itself go through:

```
/baa game setup [module command] ...
```

Everything after `setup` is passed to the module untouched.

### Enable Minigame

```
/baa game enable
```

For dynamic arenas, this saves the minigame world template.

### Manage Minigames

```
/baa game list                     # List all minigames of the selected arena
/baa game disable [confirm]        # Disable the selected minigame
/baa game remove [minigame] confirm # Remove a minigame
```

## Dynamic Arena File Structure

```
plugins/BlueArcade3/data/arenas/<id>/
├── arena.json
├── lobby/
│   └── world.zip
├── <minigame>/
│   ├── game.json
│   ├── world.zip
│   └── world-border.properties
└── __podium/
    ├── game.json
    └── world.zip
```

| File | Purpose |
|------|---------|
| `arena.json` | Arena metadata: mode, type, on-demand, lobby location, games list, podium and tie-break overrides |
| `lobby/world.zip` | Waiting lobby template |
| `<minigame>/game.json` | Per-minigame config: bounds, spawns, time, death block |
| `<minigame>/world.zip` | Minigame world template |
| `<minigame>/world-border.properties` | Saved world border for the template |
| `__podium/` | Custom podium map for this arena, if you built one (see [Podiums & Tie-Breaks](podiums.md)) |

Virtual arenas additionally keep a Slime conversion of each saved map (`world.slime`) next to the ZIP, used to stream the map to players.

Runtime worlds are created as `ba-dyn-<id>-<slot>` and deleted automatically. Setup worlds are created as `ba-temporal-<id>-lobby` or `ba-temporal-<id>-<minigame>` and are also cleaned up automatically. Virtual arenas use one shared empty world named `ba-virtual` and never create per-match worlds.

## Configuration Options

### Time Between Minigames

The pause between party minigames (where the between-games podium ceremony plays) defaults to **10 seconds** since 3.6.0:

```yaml
game:
  global:
    limbo_countdown: 10
```

### Automatic World Border

Dynamic arenas automatically create a tight world border around the built area during setup. You can disable this globally in `settings.yml`:

```yaml
game:
  global:
    automatic_dynamic_world_border: false
```

### Proxy World Preloading

When proxy mode is enabled on game servers, dynamic worlds for waiting arenas can be preloaded to reduce the spike when the first player joins:

```yaml
proxy_mode:
  game_server:
    preload_worlds:
      enabled: true
      initial_delay_ticks: 40
      refresh_ticks: 100
```

See [Commands & Permissions](commands-permissions.md) for the full proxy configuration reference.

## Utility Commands

**Force start arena:**
```
/baa forcestart [id]
```

**Force stop arena:**
```
/baa forcestop [id]
```

**Reload configuration:**
```
/baa reload <all|settings|actions|sounds|lang|menus|achievements|global|signs|arenas|games|users|store|data>
```

**Upgrade files after major updates:**
```
/baa upgradefiles confirm
```

## Tips for Dynamic Arenas

1. **Build small and focused** - only build what players will see; void worlds keep file sizes small.
2. **Save often** - enable each minigame as soon as its setup is complete.
3. **Test before going live** - join the arena as a player and run a full match.
4. **Use on-demand for busy servers** - enable it on popular arenas to avoid turning players away.
5. **Keep backups** - copy `plugins/BlueArcade3/data/arenas/<id>/` to back up an entire arena.
