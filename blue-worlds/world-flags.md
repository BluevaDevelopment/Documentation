# World Flags

World flags control behavior, protection, game rules, and player state in both private and global worlds.

## Overview

Flags can be set per-world and override default Minecraft behavior. There are **70+ flags** grouped into categories.

## Flag Types

| Type | Example Values |
|------|----------------|
| `BOOLEAN` | `true`, `false` |
| `STRING` | `"My World"`, `"Welcome!"` |
| `INTEGER` | `1000`, `10` |
| `GAMEMODE` | `SURVIVAL`, `CREATIVE`, `ADVENTURE`, `SPECTATOR` |
| `DIFFICULTY` | `PEACEFUL`, `EASY`, `NORMAL`, `HARD` |
| `LOCATION` | `here`, `0,64,0`, `0,64,0,90,0` |

## Commands

### Private Worlds

```
/pw flag set <flag> <value>
/pw flag get <flag>
/pw flag remove <flag>
/pw flag list [category]
/pw flag info <flag>
```

### Global Worlds

```
/gw flag set <flag> <value> -w <world>
/gw flag get <flag> -w <world>
/gw flag remove <flag> -w <world>
/gw flag list [category]
/gw flag info <flag>
```

## Available Flags

### Survival & Damage

| Flag | Description |
|------|-------------|
| `pvp` | PvP enabled |
| `pve` | Mob damage to players |
| `explosion` | Explosions |
| `fire-spread` | Fire spread |
| `fire-damage` | Fire damage |
| `fall-damage` | Fall damage |
| `drowning` | Drowning damage |
| `invincible` | Invincibility |

### Mobs

| Flag | Description |
|------|-------------|
| `mob-spawning` | Mob spawning |
| `animal-spawning` | Animal spawning |
| `hostile-spawning` | Hostile spawning |
| `mob-damage` | Mob damage |
| `animal-attack` | Attack animals |
| `monster-attack` | Attack monsters |

### Interaction

| Flag | Description |
|------|-------------|
| `break` | Break blocks |
| `place` | Place blocks |
| `interact` | Interactions |
| `use` | Use items |
| `pickup` | Pick up items |
| `drop` | Drop items |
| `chest-access` | Chest access |
| `door-interact` | Use doors |
| `lever-interact` | Use levers |

### World Mechanics

| Flag | Description |
|------|-------------|
| `weather-change` | Weather changes |
| `time` | Fixed time |
| `gamemode` | Game mode |
| `fly` | Flight |
| `speed` | Speed |
| `difficulty` | Difficulty |
| `world-border` | World border size |
| `spawn` | Custom spawn |
| `spawn-protection` | Spawn protection |
| `auto-save` | Auto save |
| `keep-spawn-loaded` | Keep spawn chunks loaded |
| `alias` | Custom name |
| `description` | Description |
| `tag` | Tags |

### Player Status

| Flag | Description |
|------|-------------|
| `food` | Hunger level |
| `heal` | Healing |
| `feed` | Feeding |
| `god-mode` | God mode |
| `keep-inventory` | Keep inventory |

### Mechanics

| Flag | Description |
|------|-------------|
| `redstone` | Redstone |
| `device-interact` | Devices |
| `vehicle-use` | Vehicle use |
| `vehicle-place` | Place vehicles |
| `block-physics` | Block physics |
| `leaf-decay` | Leaf decay |
| `ice-form` | Ice formation |
| `ice-melt` | Ice melting |
| `snow-form` | Snow formation |
| `snow-melt` | Snow melting |
| `command-blocks` | Command blocks |
| `enderman-grief` | Enderman grief |
| `ghast-grief` | Ghast grief |
| `wither-grief` | Wither grief |

### Social

| Flag | Description |
|------|-------------|
| `chat` | Chat |
| `titles` | Titles |
| `title` | Entry title |
| `subtitle` | Entry subtitle |
| `notify-enter` | Entry message |
| `notify-leave` | Leave message |

### Creative & Build

| Flag | Description |
|------|-------------|
| `creative-drops` | Drops in creative |

## Useful Flag Examples

```
/pw flag set pvp false
/pw flag set gamemode CREATIVE
/pw flag set difficulty HARD
/pw flag set spawn here
/pw flag set alias "My Cool World"
/pw flag set description "A peaceful survival island"
/pw flag set tag pvp,survival
/pw flag set title "Welcome!"
/pw flag set subtitle "to my island"
/pw flag set keep-inventory true
/pw flag set world-border 500
```

## Default Flags

Default flags for new private and global worlds can be configured in `settings.yml`:

```yaml
default_flags:
  private:
    pvp: false
    gamemode: SURVIVAL
  global:
    pvp: true
    gamemode: SURVIVAL
```

## Per-Flag Permissions

You can restrict which flags players can set:

- `blueworlds.private.flag.<flag>` - allow setting a specific flag in private worlds
- `blueworlds.global.flag.<flag>` - allow setting a specific flag in global worlds

## Troubleshooting

**"You cannot set this flag"**
- Check per-flag permissions.
- Verify the player has `blueworlds.private.flag` or `blueworlds.global.flag`.

**"Invalid flag value"**
- Check the flag type.
- Use `/pw flag info <flag>` or `/gw flag info <flag>` to see valid values.
