# Discovery

The public directory of houses. `/h` outside a house opens it, and `/h discovery` opens it anywhere.

**A house is listed when its whitelist is off.** There is no separate "public" switch.

## Five Rows, Not One List

Discovery is not one grid sorted one way. It is five rows, each one a channel the player chose, and each row scrolls sideways on its own arrows, so turning a page moves one row instead of throwing the other four away.

The row choice is saved per player. Where a row is scrolled to is not: that is where you are in a list right now, not something to find again tomorrow.

Bedrock gets the same channels as one scrolling form. A form has no grid, and one list means every channel is a tap away.

## Channels

A channel is configuration, not code. It is a **source**, plus two optional narrowings:

```yaml
discovery:
  channels:
    vip_parkour:
      source: rated
      tag: parkour
      condition: "%luckperms_primary_group% == vip"
```

| Source | Orders by |
|--------|-----------|
| `online` | Players inside right now |
| `visits` | Total visits |
| `likes` | Likes |
| `rated` | Average rating |
| `cookies_weekly` | Gifts received this ISO week |
| `cookies` | Gifts received in total |
| `new` | Newest first |
| `recommended` | A mix the plugin picks |
| `revisit` | Houses this player has been to before |
| `social` | Houses this player's friends built |
| `language` | Houses whose owner plays in the same language |
| `random` | Shuffled |
| `featured` | Pinned by staff with `/ha feature` |

**`tag`** narrows to one house category. **`condition`** narrows by the *owner*, in one of two forms:

| Form | Needs | Note |
|------|-------|------|
| `perm:<node>` | nothing | Can only be answered for an owner who is **online** |
| A PlaceholderAPI expression | PlaceholderAPI | `%luckperms_primary_group% == vip`, `%vault_rank% contains MVP`, or a bare `%flag%` read as yes/no |

This is why there are no built-in staff, creator or rank channels: they are a `condition`.

Without PlaceholderAPI an expression **empties its channel** and says so once. A permissive default would fill it with every house on the server the moment the dependency went missing.

The language channel reads the client's own locale, recorded on join, so there is no command and no profile page.

Names and descriptions come from `language.yml` (`discovery.channels.<id>`); setting `name`/`description` in the channel overrides them.

## Likes, Ratings and Gifts

Three different things, kept apart.

| | Command | What it is |
|---|---------|-----------|
| **Like** | `/h like` | One bit per player |
| **Rating** | `/h rate 0-5` | An opinion, stored by rater, replaceable |
| **Gift** | `/h cookie` | Spent, not free |

A rating is stored **by rater**, so re-rating replaces rather than adds, and nobody can push a number they cannot be found by.

A gift is a number the player carries, not an item. Items were the first attempt: they sat in the inventory a house swaps out from under you, took a slot, and could be thrown away by somebody who did not recognise them. `discovery.cookies.item` is only what a gift is **drawn** as: an end-themed server sets ender pearls and renames it in `language.yml`.

Gifts are spent before they are counted and credited back if the count fails, so one is never free and never paid for twice. The menu asks how many.

They are counted three ways: total, **per ISO week** (what `cookies_weekly` ranks by, so a house popular two years ago does not sit at the top for ever) and per giver.

### Where gifts come from

```yaml
discovery:
  cookies:
    allowance:
      amount: 3
      every_hours: 24
```

Worked out when the player asks rather than on a schedule, so a server that was off for a week owes one handout and not a week of them. `bluehousing.house.cookie.allowance.<n>` raises it for a rank; the highest wins, and the configured value is the floor.

## Categories and Icons

Both are [flags](flags.md):

```
/h flag set category parkour
/h flag set icon PINK_CONCRETE
```

Categories come from `discovery.categories` in `settings.yml` and one the server never declared is refused, because a house filed under a filter no menu offers looks exactly like a house that is not listed.

The icon picker asks the server for its material list, so a Minecraft version that adds blocks offers them the day it is installed.

## The Visitor Item

A visitor gets a compass that opens the visitor menu: rate, gift, like, donate, silence the music, leave.

It is the plugin's item rather than a script's on purpose: none of that should stop working because an owner deleted a file. It is handed out **by role** (`discovery.visitor_item.excluded_roles`, `owner` and `trusted` by default) so it never lands in the same slot as the house's own menu item.

## Commands

| Command | Description |
|---------|-------------|
| `/h discovery` | Open the directory |
| `/h discovery popular\|liked\|recent\|online` | Jump to a sort |
| `/h discovery category <name>` | Filter by category |
| `/h discovery search <text>` | Search by name |
| `/h browse` | The browse list |
| `/h search` | The search menu |
| `/ha feature <house>` | Pin a house to the top |
