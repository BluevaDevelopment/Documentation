# PlaceholderAPI Placeholders

Blue Arcade provides extensive PlaceholderAPI integration for use in scoreboards, holograms, TAB lists, and other plugins.

**Requirement:** [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) must be installed.

**Expansion:** `bluearcade`

---

## Player Placeholders

### Basic Information

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_player_name%` | Player's name |
| `%bluearcade_player_display%` | Player's display name |
| `%bluearcade_player_uuid%` | Player's UUID |
| `%bluearcade_player_is_online%` | Online status (true/false) |
| `%bluearcade_player_ping%` | Network ping |
| `%bluearcade_player_world%` | Current world name |
| `%bluearcade_player_health%` | Current health |
| `%bluearcade_player_food%` | Hunger level |

### Game Status

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_player_is_playing%` | In a game (true/false) |
| `%bluearcade_player_is_spectator%` | Is spectating (true/false) |
| `%bluearcade_player_arena_id%` | Current arena ID |
| `%bluearcade_player_arena_name%` | Current arena display name |

### Team (In-Game)

These placeholders return an empty string (`""`) when the player is not in an arena or is in a game with no teams (solo mode).

All values use **MiniMessage** (Adventure) format.

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_player_team_id%` | Team identifier (e.g. `red`, `blue`) |
| `%bluearcade_player_team_name%` | Team display name as a MiniMessage string |
| `%bluearcade_player_team_color%` | Team color as MiniMessage tag |
| `%bluearcade_player_team_prefix%` | Formatted tab-list prefix. Configure the symbol in `settings.yml` with `game.global.ui.tab_list_team_prefix` |

**Customising the prefix symbol**

Edit `game.global.ui.tab_list_team_prefix` in `settings.yml`. Use full MiniMessage syntax. The special tag `<team_color>` is replaced at runtime with the team's own color.

```yaml
# settings.yml
game:
  global:
    ui:
      tab_list_team_prefix: "<team_color>█ "
```

### Economy & Progress

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_credits%` | Player's credit balance |
| `%bluearcade_player_level%` | BlueArcade level (or Minecraft level if disabled) |
| `%bluearcade_player_exp%` | BlueArcade experience (or Minecraft XP if disabled) |

---

## Server Placeholders

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_server_online%` | Online player count |
| `%bluearcade_server_max_players%` | Max server capacity |
| `%bluearcade_playing_players%` | Players currently in arenas |
| `%bluearcade_modules_total%` | Number of loaded modules |

### Arena Counts

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_arenas_total%` | Total arenas |
| `%bluearcade_arenas_enabled%` | Enabled arenas |
| `%bluearcade_arenas_disabled%` | Disabled arenas |
| `%bluearcade_arenas_waiting%` | Arenas in waiting state |
| `%bluearcade_arenas_running%` | Arenas running |
| `%bluearcade_arenas_restarting%` | Arenas restarting |
| `%bluearcade_arenas_ids%` | Comma-separated arena IDs |

### Module Player Count

```
%bluearcade_module_<module_id>_players%
```

Shows how many players are currently active across all arenas using that module.

**Example:**
```
%bluearcade_module_race_players%
%bluearcade_module_spleef_players%
```

---

## Arena Placeholders

All arena placeholders can be used in two forms:

- `%bluearcade_arena_current_<field>%` — refers to the arena the player is currently in.
- `%bluearcade_arena_<id>_<field>%` — refers to a specific arena ID.

### Basic Arena Info

| Placeholder | Description |
|-------------|-------------|
| `arena_(current\|id)_id` | Arena ID |
| `arena_(current\|id)_name` | Arena display name |
| `arena_(current\|id)_display_name` | Arena display name |
| `arena_(current\|id)_state` | Arena state: `waiting`, `starting`, `running`, `finishing`, `restarting` |
| `arena_(current\|id)_status` | Arena status: `enabled` or `disabled` |
| `arena_(current\|id)_status_key` | Same as `state` |
| `arena_(current\|id)_status_display` | Formatted status label from language config |
| `arena_(current\|id)_exists` | Arena exists (true/false) |
| `arena_(current\|id)_players` | Current player count |
| `arena_(current\|id)_players_max` | Maximum players |
| `arena_(current\|id)_players_min` | Minimum players |
| `arena_(current\|id)_round` | Current round number |
| `arena_(current\|id)_round_max` | Maximum rounds |
| `arena_(current\|id)_countdown` | Countdown seconds |
| `arena_(current\|id)_countdown_formatted` | Countdown as `m:ss` |
| `arena_(current\|id)_game_id` | Current minigame ID |
| `arena_(current\|id)_game_display` | Current minigame display name |
| `arena_(current\|id)_forced_start` | Force started (true/false) |
| `arena_(current\|id)_available_minigames` | Available minigames list |
| `arena_(current\|id)_played_minigames` | Played minigames list |

### Podium & Stars

| Placeholder | Description |
|-------------|-------------|
| `arena_(current\|id)_podium_1` … `podium_10` | Player name at that podium position |
| `arena_(current\|id)_stars_1` … `stars_10` | Stars of player at that position |
| `arena_(current\|id)_player_position` | Your podium position |
| `arena_(current\|id)_player_stars` | Your stars in arena |
| `arena_(current\|id)_stars_total` | Total stars in arena |
| `arena_(current\|id)_stars_average` | Average stars |

### Voting

| Placeholder | Description |
|-------------|-------------|
| `arena_(current\|id)_vote_<minigame>` | Votes for a specific minigame |
| `arena_(current\|id)_votes_total` | Total votes cast |

### Examples

```
%bluearcade_arena_1_name%
%bluearcade_arena_1_state%
%bluearcade_arena_1_players%
%bluearcade_arena_2_game_display%
```

---

## Statistics Placeholders

### Global Stats

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_stat_global_games_played%` | Total games played |
| `%bluearcade_stat_global_mini_games_played%` | Different minigames played |
| `%bluearcade_stat_global_wins%` | Total first places |
| `%bluearcade_stat_global_first_place%` | Total first places |
| `%bluearcade_stat_global_second_place%` | Total second places |
| `%bluearcade_stat_global_third_place%` | Total third places |
| `%bluearcade_stat_global_stars%` | Total stars earned |
| `%bluearcade_stat_global_kills%` | Total kills |
| `%bluearcade_stat_global_deaths%` | Total deaths |
| `%bluearcade_stat_global_credits%` | Total credits earned |
| `%bluearcade_stat_global_current_win_streak%` | Current win streak |
| `%bluearcade_stat_global_best_win_streak%` | Best win streak |
| `%bluearcade_stat_global_time_played%` | Time played |

### Module-Specific Stats

Format: `%bluearcade_stat_module_[module_id]_[stat_name]%`

**Examples:**
```
%bluearcade_stat_module_race_wins%
%bluearcade_stat_module_spleef_games_played%
%bluearcade_stat_module_snowball_fight_kills%
```

### Stat Periods

Append a period suffix to get time-based stats:

| Suffix | Description |
|--------|-------------|
| `_alltime` | All-time statistics (default) |
| `_daily` | Today's statistics |
| `_weekly` | This week's statistics |
| `_monthly` | This month's statistics |
| `_yearly` | This year's statistics |

**Examples:**
```
%bluearcade_stat_global_wins_daily%
%bluearcade_stat_global_kills_weekly%
%bluearcade_stat_module_race_wins_monthly%
```

### Stat Metadata

Get information about stat definitions:

| Format | Returns |
|--------|---------|
| `%bluearcade_stat_display_global_[stat]%` | Display name |
| `%bluearcade_stat_description_global_[stat]%` | Description |
| `%bluearcade_stat_scope_global_[stat]%` | Scope (global/module) |
| `%bluearcade_stat_display_module_[module]_[stat]%` | Module stat display name |
| `%bluearcade_stat_description_module_[module]_[stat]%` | Module stat description |
| `%bluearcade_stat_scope_module_[module]_[stat]%` | Module stat scope |

---

## Stats System Info

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_stats_enabled%` | Stats system enabled (true/false) |
| `%bluearcade_stats_sync_enabled%` | Sync enabled (true/false) |
| `%bluearcade_stats_modules_list%` | Registered modules list |
| `%bluearcade_stats_modules_total%` | Number of modules with stats |
| `%bluearcade_stats_global_list%` | Global stat keys list |
| `%bluearcade_stats_global_total%` | Number of global stats |
| `%bluearcade_stats_module_[id]_list%` | Module stat keys |
| `%bluearcade_stats_module_[id]_total%` | Number of module stats |
| `%bluearcade_stats_module_[id]_name%` | Module display name |
| `%bluearcade_stats_period_key_[period]%` | Current period key |

---

## Top Stats Placeholders

Use these placeholders to reproduce everything shown in the `/ba tops` menus.

### Totals (Stats List)

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_stats_total%` | Total stats shown in the list |
| `%bluearcade_tops_stats_global_total%` | Global stats count |
| `%bluearcade_tops_stats_module_total%` | Module stats count |
| `%bluearcade_tops_stats_module_count%` | Number of modules with stats |

### Totals (Categories)

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_categories_total%` | Total categories (global + modules) |

### Category Info

Format: `%bluearcade_tops_category_[category_id]_[field]%`

| Field | Description |
|-------|-------------|
| `name` | Category display name |
| `stat_total` | Number of stats inside the category |
| `scope` | Scope label (Global/Module) |
| `scope_key` | Scope key (`global` or `module`) |

**Examples:**
```
%bluearcade_tops_category_global_name%
%bluearcade_tops_category_race_stat_total%
%bluearcade_tops_category_spleef_scope%
```

### Stat Info

Format: `%bluearcade_tops_stat_[field]_[module_id]_[stat_key]_[period]%`

Use `core` or `global` as the module id for global stats.

| Field | Description |
|-------|-------------|
| `display` | Stat display name |
| `key` | Stat key |
| `full` | Full stat target (`module:stat` or `stat`) |
| `module_id` | Module id |
| `module_name` | Module display name |
| `scope` | Scope label |
| `scope_label` | Scope label alias |
| `scope_key` | Scope key |
| `value` | Player's stat value |

**Examples:**
```
%bluearcade_tops_stat_display_core_wins%
%bluearcade_tops_stat_full_race_wins%
%bluearcade_tops_stat_value_core_kills_monthly%
```

### Leaderboard Entries

Format: `%bluearcade_tops_entry_[field]_[module_id]_[stat_key]_[period]_[position]%`

Use `core` as the module id for global stats.

| Field | Description |
|-------|-------------|
| `player` | Player name at that position |
| `value` | Stat value at that position |

**Examples:**
```
%bluearcade_tops_entry_player_core_wins_monthly_1%
%bluearcade_tops_entry_value_race_wins_alltime_3%
```

### Leaderboard Totals

Format: `%bluearcade_tops_entry_total_[module_id]_[stat_key]_[period]%`

**Example:**
```
%bluearcade_tops_entry_total_core_wins_monthly%
```

### Period Labels

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_period_label_[period]%` | Period display name |

### Pagination Helpers

**Stats list:**

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_stats_page_total%` | Total pages |
| `%bluearcade_tops_stats_page_next_[page]%` | Next page |
| `%bluearcade_tops_stats_page_previous_[page]%` | Previous page |
| `%bluearcade_tops_stats_page_has_next_[page]%` | Has next page (true/false) |
| `%bluearcade_tops_stats_page_has_previous_[page]%` | Has previous page (true/false) |
| `%bluearcade_tops_stats_page_current_[page]%` | Current page (clamped) |

**Categories:**

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_categories_page_total%` | Total pages |
| `%bluearcade_tops_categories_page_next_[page]%` | Next page |
| `%bluearcade_tops_categories_page_previous_[page]%` | Previous page |
| `%bluearcade_tops_categories_page_has_next_[page]%` | Has next page (true/false) |
| `%bluearcade_tops_categories_page_has_previous_[page]%` | Has previous page (true/false) |
| `%bluearcade_tops_categories_page_current_[page]%` | Current page (clamped) |

**Leaderboard:**

Format: `%bluearcade_tops_page_[field]_[module_id]_[stat_key]_[period]_[page]%`

| Field | Description |
|-------|-------------|
| `total` | Total pages |
| `next` | Next page |
| `previous` | Previous page |
| `has_next` | Has next page (true/false) |
| `has_previous` | Has previous page (true/false) |
| `current` | Current page (clamped) |

**Examples:**
```
%bluearcade_tops_page_total_core_wins_monthly%
%bluearcade_tops_page_next_race_wins_alltime_2%
%bluearcade_tops_page_has_previous_core_kills_weekly_1%
```

---

## Usage Examples

### Scoreboard (using TAB or similar)

```yaml
lines:
  - "<gray>Playing: <green>%bluearcade_arena_current_game_display%"
  - "<gray>Round: <yellow>%bluearcade_arena_current_round%/%bluearcade_arena_current_round_max%"
  - ""
  - "<gray>Your Stars: <gold>%bluearcade_arena_current_player_stars%"
  - "<gray>Position: <aqua>#%bluearcade_arena_current_player_position%"
  - ""
  - "<gold>1st <white>%bluearcade_arena_current_podium_1%"
  - "<gray>2nd <white>%bluearcade_arena_current_podium_2%"
  - "<red>3rd <white>%bluearcade_arena_current_podium_3%"
```

### TAB Plugin — Team Color Prefix

Configure a custom prefix in your TAB plugin layout using the `player_team_prefix` placeholder. It returns a MiniMessage string when the player is in a team game, or an empty string otherwise.

```yaml
# config.yml (TAB plugin — MiniMessage mode)
players:
  _DEFAULT_:
    tabprefix: "%luckperms_prefix%%bluearcade_player_team_prefix%"
    tabsuffix: ""
```

The prefix symbol is configurable in `settings.yml`:

```yaml
game:
  global:
    ui:
      tab_list_team_prefix: "<team_color>█ "
```

### Hologram (using DecentHolograms or similar)

```yaml
lines:
  - "<gold><bold>Party Games</bold>"
  - "<gray>Arena #1"
  - ""
  - "<gray>Players: <green>%bluearcade_arena_1_players%/%bluearcade_arena_1_players_max%"
  - "<gray>State: <yellow>%bluearcade_arena_1_state%"
```

### NPC (using Citizens + PlaceholderAPI)

```
<gray>Wins: <green>%bluearcade_stat_global_wins%
<gray>Level: <aqua>%bluearcade_player_level%
```
