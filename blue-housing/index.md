# BlueHousing Documentation

Official documentation for **BlueHousing**, private creative worlds for every player, in the style of Hypixel Housing.

Every player gets one or more empty worlds of their own to build in. There are no shared worlds and no regions to configure: the plugin is about ownership, visiting, per-house rules and creativity.

## Highlights

- **Multi-slot private houses**: several houses per player, each with its own world, inventory and settings
- **Per-house flags**: building, abilities, environment, safety, social and presentation, set from a menu or a command
- **Roles and regions**: free-form house roles that can override flags, and drawn regions that override them again
- **Discovery**: a public directory of five scrolling channels the player chooses, with likes, ratings and gifts
- **Lua scripting**: a sandboxed runtime per house, edited from a web IDE with Blockly or Monaco
- **Decorations**: holograms, NPCs, particle emitters and a decorative head shop
- **Custom music**: NBS playback per listener, with a song shop
- **Economy**: build and playtime rewards, house slots, borders, donations and player-to-player sales
- **Building tools**: a WorldEdit menu for people who have never typed `//set`
- **Bedrock support**: every menu has a native form through Floodgate/Geyser
- **SQL or JSON storage**, and an optional multi-server network mode

## Documentation Sections

### Getting Started
- [First Steps](first-steps.md) - Requirements, installation, and the first house

### Houses
- [Houses](houses.md) - Creating, slots, templates, reset, delete and backups
- [Flags](flags.md) - Every flag and where it is set
- [Members & Roles](members-roles.md) - Whitelist, trust, roles, regions and precedence
- [Moderation](moderation.md) - Kick, ban, mute, history and admin mode
- [Building Tools](building-tools.md) - The WorldEdit menu

### Content
- [Discovery](discovery.md) - Channels, likes, ratings, gifts and categories
- [Decorations](decorations.md) - Holograms, NPCs, particles and the head shop
- [Music](music.md) - Song library, playback and the shop
- [Economy](economy.md) - Providers, rewards, purchases and sales

### Scripting
- [Scripting](scripting.md) - The Lua runtime and the web editor
- [Lua API](lua-api.md) - Complete function reference

### Reference
- [Commands & Permissions](commands-permissions.md)
- [Placeholders](placeholders.md)
- [Configuration](configuration.md) - `settings.yml` tour
- [Storage](storage.md) - JSON, SQLite and MySQL
- [Network Mode](network.md) - Lobby and house servers
- [Performance](performance.md) - Redstone and entity budgets

## Requirements

- **Server**: Spigot 1.21 to 26.2 (Paper, Purpur and other forks supported)
- **Java**: 21 or higher
- **Internet access on first startup**: the plugin downloads its runtime libraries once into `plugins/BlueHousing/libraries/`

## Optional Dependencies

| Plugin | What it adds |
|--------|--------------|
| **PlaceholderAPI** | Placeholders in scoreboards, holograms and conditions |
| **Vault** | Vault economy as a balance provider |
| **Floodgate** + **Geyser** | Native Bedrock forms instead of chest menus |
| **WorldEdit** / **FastAsyncWorldEdit** | The building tools menu and build rewards |
| **ProtocolLib** | Extra packet support on older server builds |

## Support

- Discord: [Blueva Development](https://discord.gg/blueva)
- Store: [blueva.net/store](https://blueva.net/store)
