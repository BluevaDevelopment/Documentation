# Getting Started

## Purchase the Plugin

Download Blue Arcade from one of the official stores:

- [Blueva Store](https://blueva.net/store/blue-arcade-hytale-edition)
- [BuiltByBit](https://builtbybit.com/resources/blue-arcade-3-hytale-edition-5-games.91995)

## Server Requirements

Before installing, ensure your server meets these requirements:

- **Hytale Server**
- **Java 25** or higher
- **2-4 GB RAM** minimum (excluding other plugins and players)

## Installation

1. **Stop your server** if it's running
2. **Download** the plugin JAR from your purchase location
3. **Place** the JAR file in your server's `mods` folder
4. **Start** the server
5. **Set the main lobby** with `/baa setmainlobby`

That's it! The plugin is now installed.

## License

If you bought or downloaded the plugin from the [Blueva Store](https://blueva.net/store), your license key is already filled in and there is nothing for you to do.

If you bought it anywhere else (BuiltByBit, PixelEast, Polymart, etc.), the key starts empty and the plugin reminds you in the console every time it starts. To obtain and activate your license:

1. Create a free account at [blueva.net](https://blueva.net/register), or sign in if you already have one.
2. Open your [account settings](https://blueva.net/account?tab=settings) and find the **License** section.
3. Click **Show**, then **Copy** to copy your license key.
4. Paste the key into the `license.key` field of `mods/Blueva_BlueArcade3/license.yml` and restart the server.

```yaml
license:
  key: "your-license-key"
```

> **Note:** The product will be permanently linked to the Blueva account used during activation. Once linked, it cannot be transferred or linked to a different Blueva account.

- The same key works for every Blueva product you own.
- Your license can be used from up to **six different IP addresses**, with no limit on the number of servers running under each IP. Authorized IPs can be viewed and managed from your account settings.
- If your key is lost or compromised, you can replace it from the same page and paste the new one into `license.yml`. The old key stops working everywhere at once.
- Do not edit the `purchase` block in `license.yml`. It is filled in by the store you downloaded from and is only used to confirm your purchase.

For help, visit [blueva.net/support](https://blueva.net/support).

## Install Minigame Modules

Blue Arcade 3.0 uses a modular system. Minigames must be installed separately.

### In-Game Installation (Recommended)

Use the built-in module downloader:

```
/baa module store download [module_name]
```

**Example:**
```
/baa module store download spleef
/baa module store download race
/baa module store download tnt_run
```

### Manual Installation

1. Download the module JAR from the [store](https://blueva.net/store/blue-arcade-hytale-edition)
2. Place it in `mods/Blueva_BlueArcade3/modules/`
3. Run `/baa reload all`

## First Arena Setup

### 1. Set the Main Lobby

Stand at your main lobby location and run:

```
/baa setmainlobby
```

This is where players teleport after leaving an arena.

### 2. Create an Arena

Choose between two arena modes:

- **Standalone**: Single minigame per arena
- **Party**: Multiple minigames with rounds (recommended)

```
/baa create [id] <standalone|party>
```

**Examples:**
```
/baa create 1 party
/baa create 2 standalone
```

### 3. Configure the Arena

```
/baa arena [id] setname [name]      # Set display name
/baa arena [id] setlobby            # Set waiting lobby
/baa arena [id] minplayers [amount] # Set minimum players (2-64)
/baa arena [id] maxplayers [amount] # Set maximum players (2-64)
/baa arena [id] setrounds [number]  # Set rounds (3-15, party mode only)
```

### 4. Add Minigames

```
/baa game [id] add [mini/micro] [minigame]
```

**Example:**
```
/baa game 1 add micro spleef
/baa game 1 add mini block_party
```

### 5. Configure Each Minigame

Each minigame requires common setup steps. See the module's store page for specific instructions.

#### Common Steps (All Minigames)

**Get the Magic Stick:**
```
/baa stick
```

**Set Boundaries:**
- Left-click to set position 1
- Right-click to set position 2
```
/baa game [id] [minigame] bounds set
```

**Add Spawn Points:**
```
/baa game [id] [minigame] spawn add
```
Add at least as many spawns as your arena's max players.

**Set Duration:**
```
/baa game [id] [minigame] time [minutes]
```

**Enable the Minigame:**
```
/baa game [id] [minigame] enable
```

### 6. Enable the Arena

Once all minigames are configured:

```
/baa enable [id]
```

## Quick Reference

| Action | Command |
|--------|---------|
| Set main lobby | `/baa setmainlobby` |
| Create arena | `/baa create [id] <standalone\|party>` |
| Set arena name | `/baa arena [id] setname [name]` |
| Set arena lobby | `/baa arena [id] setlobby` |
| Set player limits | `/baa arena [id] minplayers [n]` / `maxplayers [n]` |
| Add minigame | `/baa game [id] add [mini/micro] [minigame]` |
| Set bounds | `/baa game [id] [minigame] bounds set` |
| Add spawn | `/baa game [id] [minigame] spawn add` |
| Enable minigame | `/baa game [id] [minigame] enable` |
| Enable arena | `/baa enable [id]` |
| View arena info | `/baa info [id]` |
| List arenas | `/baa list` |

## Troubleshooting

**"Arena already exists"**
- Use a different ID number
- Check existing arenas with `/baa list`

**"Cannot enable arena. Complete the configuration first"**
- You need at least one enabled minigame
- Check with `/baa info [id]`

**"Add at least X spawns"**
- Add more spawn points with `/baa game [id] [minigame] spawn add`
- Need spawns equal to max players

**"Both positions must be in the same world"**
- Clear selection with `/baa clearselection`
- Select both positions in the same world
