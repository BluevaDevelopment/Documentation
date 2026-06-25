# Private Worlds

Private worlds allow players to create and manage their own personal worlds with full access control, flags, and backups.

## Overview

- Each player can own **multiple private worlds**.
- Worlds are created from **templates**.
- Owners control who can visit, build, and interact.
- Worlds can be reset, backed up, and deleted.

## Creating a Private World

```
/pw create <template> [alias] [-f]
```

**Examples:**
```
/pw create void
/pw create skyblock "My Island"
/pw create survival "Survival World" -f
```

Arguments:
- `<template>` - Template to use
- `[alias]` - Custom display name
- `[-f]` - Force creation if alias exists

## Navigating Private Worlds

### Go to Your World

```
/pw goto [slot]
```

**Examples:**
```
/pw goto     # Opens GUI if enabled
/pw goto 1   # Go to world in slot 1
```

### Go to Another Player's World

```
/pw goto <player> [slot]
```

**Example:**
```
/pw goto Steve 2
```

### List Your Worlds

```
/pw list
```

### View World Info

```
/pw info [slot]
```

**Examples:**
```
/pw info     # Info for current world
/pw info 1   # Info for world in slot 1
```

## Managing Private Worlds

### Reset a World

Reset the world back to its template:

```
/pw reset <slot>
```

### Delete a World

```
/pw delete [slot]
```

You will be asked to confirm with `/pw confirm`.

## Access Control

Private worlds use a layered access model:

1. **Owner** - full control
2. **Trusted** - permanent access, can build/interact if flags allow
3. **Temporarily Added** - time-limited online access
4. **Whitelisted** - can enter when whitelist is enabled
5. **Visitor** - public access when whitelist is off
6. **Banned** - denied entry

### Trust a Player

Give permanent access:

```
/pw trust <player>
```

### Add a Player Temporarily

```
/pw add <player> [duration]
```

**Examples:**
```
/pw add Steve
/pw add Steve 1h
/pw add Steve 30m
/pw add Steve 7d
```

Duration formats: `m` (minutes), `h` (hours), `d` (days).

### Remove Access

```
/pw remove <player>
```

### Whitelist

```
/pw whitelist on
/pw whitelist off
/pw whitelist add <player>
/pw whitelist remove <player>
/pw whitelist list
/pw whitelist status
```

**Whitelist behavior:**
- **ON**: only owner + trusted + whitelisted players can enter
- **OFF**: anyone can enter unless banned

### View Members

```
/pw members
```

Shows owner, trusted players, temporary players, whitelisted players, and banned players.

## Moderation

### Kick a Player

```
/pw kick <player> [reason]
```

### Ban a Player

```
/pw ban <player> [reason]
```

### Temporarily Ban a Player

```
/pw tempban <player> <duration> [reason]
```

**Examples:**
```
/pw tempban Steve 1h Spam
/pw tempban Steve 1d Griefing
```

### Unban a Player

```
/pw unban <player>
```

### View Moderation History

```
/pw history [player]
```

## Search

Search public private worlds by tags:

```
/pw search [tags] [page]
```

**Examples:**
```
/pw search
/pw search pvp
/pw search pvp,survival 2
```

## Admin Commands

Administrators can manage any private world:

```
/pw admin info <world_id>
/pw admin list [page]
/pw admin goto <world_id>
/pw admin delete <world_id>
/pw admin backup <world_id> <action>
```

## Tips

1. **Set flags early** after creating a world.
2. **Trust carefully**; use `/pw add` for temporary visitors.
3. **Enable whitelist** for exclusive/private worlds.
4. **Back up important worlds** before major changes.
5. **Use meaningful aliases and descriptions** to help players find worlds via `/pw search`.

## Troubleshooting

**"You have reached the world limit"**
- The player has reached the maximum number of private worlds.
- Adjust limits in `settings.yml` if needed.

**"Template not found"**
- Verify the template exists with `/bw template list`.
- Check spelling and case sensitivity.

**"Cannot build"**
- Check the `break` and `place` flags.
- Verify the player is trusted or added.
- Check flag override permissions.
