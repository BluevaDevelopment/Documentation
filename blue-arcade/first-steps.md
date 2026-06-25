# Getting Started

This guide walks you through installing Blue Arcade and creating your first dynamic arena.

## Purchase the Plugin

Download Blue Arcade from one of the official stores:

- [Blueva Store](https://blueva.net/store/blue-arcade)
- [BuiltByBit](https://builtbybit.com/resources/blue-arcade-8-minigames-party-games.28272/)
- [Polymart](https://polymart.org/product/4146/blue-arcade)
- [Spigot](https://www.spigotmc.org/resources/blue-arcade-2-14-minigames-in-1-plugin-new-block-party.111961/)

## Server Requirements

Before installing, make sure your server meets these requirements:

- **Spigot** or any fork (Paper, Purpur, Folia, etc.)
- **Minecraft 1.18** or newer
- **Java 17** or higher
- **2–4 GB RAM** minimum (excluding other plugins and players)

## Installation

1. **Stop your server** if it is running.
2. **Download** the plugin JAR from your purchase location.
3. **Place** the JAR in your server's `plugins/` folder.
4. **Start** the server. Required libraries are downloaded automatically on first startup.
5. **Set the main lobby** with `/baa setmainlobby`.

That is it - the plugin is now installed.

## Install Minigame Modules

Blue Arcade uses a modular system. Minigames must be installed separately.

### In-Game Installation (Recommended)

Use the built-in module store:

```
/baa module store download [module_id]
```

**Examples:**
```
/baa module store download spleef
/baa module store download race
/baa module store download tnt_run
```

### Manual Installation

1. Download the module JAR from the [Blueva Store](https://blueva.net/store).
2. Place it in `plugins/BlueArcade3/modules/`.
3. Run `/baa reload all` or restart the server.

## First Arena Setup

Blue Arcade creates **dynamic arenas by default**. Each match runs in a fresh temporary world copied from a template you build. This removes the need for manual world regeneration and is the recommended approach.

### 1. Set the Main Lobby

Stand at your main lobby location and run:

```
/baa setmainlobby
```

This is where players teleport after leaving an arena.

### 2. Create an Arena

Choose a mode:

- **Standalone**: single minigame per arena
- **Party**: multiple minigames with rounds and voting (recommended)

```
/baa create [id] <standalone|party> [dynamic|static]
```

**Examples:**
```
/baa create 1 party dynamic
/baa create 2 standalone dynamic
```

If you do not specify a type, the arena is created as **dynamic**.

When you create a dynamic arena, the plugin creates a temporary void world (`ba-temporal-<id>-lobby`) and teleports you into it in Creative mode. Build your waiting lobby there.

### 3. Configure the Arena

```
/baa arena [id] setname [name]      # Set display name
/baa arena [id] setlobby            # Set waiting lobby spawn
/baa arena [id] minplayers [amount] # Set minimum players (0 = admin-managed, otherwise 2+)
/baa arena [id] maxplayers [amount] # Set maximum players (2+)
/baa arena [id] setrounds [number]  # Set rounds (3-15, party mode only)
```

### 4. Add Minigames

```
/baa game [id] add <micro|mini> [minigame]
```

**Examples:**
```
/baa game 1 add micro spleef
/baa game 1 add mini block_party
```

When you add a minigame to a dynamic arena, the plugin saves the lobby template, creates a new temporary world for that minigame (`ba-temporal-<id>-<minigame>`), and teleports you into it. Build the map there.

### 5. Configure Each Minigame

All minigames share common setup steps. Each module may add its own steps.

#### Get the Magic Stick

```
/baa stick
```

#### Set Boundaries

- Left-click to set position 1
- Right-click to set position 2
- Save with `/baa game [id] [minigame] bounds set`

#### Add Spawn Points

```
/baa game [id] [minigame] spawn add
```

Add at least as many spawns as your arena's max players.

#### Set Duration

```
/baa game [id] [minigame] time [minutes]
```

#### Enable the Minigame

```
/baa game [id] [minigame] enable
```

For dynamic arenas, enabling a minigame zips the temporary world as the template for that game and returns you to the lobby.

### 6. Enable the Arena

Once all minigames are configured:

```
/baa enable [id]
```

For dynamic arenas, this saves the final lobby template and enables the arena.

## Quick Reference

| Action | Command |
|--------|---------|
| Set main lobby | `/baa setmainlobby` |
| Create arena | `/baa create [id] <standalone\|party> [dynamic\|static]` |
| Set arena name | `/baa arena [id] setname [name]` |
| Set arena lobby | `/baa arena [id] setlobby` |
| Set player limits | `/baa arena [id] minplayers [n]` / `maxplayers [n]` |
| Add minigame | `/baa game [id] add <micro\|mini> [minigame]` |
| Set bounds | `/baa game [id] [minigame] bounds set` |
| Add spawn | `/baa game [id] [minigame] spawn add` |
| Enable minigame | `/baa game [id] [minigame] enable` |
| Enable arena | `/baa enable [id]` |
| View arena info | `/baa info [id]` |
| List arenas | `/baa list` |

## Troubleshooting

**"Arena already exists"**
- Use a different ID number.
- Check existing arenas with `/baa list`.

**"Cannot enable arena. Complete the configuration first"**
- You need at least one enabled minigame.
- Check with `/baa info [id]`.

**"Add at least X spawns"**
- Add more spawn points with `/baa game [id] [minigame] spawn add`.
- You need spawns equal to or greater than max players.

**"Both positions must be in the same world"**
- Clear selection with `/baa clearselection`.
- Select both positions in the same world.

**Falling into the void during setup**
- Dynamic setup worlds are void worlds. Build a floor or platform before moving around.

**World inventory plugins overwrite waiting-room items**
- Blue Arcade now delays inventory handling so world-inventory plugins can save the previous state first. If issues persist, make sure you are on the latest version.
