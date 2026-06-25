# PlaceholderAPI Placeholders

Blue Worlds provides extensive PlaceholderAPI integration for use in scoreboards, holograms, TAB lists, and other plugins.

**Requirement:** [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) must be installed.

**Expansion:** `blueworlds`

---

## General Placeholders

These placeholders do not require a specific player.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_version%` | Plugin version |
| `%blueworlds_global_count%` | Total number of global worlds |
| `%blueworlds_private_total%` | Total number of private worlds on the server |

---

## Player Private Worlds

Placeholders related to the worlds of the player executing the placeholder.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_private_count%` | Worlds owned by the player |
| `%blueworlds_private_trusted_count%` | Worlds where the player is trusted |
| `%blueworlds_private_total%` | Total (owned + trusted) |

---

## Slot Placeholders

Information about worlds in specific player slots. Replace `<number>` with the slot number (1, 2, 3...).

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_private_slot_<number>_exists%` | Whether a world exists in that slot |
| `%blueworlds_private_slot_<number>_name%` | Technical world name |
| `%blueworlds_private_slot_<number>_alias%` | World alias |
| `%blueworlds_private_slot_<number>_loaded%` | Whether the world is loaded |
| `%blueworlds_private_slot_<number>_environment%` | Environment type |
| `%blueworlds_private_slot_<number>_gamemode%` | Game mode |
| `%blueworlds_private_slot_<number>_trusted_count%` | Trusted players |
| `%blueworlds_private_slot_<number>_whitelist%` | Whitelist status |
| `%blueworlds_private_slot_<number>_players_count%` | Online players |
| `%blueworlds_private_slot_<number>_flag_<flagname>%` | Value of a specific flag |

**Examples:**
```
%blueworlds_private_slot_1_alias%
%blueworlds_private_slot_2_flag_pvp%
%blueworlds_private_slot_3_trusted_count%
```

---

## Current World (General)

General information about the world the player is currently in.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_current_name%` | Technical world name |
| `%blueworlds_current_alias%` | Alias / custom name |
| `%blueworlds_current_type%` | World type (`private`, `global`, or `other`) |
| `%blueworlds_current_players_count%` | Players in the world |
| `%blueworlds_current_environment%` | World environment |
| `%blueworlds_current_difficulty%` | Difficulty |
| `%blueworlds_current_pvp%` | PvP enabled |

---

## Current Private World

Specific information if the player is in a private world.

### Basic Information

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_id%` | Internal world ID |
| `%blueworlds_currentprivate_name%` | Technical name |
| `%blueworlds_currentprivate_alias%` | Custom alias |
| `%blueworlds_currentprivate_description%` | World description |
| `%blueworlds_currentprivate_tag%` | Tag / category |

### Player Relationship

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_slot%` | World slot |
| `%blueworlds_currentprivate_role%` | Player role (`OWNER`, `TRUSTED`, `ADDED`, `VISITOR`) |
| `%blueworlds_currentprivate_is_owner%` | Is the owner |
| `%blueworlds_currentprivate_is_trusted%` | Is trusted |
| `%blueworlds_currentprivate_can_build%` | Can build |
| `%blueworlds_currentprivate_is_banned%` | Is banned |

### Owner Information

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_owner%` | Owner name |
| `%blueworlds_currentprivate_owner_uuid%` | Owner UUID |

### Members and Permissions

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_trusted_count%` | Trusted players |
| `%blueworlds_currentprivate_added_count%` | Added players (temporary) |
| `%blueworlds_currentprivate_whitelist%` | Whitelist status |
| `%blueworlds_currentprivate_whitelist_enabled%` | Whitelist enabled (boolean) |
| `%blueworlds_currentprivate_whitelist_count%` | Players on whitelist |
| `%blueworlds_currentprivate_banned_count%` | Banned players |

### World Details

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_players_count%` | Online players |
| `%blueworlds_currentprivate_environment%` | Environment |
| `%blueworlds_currentprivate_type%` | Generator type |
| `%blueworlds_currentprivate_generator%` | Generator used |
| `%blueworlds_currentprivate_gamemode%` | Game mode |
| `%blueworlds_currentprivate_difficulty%` | Difficulty |
| `%blueworlds_currentprivate_seed%` | World seed |
| `%blueworlds_currentprivate_template%` | Template used |
| `%blueworlds_currentprivate_world_border%` | Border size |
| `%blueworlds_currentprivate_loaded%` | World loaded |

### Dimensions

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_has_nether%` | Has linked nether |
| `%blueworlds_currentprivate_has_end%` | Has linked end |

### Flags

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentprivate_flag_<flagname>%` | Value of any flag |

**Examples:**
```
%blueworlds_currentprivate_flag_pvp%
%blueworlds_currentprivate_flag_fly%
%blueworlds_currentprivate_flag_gamemode%
```

---

## Current Global World

Specific information if the player is in a global world.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_currentglobal_name%` | Technical name |
| `%blueworlds_currentglobal_alias%` | Custom alias |
| `%blueworlds_currentglobal_description%` | Description |
| `%blueworlds_currentglobal_environment%` | Environment |
| `%blueworlds_currentglobal_difficulty%` | Difficulty |
| `%blueworlds_currentglobal_gamemode%` | Game mode |
| `%blueworlds_currentglobal_tag%` | Tag |
| `%blueworlds_currentglobal_players_count%` | Online players |
| `%blueworlds_currentglobal_players%` | Player list |
| `%blueworlds_currentglobal_flag_<flagname>%` | Flag value |

---

## Global World by Name

Replace `<world>` with the world name.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_global_<world>_alias%` | Alias / custom name |
| `%blueworlds_global_<world>_description%` | World description |
| `%blueworlds_global_<world>_environment%` | Environment type |
| `%blueworlds_global_<world>_difficulty%` | Difficulty |
| `%blueworlds_global_<world>_gamemode%` | Default game mode |
| `%blueworlds_global_<world>_tag%` | Tags / categories |
| `%blueworlds_global_<world>_players%` | List of online players |
| `%blueworlds_global_<world>_players_count%` | Number of players |
| `%blueworlds_global_<world>_flag_<flagname>%` | Value of a specific flag |

**Example:**
```
%blueworlds_global_world_flag_pvp%
```

---

## Private World by ID

Replace `<id>` with the world ID.

| Placeholder | Description |
|-------------|-------------|
| `%blueworlds_private_<id>_name%` | Technical name |
| `%blueworlds_private_<id>_alias%` | World alias |
| `%blueworlds_private_<id>_owner%` | Owner name |
| `%blueworlds_private_<id>_loaded%` | Whether it is loaded |
| `%blueworlds_private_<id>_trusted_count%` | Trusted players |
| `%blueworlds_private_<id>_whitelist%` | Whitelist status |
| `%blueworlds_private_<id>_whitelist_count%` | Players on whitelist |
| `%blueworlds_private_<id>_players_count%` | Online players |
| `%blueworlds_private_<id>_environment%` | Environment type |
| `%blueworlds_private_<id>_gamemode%` | Game mode |
| `%blueworlds_private_<id>_flag_<flagname>%` | Flag value |

**Examples:**
```
%blueworlds_private_123_alias%
%blueworlds_private_456_owner%
%blueworlds_private_789_flag_pvp%
```

---

## Usage Examples

### Scoreboard

```yaml
lines:
  - "<gold><bold>MY WORLD</bold>"
  - ""
  - "<yellow>%blueworlds_currentprivate_alias%"
  - ""
  - "<gray>Owner: <white>%blueworlds_currentprivate_owner%"
  - "<gray>Role: <green>%blueworlds_currentprivate_role%"
  - ""
  - "<gray>PvP: <red>%blueworlds_currentprivate_flag_pvp%"
  - "<gray>Fly: <green>%blueworlds_currentprivate_flag_fly%"
  - "<gray>Mode: <yellow>%blueworlds_currentprivate_gamemode%"
```

### Hologram

```
<gold><bold>%blueworlds_currentprivate_alias%
<gray>Owner: <white>%blueworlds_currentprivate_owner%

<gray>Players: <aqua>%blueworlds_currentprivate_players_count%
<gray>Trusted: <blue>%blueworlds_currentprivate_trusted_count%
```

### DeluxeMenus - Player World List

```yaml
item_1:
  material: GRASS_BLOCK
  display_name: "<gold>World #1: <white>%blueworlds_private_slot_1_alias%"
  lore:
    - ""
    - "<gray>Type: %blueworlds_private_slot_1_environment%"
    - "<gray>Mode: %blueworlds_private_slot_1_gamemode%"
    - "<gray>Trusted: %blueworlds_private_slot_1_trusted_count%"
    - "<gray>Players: %blueworlds_private_slot_1_players_count%"
    - ""
    - "<gray>Click to teleport"
  left_click_commands:
    - "[player] pw goto 1"
  view_requirement: "%blueworlds_private_slot_1_exists% == true"
```
