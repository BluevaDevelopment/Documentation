# PlaceholderAPI Placeholders

Blue Arcade 3.0 provides extensive PlaceholderAPI integration for use in scoreboards, holograms, and other plugins.

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

### Team (in-game)

These placeholders return an empty string (`""`) when the player is not in an arena or is in a game with no teams (solo mode).

All values use **MiniMessage** (Adventure) format.

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_player_team_id%` | Team identifier (e.g. `red`, `blue`) |
| `%bluearcade_player_team_name%` | Team display name as a MiniMessage string (e.g. `<red>Red Team</red>`) |
| `%bluearcade_player_team_color%` | MiniMessage color tag of the team (e.g. `<red>` or `<#FF5555>`) |
| `%bluearcade_player_team_prefix%` | Formatted prefix component serialized as MiniMessage — the symbol and color come from the `game.global.ui.tab_list_team_prefix` setting in `settings.yml` (e.g. `<red>█ </red>`) |

**Customising the prefix symbol**  
 Edit `game.global.ui.tab_list_team_prefix` in `settings.yml`. Use full MiniMessage syntax.  
 The special tag `<team_color>` is replaced at runtime with the team's own color.  
 ```yaml
 # settings.yml
 game:
   global:
     ui:
       tab_list_team_prefix: "<team_color>█ "   # default
       # other examples:
       # "<bold><team_color>■</bold> "
       # "<red>★ "   (always red, ignores team color)
 ```

### Economy & Progress

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_credits%` | Player's credit balance |
| `%bluearcade_player_level%` | BlueArcade level (or Minecraft level if disabled) |
| `%bluearcade_player_exp%` | BlueArcade experience (or Minecraft XP if disabled) |

---

## Statistics Placeholders

### Global Stats

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_stat_global_games_played%` | Total games played |
| `%bluearcade_stat_global_mini_games_played%` | Different minigames played |
| `%bluearcade_stat_global_wins%` | Total first places |
| `%bluearcade_stat_global_second_place%` | Total second places |
| `%bluearcade_stat_global_third_place%` | Total third places |
| `%bluearcade_stat_global_stars%` | Total stars earned |
| `%bluearcade_stat_global_kills%` | Total kills |
| `%bluearcade_stat_global_deaths%` | Total deaths |

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
| `%bluearcade_arenas_running%` | Arenas in running state |
| `%bluearcade_arenas_restarting%` | Arenas restarting |
| `%bluearcade_arenas_ids%` | Comma-separated arena IDs |

---

## Arena Placeholders

### Current Arena (Player's Arena)

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_arena_current_id%` | Arena ID |
| `%bluearcade_arena_current_name%` | Arena display name |
| `%bluearcade_arena_current_state%` | Arena state (waiting/running/restarting) |
| `%bluearcade_arena_current_status%` | Arena status (enabled/disabled) |
| `%bluearcade_arena_current_players%` | Current player count |
| `%bluearcade_arena_current_players_max%` | Maximum players |
| `%bluearcade_arena_current_players_min%` | Minimum players |
| `%bluearcade_arena_current_round%` | Current round number |
| `%bluearcade_arena_current_round_max%` | Maximum rounds |
| `%bluearcade_arena_current_countdown%` | Countdown seconds |
| `%bluearcade_arena_current_countdown_formatted%` | Countdown (MM:SS) |
| `%bluearcade_arena_current_game_id%` | Current minigame ID |
| `%bluearcade_arena_current_game_display%` | Current minigame name |
| `%bluearcade_arena_current_forced_start%` | Force started (true/false) |
| `%bluearcade_arena_current_available_minigames%` | Available minigames list |
| `%bluearcade_arena_current_played_minigames%` | Played minigames list |

### Podium & Stars

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_arena_current_podium_1%` | 1st place player name |
| `%bluearcade_arena_current_podium_2%` | 2nd place player name |
| `%bluearcade_arena_current_podium_3%` | 3rd place player name |
| `%bluearcade_arena_current_stars_1%` | Stars for 1st place |
| `%bluearcade_arena_current_stars_2%` | Stars for 2nd place |
| `%bluearcade_arena_current_stars_3%` | Stars for 3rd place |
| `%bluearcade_arena_current_player_position%` | Your podium position |
| `%bluearcade_arena_current_player_stars%` | Your stars in arena |
| `%bluearcade_arena_current_stars_total%` | Total stars in arena |
| `%bluearcade_arena_current_stars_average%` | Average stars |

### Voting

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_arena_current_vote_[minigame]%` | Votes for a specific minigame |
| `%bluearcade_arena_current_votes_total%` | Total votes cast |

### Specific Arena (by ID)

Replace `current` with the arena ID:

```
%bluearcade_arena_1_name%
%bluearcade_arena_1_state%
%bluearcade_arena_1_players%
%bluearcade_arena_2_game_display%
```

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_arena_[id]_exists%` | Arena exists (true/false) |
| `%bluearcade_arena_[id]_name%` | Arena display name |
| `%bluearcade_arena_[id]_state%` | Arena state |
| `%bluearcade_arena_[id]_status%` | Arena status |
| `%bluearcade_arena_[id]_players%` | Player count |
| `%bluearcade_arena_[id]_players_max%` | Max players |

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

Use these placeholders to reproduce everything shown in the `/ba tops` menus (categories, stat lists, and leaderboards).

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

### Stat Info (For Leaderboards)

Format: `%bluearcade_tops_stat_[field]_[module_id]_[stat_key]_[period?]%`

Use `core` as the module id for global stats.

| Field | Description |
|-------|-------------|
| `display` | Stat display name |
| `key` | Stat key |
| `full` | Full stat target (`module:stat` or `stat`) |
| `module_id` | Module id (`global` if global stat) |
| `module_name` | Module display name |
| `scope` | Scope label (Global/Module) |
| `scope_label` | Scope label (alias of `scope`) |
| `scope_key` | Scope key (`global` or `module`) |
| `value` | Player's stat value (uses optional period) |

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

Use `core` as the module id for global stats.

**Example:**
```
%bluearcade_tops_entry_total_core_wins_monthly%
```

### Period Labels

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_period_label_[period]%` | Period display name |

### Pagination Helpers

These helpers mirror the values used by the `/ba tops` menus. When a page parameter is required, the menu will be clamped to a valid page range.

**Stats list (menu: `tops_menu`):**

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_stats_page_total%` | Total pages |
| `%bluearcade_tops_stats_page_next_[page]%` | Next page |
| `%bluearcade_tops_stats_page_previous_[page]%` | Previous page |
| `%bluearcade_tops_stats_page_has_next_[page]%` | Has next page (true/false) |
| `%bluearcade_tops_stats_page_has_previous_[page]%` | Has previous page (true/false) |
| `%bluearcade_tops_stats_page_current_[page]%` | Current page (clamped) |

**Categories (menu: `tops_categories`):**

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_tops_categories_page_total%` | Total pages |
| `%bluearcade_tops_categories_page_next_[page]%` | Next page |
| `%bluearcade_tops_categories_page_previous_[page]%` | Previous page |
| `%bluearcade_tops_categories_page_has_next_[page]%` | Has next page (true/false) |
| `%bluearcade_tops_categories_page_has_previous_[page]%` | Has previous page (true/false) |
| `%bluearcade_tops_categories_page_current_[page]%` | Current page (clamped) |

**Leaderboard (menu: `tops_list_menu`):**

Format: `%bluearcade_tops_page_[field]_[module_id]_[stat_key]_[period]_[page?]%`

Use `core` as the module id for global stats.

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
  - "&7Playing: &a%bluearcade_arena_current_game_display%"
  - "&7Round: &e%bluearcade_arena_current_round%/%bluearcade_arena_current_round_max%"
  - ""
  - "&7Your Stars: &6%bluearcade_arena_current_player_stars%"
  - "&7Position: &b#%bluearcade_arena_current_player_position%"
  - ""
  - "&61st &f%bluearcade_arena_current_podium_1%"
  - "&72nd &f%bluearcade_arena_current_podium_2%"
  - "&c3rd &f%bluearcade_arena_current_podium_3%"
```

### TAB plugin — team color prefix in the tab list

To display the team color indicator in the TAB list, configure a custom prefix in your TAB plugin layout using the `player_team_prefix` placeholder. It returns a MiniMessage string (e.g. `<red>█ </red>`) when the player is in a team game, or an empty string otherwise — safe to use globally.

```yaml
# config.yml (TAB plugin — MiniMessage mode)
players:
  _DEFAULT_:
    tabprefix: "%bluearcade_player_team_prefix%"
    tabsuffix: ""
```

You can also combine it with a rank prefix from LuckPerms or another permissions plugin:

```yaml
tabprefix: "%luckperms_prefix%%bluearcade_player_team_prefix%"
```

The prefix symbol and default color are configurable in `settings.yml`:

```yaml
# settings.yml
game:
  global:
    ui:
      tab_list_team_prefix: "<team_color>█ "
```

### Hologram (using DecentHolograms or similar)

```yaml
lines:
  - "&6&lParty Games"
  - "&7Arena #1"
  - ""
  - "&7Players: &a%bluearcade_arena_1_players%/%bluearcade_arena_1_players_max%"
  - "&7State: &e%bluearcade_arena_1_state%"
```

### NPC (using Citizens + PlaceholderAPI)

```
&7Wins: &a%bluearcade_stat_global_wins%
&7Level: &b%bluearcade_player_level%
```
