# Getting Started

This guide walks you through installing Blue Worlds and setting up your first templates and worlds.

## Purchase the Plugin

Download Blue Worlds from one of the official stores:

- [Blueva Store](https://blueva.net/store/blue-worlds)
- [BuiltByBit](https://builtbybit.com)
- [Polymart](https://polymart.org)
- [SpigotMC](https://spigotmc.org)

## Server Requirements

Before installing, ensure your server meets these requirements:

- **Spigot/Paper 1.21.x** or **Paper 26.1+**
- **Java 21** or higher
- **2-4 GB RAM** minimum (excluding other plugins and players)

> **Note:** If `server.properties` has `spawn-protection > 0`, Blue Worlds will warn you because vanilla spawn protection can block interactions in private worlds.

## Installation

1. **Stop your server** if it is running.
2. **Download** the plugin JAR from your purchase location.
3. **Place** the JAR in your server's `plugins/` folder.
4. **Start** the server. Blue Worlds will generate its configuration files and folders.
5. **Edit** `plugins/BlueWorlds/settings.yml` and `plugins/BlueWorlds/language.yml` as needed.
6. **Restart** the server or run `/bw reload`.

## Initial Configuration

After first installation, Blue Worlds creates the following directory structure:

```
plugins/BlueWorlds/
├── settings.yml          # Main configuration file
├── language.yml          # All player-facing messages
├── menus/                # GUI menu configurations
│   ├── private/
│   │   ├── java/
│   │   └── bedrock/
│   └── public/
│       ├── java/
│       └── bedrock/
└── data/                 # World, user, and template data
    ├── templates/        # World template ZIP archives
    ├── worlds/           # Stored worlds
    │   ├── private/      # Stored private worlds
    │   └── global/       # Stored global worlds
    └── trash/            # Deleted worlds pending removal
```

### Basic Settings (settings.yml)

Open `plugins/BlueWorlds/settings.yml` and review:

```yaml
# GUI mode
private_worlds:
  use_gui: true

global_worlds:
  use_gui: true

# World unloading (saves RAM)
world_unloading:
  enabled: true
  unload_when_owner_offline: true
  public_worlds:
    enabled: true

# Backup settings
backups:
  enabled: true
  backup_interval: 2d
  max_backups: 10
  delete_old_backups: true
  compression: true
```

After editing, save the file and run `/bw reload` or restart the server.

## Creating Your First Template

Templates are the foundation of Blue Worlds. Before players can create private worlds or you can create global worlds, you need templates.

### Create from an Existing World

If you have a world you want to use as a template:

```
/bw template create <template_name> [world_name]
```

**Examples:**
```
/bw template create void world
/bw template create skyblock my_skyblock_world
```

If you omit `world_name`, the current world is used.

### Import from a Folder

If you have a pre-built world folder:

```
/bw template import <folder> <template_name>
```

**Example:**
```
/bw template import world_templates/custom_island my_island
```

### Verify Your Template

```
/bw template list
/bw template info <template_name>
```

## Setting Up Private Worlds

Private worlds allow players to create and manage their own personal worlds.

### Step 1: Create Templates

Create at least one template that players can use (see above).

### Step 2: Configure Permissions

Give players permission to create private worlds. The easiest way is to grant the user pack:

```yaml
permissions:
  - blueworlds.pack.user
```

### Step 3: Test Private World Creation

As a player, run:

```
/pw create <template_name> [custom_name]
```

**Examples:**
```
/pw create void
/pw create skyblock "My Island"
```

The world will be created and you will be teleported to it automatically.

## Setting Up Global Worlds

Global worlds are server-wide worlds accessible to all players.

### Create from a Template

```
/gw create template <world_name> <template_name>
```

**Examples:**
```
/gw create template spawn void
/gw create template resource survival_template
```

### Link Dimensions (Optional)

For a full survival experience, link Nether and End dimensions:

```
/gw link <world> --all
```

**Example:**
```
/gw link world --all
```

## Quick Reference

| Action | Command |
|--------|---------|
| Create template | `/bw template create <name> [world]` |
| List templates | `/bw template list` |
| Create private world | `/pw create <template> [name]` |
| Go to private world | `/pw goto [slot]` |
| Create global world | `/gw create template <name> <template>` |
| Set world flag | `/pw flag set <flag> <value>` |
| Link dimensions | `/gw link <world> --all` |
| Reload config | `/bw reload` |

## Troubleshooting

**"Plugin not loading"**
- Check console for errors.
- Verify Java 21+.
- Ensure compatible server version (1.21.x / 26.1+).

**"Commands not working"**
- Verify permissions are set correctly.
- Check if you are using the correct aliases (`/bw`, `/pw`, `/gw`).
- Ensure you have operator status for admin commands.

**"Worlds not creating"**
- Check server RAM availability.
- Verify template exists (`/bw template list`).
- Check console for error messages.

**"Vanilla spawn protection blocks interactions"**
- Set `spawn-protection=0` in `server.properties`.
- Restart the server.

**Need more help?**
- Visit: https://blueva.net
- Check console logs for detailed error messages.
