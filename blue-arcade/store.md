# Store System

The store allows players to spend credits on cosmetic items like victory effects, death effects, and more.

## Overview

- **Credits**: In-game currency earned by playing games
- **Categories**: Victory Effects, Death Effects, Kill Effects, Victory Music
- **Items**: Cosmetics that can be purchased and selected

**Permission Required:** `bluearcade.store`

---

## Commands

### Opening the Store

```
/ba store
```
Opens the main store menu.

```
/ba store page [number]
```
Navigate to a specific page.

### Browsing Categories

```
/ba store category [category_id] (page)
```
View items in a specific category.

### Purchasing Items

```
/ba store buy [category] [item]
```
Opens purchase confirmation for an item.

### Selecting Items

```
/ba store select [category] [item]
```
Select an owned item as your active choice.

### Viewing Credits

```
/ba credits
```
Shows your current credit balance.

---

## Store Categories

### Victory Effects

Celebrate your wins with special effects!

| Effect | Description |
|--------|-------------|
| Fireworks | Classic firework display |
| Notes | Musical notes particle shower |
| Vulcan Fire | Fiery volcanic explosion |
| Star Shower | Falling stars around you |
| Titan | Giant player appearance |
| Meteors | Meteors crashing around you |
| Wither Rider | Ride a wither briefly |
| Chicken | Chicken explosion |
| Daredevil | Motorcycle stunt display |
| Wardens | Warden spawn celebration |
| *And more...* | |

### Death Effects

Effects that play when you die:

- Particle explosions
- Block effects
- Sound effects

### Kill Effects

Effects that play when you eliminate another player:

- Particle effects
- Visual indicators
- Sound effects

### Victory Music

Custom music that plays during victory celebrations:
- Requires NoteBlockAPI plugin
- Various themed music options

---

## Earning Credits

Players earn credits by:
- Playing games
- Winning games
- Completing achievements
- Admin rewards (`/baa credits give`)

Credit rewards are configured in the plugin settings.

---

## Economy Integration

Blue Arcade can use Vault for economy integration:

**With Vault:**
- Uses your server's economy (Essentials, CMI, etc.)
- Shared currency across plugins

**Without Vault:**
- Uses built-in credits system
- Credits are Blue Arcade-specific

Configure in `settings.yml`:
```yaml
economy:
  provider: internal  # or "vault"
```

---

## Admin Commands

### Give Credits

```
/baa credits give [player] [amount]
/baa credits give [player] [amount] -s
```

Give credits to a player. Use `-s` for silent mode (no notification).

**Examples:**
```
/baa credits give Steve 100
/baa credits give Alex 500 -s
```

---

## Store Configuration

The store is configured in various YAML files:

- `victory_effects.yml` - Victory effect items
- `death_effects.yml` - Death effect items
- `kill_effects.yml` - Kill effect items
- `victory_music.yml` - Victory music tracks

Each item defines:
- Display name
- Description
- Price
- Icon
- Effect type

---

## Item States

When browsing the store, items show different states:

| State | Meaning |
|-------|---------|
| **Locked** | Not purchased yet |
| **Owned** | Purchased but not selected |
| **Selected** | Currently active |

---

## Troubleshooting

**"Insufficient credits"**
- You don't have enough credits
- Play more games to earn credits

**"Store is disabled"**
- The store has been disabled in settings
- Contact server administrator

**"Item not found"**
- The item ID is incorrect
- Check available items in the store menu

**"Already owned"**
- You already purchased this item
- Use `/ba store select` to activate it
