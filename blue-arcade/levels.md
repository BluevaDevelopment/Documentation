# XP & Levels System

Blue Arcade includes a progression system with experience points and levels.

## Overview

- **Experience (XP)**: Points earned by playing games
- **Levels**: Milestones reached by accumulating XP
- **Titles**: Display titles based on level ranges
- **Progress Bar**: Visual representation of level progress

**Permission Required:** `bluearcade.level`

---

## Commands

### View Your Level

```
/ba level
```

Shows:
- Current level
- Current title
- XP progress
- XP needed for next level
- Progress bar

---

## How Leveling Works

### Earning Experience

Players earn XP by:
- Playing games
- Winning games
- Completing achievements
- Admin rewards

### Level Calculation

XP required for each level follows an exponential formula:

```
Required XP = base_xp * (1 + growth_percent)^level
```

**Default Values:**
- Base XP: 800
- Growth: 15% per level

**Example:**
- Level 1: 800 XP
- Level 2: 920 XP
- Level 3: 1,058 XP
- Level 10: 2,918 XP
- Level 50: 73,558 XP

### Leveling Up

When you accumulate enough XP:
1. Level increases automatically
2. XP counter resets for next level
3. New title may be unlocked
4. Notification is shown

---

## Titles

Titles are display names based on level ranges:

| Level Range | Title |
|-------------|-------|
| 1-5 | Rookie |
| 6-15 | Apprentice |
| 16-30 | Skilled |
| 31-50 | Veteran |
| 51-75 | Expert |
| 76-100 | Master |
| 100+ | Legend |

*Titles are configurable by server administrators.*

---

## Progress Bar

The level command shows a visual progress bar:

```
[████████░░░░░░░░░░░░] 42%
```

Configuration options:
- Bar length
- Filled character
- Empty character

---

## Reset Periods

Levels can optionally reset on a schedule:

| Period | Description |
|--------|-------------|
| `permanent` | Levels never reset (default) |
| `yearly` | Reset on January 1st |
| `monthly` | Reset on the 1st of each month |

When reset occurs:
- Level returns to 1
- XP counter resets
- Previous progress is not recoverable

---

## Admin Commands

### Give Experience

```
/baa xp give [player] [amount]
/baa xp give [player] [amount] -s
```

Give XP to a player. Use `-s` for silent mode.

**Examples:**
```
/baa xp give Steve 500
/baa xp give Alex 1000 -s
```

---

## Configuration

Level settings in `settings.yml`:

```yaml
levels:
  enabled: true
  base_xp: 800
  growth_percent: 15
  period: permanent  # permanent, yearly, monthly
  progress_bar:
    length: 20
    filled: "█"
    empty: "░"
  titles:
    rookie:
      min_level: 1
      max_level: 5
    apprentice:
      min_level: 6
      max_level: 15
    # ... more titles
```

---

## PlaceholderAPI Integration

| Placeholder | Description |
|-------------|-------------|
| `%bluearcade_player_level%` | Current level |
| `%bluearcade_player_exp%` | Current XP |

If levels are disabled, these return Minecraft vanilla level/XP.

---

## Integration with Other Systems

### Achievements

Achievements can reward XP:
```yaml
rewards:
  exp: 500
```

### Store

Store items cannot be purchased with XP (only credits).

### Leaderboards

Level is not tracked on leaderboards by default, but XP can be added as a custom stat.

---

## Tips

1. **Play regularly** - XP accumulates over time
2. **Win games** - Winners typically earn more XP
3. **Complete achievements** - Extra XP bonuses
4. **Check progress** - Use `/ba level` to see how close you are

---

## Troubleshooting

**"Levels are disabled"**
- The leveling system is turned off
- Contact server administrator

**"XP not updating"**
- XP is granted after games complete
- Make sure you finish games (don't quit early)

**"Level not increasing"**
- You may not have enough XP yet
- Check required XP with `/ba level`

**"Title not changing"**
- Titles only change at specific level thresholds
- Keep leveling to unlock new titles
