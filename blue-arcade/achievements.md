# Achievements System

Track player progress and reward milestones with the achievements system.

## Overview

- **Achievements**: Goals players can complete
- **Phases**: Multi-step progression within achievements
- **Categories**: Organized groups of related achievements
- **Rewards**: XP, credits, and stars for completion

**Permission Required:** `bluearcade.achievements`

---

## Commands

### Viewing Achievements

```
/ba achievements
```
Opens the main achievements menu showing all categories.

```
/ba achievements [category]
```
View achievements in a specific category.

```
/ba achievements [category] [achievement]
```
View details and phases of a specific achievement.

### Pagination

```
/ba achievements page [number]
/ba achievements [category] page [number]
```

---

## How Achievements Work

### Categories

Achievements are organized into categories:
- **Global**: Available across all minigames
- **Module-specific**: Tied to specific minigames

### Phases

Achievements can have multiple phases (tiers):

**Example: "Winner" Achievement**
- Phase 1: Win 1 game
- Phase 2: Win 10 games
- Phase 3: Win 50 games
- Phase 4: Win 100 games

Each phase has its own:
- Requirements
- Rewards
- Completion status

### Conditions

Achievement conditions use PlaceholderAPI expressions:

```yaml
condition: "%bluearcade_stat_global_wins% >= 10"
```

Conditions are evaluated after games complete.

---

## Rewards

Completing achievement phases grants rewards:

| Reward Type | Description |
|-------------|-------------|
| **Experience** | XP for the leveling system |
| **Credits** | Currency for the store |
| **Stars** | Ranking points |

Rewards are configured per phase, allowing increasing rewards for harder phases.

---

## Achievement States

| State | Description |
|-------|-------------|
| **Locked** | Not yet achieved |
| **In Progress** | Working towards next phase |
| **Completed** | All phases finished |

---

## Global Achievements

Available to all players regardless of minigame:

| Achievement | Description |
|-------------|-------------|
| First Steps | Play your first game |
| Dedicated | Play multiple games |
| Winner | Win games |
| Champion | Reach top positions |
| Collector | Earn stars |
| *And more...* | |

---

## Module Achievements

Each minigame module can define its own achievements:

**Race Module:**
- Speed Demon: Complete races quickly
- Marathon Runner: Play many races
- First Place: Win races

**Spleef Module:**
- Block Breaker: Break blocks
- Survivor: Survive without falling
- Spleef Master: Win spleef games

*Check each module's store page for its specific achievements.*

---

## Configuration

### Global Achievements

Located in `achievements.yml`:

```yaml
categories:
  general:
    display_name: "General"
    icon: DIAMOND
    achievements:
      first_game:
        display_name: "First Steps"
        description: "Play your first game"
        phases:
          1:
            condition: "%bluearcade_stat_global_games_played% >= 1"
            rewards:
              exp: 100
              credits: 50
```

### Module Achievements

Each module has its own `achievements.yml` in its config folder.

---

## Achievement Menu

The achievements menu shows:

1. **Category Overview**
   - Category icon and name
   - Completion percentage
   - Number of completed achievements

2. **Achievement List**
   - Achievement icon
   - Name and description
   - Current phase progress
   - Completion status

3. **Phase Details**
   - Phase requirements
   - Rewards for completion
   - Current progress

---

## Tips

1. **Check regularly** - New achievements may be added with updates
2. **Focus on phases** - Complete one phase at a time
3. **Play variety** - Different minigames have different achievements
4. **Collect rewards** - Don't forget to claim your earned rewards

---

## Troubleshooting

**"No achievements found"**
- The achievements system may be disabled
- Check if the category exists

**"Achievement not updating"**
- Achievements update after games complete
- Some achievements require specific actions

**"Rewards not received"**
- Check if the phase was actually completed
- Some rewards require manual claim
