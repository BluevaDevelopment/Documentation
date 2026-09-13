# Flags

Flags are the rules a house plays by. Every one of them can be set from the menu, from a command, from a Bedrock form and from Lua, and all four go through the same write, so none of them can drift.

```
/h flag set <flag> <value> [slot]
/h flag get <flag>
/h flag remove <flag>              # back to the server default
/h flag list
/h flag info <flag>
```

A house is addressed by slot, so `/h flag set pvp false 2` works from anywhere. Prose flags (`alias`, `description`, `title`, `subtitle`, `notify-enter`, `notify-leave`) take the rest of the line, so names with spaces work; quoting is supported when something must follow.

Server defaults live under `default_flags:` in `settings.yml`. Per-flag permissions are `bluehousing.house.flag.<flag>` for owners and `bluehousing.admin.flag.<flag>` for staff.

---

## Build & Interact

Enforced on the event. All of them accept `true` / `false`.

| Flag | Default | Description |
|------|---------|-------------|
| `break` | `true` | Break blocks |
| `place` | `true` | Place blocks |
| `interact` | `true` | General interaction |
| `use` | `true` | Use items |
| `pickup` | `true` | Pick items up |
| `chest-access` | `true` | Open containers |
| `door-interact` | `true` | Doors, trapdoors, gates |
| `lever-interact` | `true` | Levers and buttons |

| Flag | Values | Default | Description |
|------|--------|---------|-------------|
| `drops` | `all`, `survival`, `creative`, `none` | `all` | Who may throw items on the floor. `survival` includes adventure. Applies to the owner too, because it is a house rule and not a protection. |

> The owner and staff in admin mode bypass the eight protection flags above, so testing one on yourself always looks as if it does nothing. `drops` has no bypass.

## Abilities

Applied to the player and taken back when they leave, so a house cannot leave somebody fast or flying elsewhere.

| Flag | Values | Default | Description |
|------|--------|---------|-------------|
| `gamemode` | `survival`, `creative`, `adventure`, `spectator` | unset | Gamemode on entry |
| `fly` | `true` / `false` | unset | Grant or revoke flight |
| `speed` | `1`–`10` | unset | `1` is vanilla speed, `10` the maximum. Walking and flying are scaled from their own defaults |
| `god-mode` | `true` / `false` | `false` | No damage |

## Environment

| Flag | Values | Default | Description |
|------|--------|---------|-------------|
| `time` | `dawn`, `day`, `dusk`, `night`, or ticks | unset | Locks the time of day |
| `weather-change` | `true` / `false` | `true` | Allow weather to change |
| `difficulty` | `peaceful`, `easy`, `normal`, `hard` | `normal` | House difficulty |
| `mob-spawning` | `true` / `false` | `false` | Natural spawning of monsters and animals |
| `world-border` | size in blocks, or `auto` | `60000000` | House border |
| `spawn` | `x,y,z[,yaw,pitch]` or `here` | unset | Where players arrive |

`here` only works while you are standing **in that house**. Addressing another house by slot and typing `here` is refused rather than storing the lobby's coordinates as that house's spawn.

## Safety

| Flag | Default | Description |
|------|---------|-------------|
| `pvp` | `true` | Player versus player |
| `explosion` | `true` | Explosions damage the world |
| `fire-spread` | `true` | Fire spreads |

## Social

| Flag | Values | Default | Description |
|------|--------|---------|-------------|
| `chat` | `true` / `false` | `true` | Chat inside the house |
| `titles` | `true` / `false` | `true` | Show the entry title |
| `title` | text | unset | Entry title |
| `subtitle` | text | unset | Entry subtitle |
| `notify-enter` | text | unset | Message when somebody enters |
| `notify-leave` | text | unset | Message when somebody leaves |
| `scoreboard` | `true` / `false` | `true` | Show the sidebar |

## Presentation

| Flag | Values | Description |
|------|--------|-------------|
| `alias` | text | The house's display name |
| `description` | text | Shown in Discovery and `/h goto` |
| `icon` | a material | The item the house is drawn as. Picked from a searchable grid; a material that cannot be drawn is refused at the write |
| `category` | one of `discovery.categories` | What the house is for. A category the server never declared is refused |

---

## The Scoreboard

The sidebar is a flag with its own sub-command:

```
/h flag scoreboard title <text>
/h flag scoreboard set <n> <text>
/h flag scoreboard clear <n>
```

A Minecraft sidebar draws **15** lines. The board is `scoreboard.fixed_top_lines` + the house's lines + `scoreboard.fixed_bottom_lines`, so every fixed line the server adds is one an owner loses: with the shipped 2 + 2, houses get 11. The editor shows exactly that and marks the rest as the server's.

Two switches in `settings.yml` decide how much is the owner's:

| Setting | Default | Effect |
|---------|---------|--------|
| `scoreboard.allow_house_title` | `false` | The header is the server's name for the board. Writing one is refused and says so |
| `scoreboard.allow_house_lines` | `true` | Turning it off makes every row the server's |

A house's lines are **seeded** from `settings.yml` rather than replacing them: editing one line keeps the rest, and an untouched house keeps following the server.

## Where Else Flags Apply

A flag is the house-wide answer. Two layers sit above it:

1. **Region + role**: the most specific answer
2. **Region**
3. **Role**
4. **House flag**

See [Members & Roles](members-roles.md). `/h region check` prints, for where you stand, what every overridable flag answers and which layer decided it.
