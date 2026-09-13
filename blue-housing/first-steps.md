# Getting Started

## Purchase the Plugin

- [Blueva Store](https://blueva.net/store/blue-housing)

## Server Requirements

- **Spigot** or any fork (Paper, Purpur, etc.)
- **Minecraft 1.21 to 26.2**: one JAR works on every supported version
- **Java 21** or higher
- **2 GB RAM** minimum, plus room for the house worlds that are loaded at once
- **Internet access on first startup**: the plugin downloads its runtime libraries once

## Installation

1. Stop the server
2. Drop the JAR into `plugins/`
3. Start the server. Every configuration file is generated, and the `void` and `default` templates are built
4. Give players `bluehousing.house.create` and `bluehousing.house.goto`

Nothing else is required. The plugin ships with an internal economy, JSON storage and a working set of menus.

## The First House

```
/h create
```

This opens the template picker. Choosing one creates the house, copies the world in and teleports the player to it.

From then on:

| Command | What it does |
|---------|--------------|
| `/h` | Opens the house menu when standing in your own house, Discovery otherwise |
| `/h goto` | Opens the navigation menu |
| `/h goto <player> [slot]` | Goes to somebody's house |
| `/h leave` | Back to the lobby, or to where you entered from |

## Set a Lobby

Stand where players should land when they leave a house and run:

```
/ha setlobby
```

Optional. Without it, `/h leave` returns the player to where they were standing when they entered, falling back to the main world spawn.

## Give Players More Houses

A player's house count is the highest `bluehousing.house.slots.<n>` they hold, plus any slots they have bought.

```
bluehousing.house.slots.3      # three houses
```

Slots can also be sold; see [Economy](economy.md).

## Where Things Live

```
plugins/BlueHousing/
├── settings.yml            # every server-side setting
├── language.yml            # every player-facing message
├── menus/java/*.yml        # chest menus
├── menus/bedrock/*.yml     # Bedrock forms
└── data/
    ├── houses/<owner>/<id>/    # house.json, world/, scripts/, backups/
    ├── templates/<id>/         # template.json + world/
    ├── players/<uuid>.json
    └── songs/                  # shared NBS library
```

A house's folder is its identity. Moving `data/houses/<owner>/<id>/` to another owner's folder transfers it.

## Next Steps

- [Flags](flags.md) - decide what players may do in their own houses
- [Configuration](configuration.md) - the parts of `settings.yml` worth changing first
- [Discovery](discovery.md) - make houses visible to other players
