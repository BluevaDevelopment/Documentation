# Access Control

Blue Worlds provides a layered access control system for private worlds, allowing owners to manage who can visit, build, and interact.

## Access Hierarchy

Private worlds use the following access levels:

1. **Owner** - full control over the world
2. **Trusted** - permanent access, can build/interact if flags allow
3. **Temporarily Added** - time-limited online access
4. **Whitelisted** - can enter when whitelist is enabled
5. **Visitor** - public access when whitelist is off
6. **Banned** - denied entry

## Trust System

### Trust a Player

Give a player permanent access to your world:

```
/pw trust <player>
```

**Example:**
```
/pw trust Steve
```

Trusted players keep access across server restarts and can build/interact based on world flags.

### Remove Trust

```
/pw remove <player>
```

This removes trust, temporary access, and whitelist status.

## Temporary Access

### Add a Player Temporarily

Give a player time-limited access:

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

Duration formats:
- `m` - minutes
- `h` - hours
- `d` - days

### Remove Temporary Access

```
/pw remove <player>
```

## Whitelist

The whitelist controls who can enter your world when it is enabled.

### Enable/Disable Whitelist

```
/pw whitelist on
/pw whitelist off
```

### Manage Whitelist Players

```
/pw whitelist add <player>
/pw whitelist remove <player>
/pw whitelist list
/pw whitelist status
```

### Whitelist Behavior

- **ON**: only owner + trusted + whitelisted players can enter
- **OFF**: anyone can enter unless banned

## Moderation

### Kick a Player

Remove a player from your world:

```
/pw kick <player> [reason]
```

**Examples:**
```
/pw kick Steve
/pw kick Steve Griefing
```

### Ban a Player

Permanently ban a player from your world:

```
/pw ban <player> [reason]
```

**Example:**
```
/pw ban Steve Abusive behavior
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

**Examples:**
```
/pw history          # All history
/pw history Steve    # Steve's history
```

## Viewing Members

List all players with access to your world:

```
/pw members
```

This shows:
- Owner
- Trusted players
- Temporarily added players
- Whitelisted players
- Banned players

## Admin Bypass

Administrators can bypass access restrictions with these permissions:

| Permission | Description |
|------------|-------------|
| `blueworlds.admin.bypass.access` | Bypass access restrictions |
| `blueworlds.admin.bypass.build` | Bypass build restrictions |

## Tips

1. **Trust carefully** - trusted players keep access permanently.
2. **Use `/pw add`** for temporary visitors or builders.
3. **Enable whitelist** for exclusive or private worlds.
4. **Document bans** with reasons using `/pw history`.
5. **Use moderation history** to track repeated offenders.

## Troubleshooting

**"Player cannot enter"**
- Check if whitelist is enabled and the player is whitelisted.
- Check if the player is banned.
- Verify world flags allow visitors.

**"Trusted player cannot build"**
- Check `break` and `place` flags.
- Verify the player is actually trusted with `/pw members`.
- Check flag override permissions.

**"Temporary access expired"**
- Re-add the player with `/pw add <player> <duration>`.
