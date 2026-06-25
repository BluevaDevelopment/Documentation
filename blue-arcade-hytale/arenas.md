# Creating Arenas

This guide covers arena creation and configuration in detail.

## Before You Start

We recommend:

- **Enable server whitelist** while configuring
- **Prepare your worlds/zones** in advance (lobby, waiting area, minigame maps)
- **Use void worlds** for optimal performance - [Void World Generator](https://www.curseforge.com/hytale/mods/void-world-generator)

## Arena Modes

### Standalone Mode

- Single minigame per arena
- Players play the same game repeatedly
- Best for dedicated servers (e.g., "TNT Tag servers")

### Party Mode (Recommended)

- Multiple minigames with rounds
- Random minigame selection or voting
- Players earn stars across rounds
- Winner determined by total stars

## Creating an Arena

### Step 1: Create

```
/baa create [id] <standalone|party>
```

**Examples:**
```
/baa create 1 party
/baa create 2 standalone
```

**Tips:**
- Use sequential numbers (1, 2, 3...)
- IDs must be unique
- Cannot change ID later

### Step 2: Set Display Name

```
/baa arena [id] setname [name]
```

**Examples:**
```
/baa arena 1 setname Party Games Arena
/baa arena 1 setname <green>Mega Arena</green> <gray>#1</gray>
```

### Step 3: Set Waiting Lobby

Stand at your desired location and run:

```
/baa arena [id] setlobby
```

**Important:**
- Your position AND viewing direction are saved
- Players spawn facing the same direction

### Step 4: Configure Player Limits

```
/baa arena [id] minplayers [amount]
/baa arena [id] maxplayers [amount]
```

**Requirements:**
- Minimum: 2-64 players
- Maximum: 2-64 players
- Min cannot exceed max

### Step 5: Set Rounds (Party Mode Only)

```
/baa arena [id] setrounds [number]
```

**Requirements:**
- Between 3-15 rounds
- Default: 5 rounds

## Changing Arena Mode

```
/baa arena [id] setmode <standalone|party>
```

**Restrictions:**
- Standalone can only have 1 minigame
- Remove extra minigames before switching to standalone

## Verify Configuration

```
/baa info [id]
```

Shows:
- Arena name and mode
- Status (ENABLED/DISABLED)
- Player limits and rounds
- Configured minigames
- Lobby location

## Arena States

| State | Description |
|-------|-------------|
| WAITING | Waiting for players to join |
| RUNNING | Game in progress |
| RESTARTING | Resetting for next game |

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
/baa disable [id]
```

**Delete arena:**
```
/baa delete [id] confirm
```

## Adding Minigames

After creating an arena, add minigames:

```
/baa game [id] add [minigame]
```

See the module's store page for minigame-specific setup instructions.

## Common Minigame Setup

All minigames share these common configuration steps:

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

1. Use Magic Stick to select opposite corners
2. Save with: `/baa game [id] [minigame] bounds set`

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

Range: 1-60 minutes (default: 5)

### Set Death Block (Optional)

```
/baa game [id] [minigame] deathblock [material]
```

### Enable Minigame

```
/baa game [id] [minigame] enable
```

### Manage Minigames

```
/baa game [id] list                    # List all minigames
/baa game [id] [minigame] disable      # Disable minigame
/baa game [id] remove [minigame] confirm # Remove minigame
```

## Utility Commands

**Teleport to spawn:**
```
/baa goto [id] [minigame]
```

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
/baa reload <all|lang|arenas|settings|rewards|signs|global|games|users|data>
```
