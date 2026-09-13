# Placeholders

BlueHousing placeholders work in two ways:

- **Inside the plugin**: menus, scoreboard lines, hologram text, NPC names: `{bluehousing_<name>}`
- **In other plugins**: TAB, scoreboards, holograms: `%bluehousing_<name>%`, with [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) installed

**Expansion:** `bluehousing`

Anything that answers about *a house* answers about the house the player is standing in, and resolves to an empty string outside one.

---

## Plugin & Player

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_version%` | Plugin version (alias `plugin_version`) |
| `%bluehousing_prefix%` | The chat prefix every message uses |
| `%bluehousing_prefix_admin%` | The admin prefix |
| `%bluehousing_player%` | The player's name |
| `%bluehousing_uuid%` | Their UUID |
| `%bluehousing_ping%` | Ping in milliseconds |
| `%bluehousing_date%` | Current date |
| `%bluehousing_time%` | Current time |

## Current House

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_currenthouse_id%` | The house id (alias `world_id`, `id`) |
| `%bluehousing_currenthouse_name%` | World name (alias `world_name`, `world`) |
| `%bluehousing_currenthouse_alias%` | Display name (alias `alias`) |
| `%bluehousing_currenthouse_owner%` | Owner name (alias `owner`) |
| `%bluehousing_currenthouse_owner_uuid%` | Owner UUID (alias `owner_uuid`) |
| `%bluehousing_currenthouse_role%` | Your role here (alias `role`) |
| `%bluehousing_currenthouse_slot%` | Its slot for the owner (alias `slot`) |
| `%bluehousing_description%` | House description |
| `%bluehousing_online%` | Players inside (alias `online_players`) |
| `%bluehousing_status%` | Open or private |
| `%bluehousing_status_symbol%` | The same, as a symbol |
| `%bluehousing_last_visited%` | When you were last here |

## Members

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_trusted_count%` | Trusted players |
| `%bluehousing_added_count%` | Added players |
| `%bluehousing_whitelisted_count%` | Whitelisted players |
| `%bluehousing_whitelist_status%` | Whether the whitelist is on |
| `%bluehousing_banned_count%` | Bans |

## Your Houses

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_house_count%` | How many you own (alias `house_owned_count`) |
| `%bluehousing_house_trusted_count%` | How many you are trusted in |
| `%bluehousing_house_total%` | Houses on the server |
| `%bluehousing_house_slot_<n>_<field>%` | A field of your slot `<n>` |

## Economy

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_balance%` | Formatted with the currency (alias `money`) |
| `%bluehousing_balance_raw%` | The number alone |
| `%bluehousing_rewards_earned_building%` | Earned from building today |
| `%bluehousing_rewards_earned_playtime%` | Earned from playtime today |
| `%bluehousing_rewards_left_building%` | Left of today's building limit |
| `%bluehousing_rewards_left_playtime%` | Left of today's playtime limit |

`rewards_left_*` shows `economy.rewards.no_limit` (`∞`) when that source is uncapped.

## Building Tools

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_worldedit_available%` | Whether the building menu applies to this player |
| `%bluehousing_worldedit_block%` | The pattern they are placing |
| `%bluehousing_worldedit_radius%` | The size they have chosen |
| `%bluehousing_worldedit_selection%` | How to make a selection |

## Any House

Prefix a house id to ask about a house you are not in:

```
%bluehousing_house_<id>_<field>%
%bluehousing_current_<field>%
```

Example: `%bluehousing_house_Wrr4nsmP_alias%`.

## In Scoreboard Lines

The shipped board uses the `{...}` form:

```yaml
scoreboard:
  lines:
    - "<gray>Owner: <white>{bluehousing_currenthouse_owner}"
    - "<gray>Players: <white>{bluehousing_online}"
    - "<gray>Balance: <white>{bluehousing_balance}"
```

Any PlaceholderAPI placeholder from another plugin works there too.
