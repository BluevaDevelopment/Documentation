# Leaderboards & Statistics

Blue Arcade tracks comprehensive statistics and provides leaderboards for competitive ranking.

## Overview

- **Statistics**: Track player performance across games
- **Leaderboards (Tops)**: Rank players by various stats
- **Time Periods**: All-time, daily, weekly, monthly, yearly
- **Scopes**: Global (all games) and module-specific

**Permission Required:** `bluearcade.stats`

---

## Commands

### View Your Stats

```
/ba stats
```
Opens the statistics menu showing your stats.

```
/ba stats [category]
```
View stats for a specific category (global or module ID).

```
/ba stats [category] [period]
```
View stats for a specific time period.

**Examples:**
```
/ba stats global alltime
/ba stats race weekly
/ba stats spleef monthly
```

### View Leaderboards

```
/ba tops
```
Opens the leaderboards menu.

```
/ba tops [category]
```
View leaderboard categories.

```
/ba tops [stat] [period]
```
View ranking for a specific stat and period.

**Examples:**
```
/ba tops wins alltime
/ba tops kills weekly
/ba tops race:wins monthly
```

### Pagination

```
/ba stats page [number]
/ba tops page [number]
/ba tops [stat] [period] page [number]
```

---

## Time Periods

| Period | Description |
|--------|-------------|
| `alltime` | Since statistics began tracking |
| `daily` | Today only (resets at midnight) |
| `weekly` | This week (resets on Sunday) |
| `monthly` | This month (resets on 1st) |
| `yearly` | This year (resets on Jan 1st) |

---

## Global Statistics

Statistics tracked across all minigames:

| Stat | Description |
|------|-------------|
| `games_played` | Total games played |
| `mini_games_played` | Number of different minigames played |
| `wins` | First place finishes |
| `second_place` | Second place finishes |
| `third_place` | Third place finishes |
| `stars` | Total stars earned |
| `kills` | Total PvP kills |
| `deaths` | Total deaths |

---

## Module Statistics

Each minigame module can track its own statistics:

### Race Module
| Stat | Description |
|------|-------------|
| `wins` | Races won |
| `games_played` | Races played |
| `finish_line_crosses` | Times crossing finish line |

### Combat Modules (Snowball Fight, All Against All, etc.)
| Stat | Description |
|------|-------------|
| `wins` | Games won |
| `games_played` | Games played |
| `kills` | Kills in this mode |
| `deaths` | Deaths in this mode |

*Each module defines its own stats. Check the module's documentation.*

---

## Stat Scopes

### Global Scope
- Aggregated across all minigames
- Example: Total wins from all games

### Module Scope
- Specific to one minigame
- Example: Wins only in Race

### Accessing Module Stats in Commands

Use the format `module:stat`:

```
/ba tops race:wins alltime
/ba tops spleef:games_played weekly
```

---

## Leaderboard Display

The leaderboard shows:

1. **Position** - Ranking number
2. **Player Name** - Who holds this position
3. **Value** - The stat value

### Menu View

- Browse categories
- Select stat and period
- View top players
- See your own rank

### Text View

If menus are disabled, stats display in chat with pagination.

---

## Stars System

Stars are earned during games based on performance:

| Position | Stars |
|----------|-------|
| 1st Place | 3 stars |
| 2nd Place | 2 stars |
| 3rd Place | 1 star |

Stars determine the winner in party mode arenas.

---

## Configuration

Statistics settings in `settings.yml`:

```yaml
stats:
  enabled: true
  tops:
    page_size: 10
```

---

## PlaceholderAPI Integration

Access stats via placeholders:

```
%bluearcade_stat_global_wins%
%bluearcade_stat_global_kills_weekly%
%bluearcade_stat_module_race_wins%
```

See [Placeholders](placeholders.md) for complete list.

---

## Admin Commands

### View Player Stats

Administrators can check any player's stats through the database or PlaceholderAPI.

### Reset Stats

Stats cannot be reset via commands. Contact support if needed.

---

## Data Storage

Statistics are stored per-player in:
```
plugins/BlueArcade3/data/users/[uuid].json
```

Data includes:
- All stat values
- Period-based tracking
- Last update timestamps

---

## Tips

1. **Check daily leaderboards** - Compete for daily rankings
2. **Specialize** - Focus on specific minigames to top those leaderboards
3. **Watch periods reset** - Get an early lead at the start of each period
4. **Balance stats** - Some achievements require varied statistics

---

## Troubleshooting

**"Stats are disabled"**
- Statistics tracking is turned off in settings
- Contact server administrator

**"No stats found"**
- You haven't played any games yet
- Play some games to start tracking

**"Leaderboard empty"**
- No players have stats for this category/period
- Try a different period (alltime usually has data)

**"Module not found"**
- The module ID is incorrect
- Use `/ba stats` to see available categories
