# Creating Arenas

This guide covers arena creation and configuration in detail. Blue Arcade uses **dynamic arenas by default**, and this documentation focuses on the modern dynamic workflow. Static arenas are still supported for backward compatibility but are considered legacy and will be deprecated in a future release.

## Before You Start

We recommend:

- **Enable your server whitelist** while configuring.
- **Plan your arenas**: decide which minigames go in each arena, max player counts, and visual theme.
- **Use void worlds** for dynamic setup worlds. The plugin creates them automatically; you only build what the players will see.
- **Install WorldEdit** if you want to import schematics or copy large structures into setup worlds.

## Arena Types

### Dynamic Arena (Recommended, Default)

- Each match runs in a **fresh temporary world** copied from a saved template.
- The waiting lobby and each minigame have their own template ZIP.
- No manual regeneration is needed — the world is deleted and recreated for every match.
- Supports **Arena On Demand** for automatic extra instances.
- Supports **AdvancedSlimePaper** for faster in-memory runtime worlds.

### Static Arena (Legacy)

- Uses a persistent world you build or import yourself.
- Only the configured game region is restored between matches using block snapshots.
- Still available for backward compatibility, but **static arenas will be deprecated**.
- This guide focuses on dynamic arenas; most commands still apply to static arenas unless noted.

## Arena Modes

### Standalone Mode

- Single minigame per arena.
- Players play the same game repeatedly.
- Best for dedicated servers (for example, a "Spleef Server").

### Party Mode (Recommended)

- Multiple minigames with rounds.
- Players vote for the next minigame or follow a fixed order.
- Players earn stars across rounds.
- Winner is determined by total stars.

## Creating an Arena

### Step 1: Create

```
/baa create [id] <standalone|party> [dynamic|static]
```

**Examples:**
```
/baa create 1 party dynamic
/baa create 2 standalone dynamic
/baa create 3 party           # type defaults to dynamic
```

**Tips:**
- Use sequential numbers (1, 2, 3...).
- IDs must be unique.
- The ID cannot be changed later.
- For dynamic arenas, the plugin creates a temporary lobby world and teleports you into it in Creative mode.

### Step 2: Set Display Name

```
/baa arena [id] setname [name]
```

**Examples:**
```
/baa arena 1 setname Party Games Arena
/baa arena 1 setname <green><bold>Mega Arena</bold></green>
```

Names use **MiniMessage** formatting. Examples:
- `<green>Green Text</green>`
- `<bold>Bold</bold>`
- `<gradient:red:blue>Gradient</gradient>`

Legacy `&` color codes are also accepted in most places.

### Step 3: Set Waiting Lobby

In a dynamic arena, stand in the temporary lobby world at your desired location and run:

```
/baa arena [id] setlobby
```

**Important:**
- Your position **and** viewing direction are saved.
- Players spawn facing the same direction.

If you want the waiting lobby to be inside the minigame world instead of a separate world (useful for standalone arenas), use:

```
/baa arena [id] setlobby skip
```

### Step 4: Configure Player Limits

```
/baa arena [id] minplayers [amount]
/baa arena [id] maxplayers [amount]
```

**Requirements:**
- Minimum: `0` (admin-managed) or `2+`.
- Maximum: `2+`.
- Minimum cannot exceed maximum.
- There is no hard 64-player cap; set limits that make sense for your hardware.

### Step 5: Set Rounds (Party Mode Only)

```
/baa arena [id] setrounds [number]
```

**Requirements:**
- Between 3–15 rounds.
- Default: configured in `settings.yml` (`game.global.default_rounds_per_arena`, default 8).

## Dynamic Arena Setup Flow

A typical dynamic arena setup looks like this:

1. `/baa create 3 party dynamic`
   - Teleports you to `ba-temporal-3-lobby` in Creative mode.
2. Build the waiting lobby.
3. `/baa arena 3 setlobby`
   - Saves the waiting-lobby spawn location.
4. `/baa game 3 add micro spleef`
   - Saves the lobby template, creates `ba-temporal-3-spleef`, teleports you in.
5. Build the spleef map.
6. Use `/baa stick` + `/baa pos1`/`/baa pos2` to select the play area, then:
   - `/baa game 3 spleef bounds set`
7. `/baa game 3 spleef spawn add` (repeat for all spawns).
8. `/baa game 3 spleef time 5`
9. `/baa game 3 spleef enable`
   - Saves the spleef world ZIP and returns you to the lobby.
10. Repeat steps 4–9 for more minigames (party mode).
11. `/baa enable 3`
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

### Editing Restrictions

- While in a setup session you **cannot teleport out of the temporal world**.
- You are forced into Creative mode inside the temporal world.
- Disconnecting ends the session.
- Only one player can edit an arena at a time.

### Editing an Existing Dynamic Arena

To reopen a dynamic arena or minigame template:

```
/baa arena [id] edit     # Reopens the lobby template
/baa game [id] [minigame] edit  # Reopens the minigame template
```

Make your changes, then re-enable the game or arena to save the updated ZIP.

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

### Change Mode

```
/baa arena [id] setmode <standalone|party>
```

**Restrictions:**
- Standalone can only have one minigame.
- Remove extra minigames before switching to standalone.

### Change Type

```
/baa arena [id] settype <static|dynamic>
```

**Important:** This only changes the arena type flag. It does **not** convert existing templates or persistent worlds. Use it when you know what you are doing.

### Arena On Demand

Enable automatic extra instances for busy dynamic arenas:

```
/baa arena [id] ondemand <true|false>
```

- The base arena is instance `0`.
- When a join request cannot fit, a new virtual instance is created (`3-1`, `3-2`, etc.).
- Each instance uses the same templates and gets its own runtime world.
- Empty instances are destroyed automatically.
- Target instances in admin commands using the full key: `/baa info 3-1`, `/baa forcestop 3-2`.

### Voting and Game Order (Party Mode)

Disable voting:
```
/baa arena [id] disablevoting <true|false>
```

Set a fixed minigame rotation when voting is disabled:
```
/baa arena [id] setgameorder <game1,game2,...>
```

Clear the fixed order and return to random:
```
/baa arena [id] cleargameorder
```

## Verify Configuration

```
/baa info [id]
```

Shows:
- Arena name, mode, and type
- Status (ENABLED/DISABLED) and state
- Player limits and rounds
- Configured minigames
- Lobby location
- On-demand status

## Adding Minigames

After creating an arena, add minigames:

```
/baa game [id] add <micro|mini> [minigame]
```

For a list of available minigames of a type:
```
/baa game [id] add <micro|mini> list
```

See the module's store page for minigame-specific setup instructions.

## Common Minigame Setup

All minigames share these common configuration steps. Each module may register additional steps.

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
2. Save with: `/baa game [id] [minigame] bounds set`.

### Add Spawn Points

```
/baa game [id] [minigame] spawn add      # Add at current position
/baa game [id] [minigame] spawn list     # View all spawns
/baa game [id] [minigame] spawn remove [#] # Remove spawn
/baa game [id] [minigame] spawn teleport [#] # Teleport to spawn
```

**Requirement:** Add spawns equal to or greater than max players.

### Set Duration

```
/baa game [id] [minigame] time [minutes]
```

Range: 1–60 minutes (default: 5).

### Set Death Block (Optional)

```
/baa game [id] [minigame] deathblock [material]
```

Common options: `LAVA`, `WATER`, `AIR`, `BARRIER`.

### Enable Minigame

```
/baa game [id] [minigame] enable
```

For dynamic arenas, this saves the minigame world template.

### Manage Minigames

```
/baa game [id] list                    # List all minigames
/baa game [id] [minigame] disable      # Disable minigame
/baa game [id] remove [minigame] confirm # Remove minigame
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
```

| File | Purpose |
|------|---------|
| `arena.json` | Arena metadata: mode, type, on-demand, lobby location, games list |
| `lobby/world.zip` | Waiting lobby template |
| `<minigame>/game.json` | Per-minigame config: bounds, spawns, time, death block |
| `<minigame>/world.zip` | Minigame world template |
| `<minigame>/world-border.properties` | Saved world border for the template |

Runtime worlds are created as `ba-dyn-<id>-<slot>` and deleted automatically. Setup worlds are created as `ba-temporal-<id>-lobby` or `ba-temporal-<id>-<minigame>` and are also cleaned up automatically.

## Configuration Options

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
/baa reload <all|settings|rewards|sounds|lang|menus|achievements|global|signs|arenas|games|users|store|data>
```

**Upgrade files after major updates:**
```
/baa upgradefiles confirm
```

## Tips for Dynamic Arenas

1. **Build small and focused** — only build what players will see; void worlds keep file sizes small.
2. **Save often** — enable each minigame as soon as its setup is complete.
3. **Test before going live** — join the arena as a player and run a full match.
4. **Use on-demand for busy servers** — enable it on popular arenas to avoid turning players away.
5. **Keep backups** — copy `plugins/BlueArcade3/data/arenas/<id>/` to back up an entire arena.
