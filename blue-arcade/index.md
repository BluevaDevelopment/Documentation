# Blue Arcade 3.0 Documentation

Welcome to the official documentation for **Blue Arcade 3.0** - the ultimate party games and minigames plugin for Minecraft servers.

## What's New in 3.0

Blue Arcade 3.0 is a complete rewrite of the plugin with a modular architecture:

- **Modular System**: Minigames are now separate modules that can be downloaded and installed independently
- **Party System**: Create and manage parties to play with friends
- **Store System**: In-game cosmetics shop with victory effects, death effects, and more
- **Achievements System**: Track player progress with multi-phase achievements
- **Leaderboards**: Global and per-minigame rankings with time periods
- **XP & Levels**: Player progression system with configurable titles
- **PlaceholderAPI**: Extensive placeholder support for scoreboards and other plugins
- **Bedrock Support**: Full compatibility with Geyser/Floodgate for Bedrock players

## Documentation Sections

### Getting Started
- [First Steps](first-steps.md) - Installation and initial setup
- [Creating Arenas](arenas.md) - How to create and configure arenas

### Systems
- [Party System](party-system.md) - Party creation and management
- [Store](store.md) - Cosmetics shop and credits economy
- [Achievements](achievements.md) - Achievement system and rewards
- [Leaderboards & Stats](leaderboards.md) - Statistics and rankings
- [XP & Levels](levels.md) - Experience and leveling system

### Reference
- [Commands & Permissions](commands-permissions.md) - Complete command reference
- [Placeholders](placeholders.md) - PlaceholderAPI integration

### For Developers
- [Creating a Module](developers.md) - Guide to creating custom minigame modules

## Minigame Modules

Minigames are now separate modules. Each module has its own store page with detailed setup instructions:

### Minigames
| Module | Store Page | In-Game Install |
|--------|-----------|-----------------|
| SkyWars | [Store](https://blueva.net/store/blue-arcade/modules/skywars) | `/baa module store download skywars` |
| Battle Royale | [Store](https://blueva.net/store/blue-arcade/modules/battle-royale) | `/baa module store download battle_royale` |
| TNT Run | [Store](https://blueva.net/store/blue-arcade/modules/tnt-run) | `/baa module store download tnt_run` |
| Block Party | [Store](https://blueva.net/store/blue-arcade/modules/block-party) | `/baa module store download block_party` |
| Run From The Beast | [Store](https://blueva.net/store/blue-arcade/modules/run-from-the-beast) | `/baa module store download run_from_the_beast` |
| TNT Tag | [Store](https://blueva.net/store/blue-arcade/modules/tnt-tag) | `/baa module store download tnt_tag` |
| Lucky Pillars | [Store](https://blueva.net/store/blue-arcade/modules/lucky-pillars) | `/baa module store download lucky_pillars` |

### Microgames
| Module | Store Page | In-Game Install |
|--------|-----------|-----------------|
| Traffic Light | [Store](https://blueva.net/store/blue-arcade/modules/traffic-light) | `/baa module store download traffic_light` |
| Spleef | [Store](https://blueva.net/store/blue-arcade/modules/spleef) | `/baa module store download spleef` |
| Snowball Fight | [Store](https://blueva.net/store/blue-arcade/modules/snowball-fight) | `/baa module store download snowball_fight` |
| One In The Chamber | [Store](https://blueva.net/store/blue-arcade/modules/one-in-the-chamber) | `/baa module store download one_in_the_chamber` |
| Minefield | [Store](https://blueva.net/store/blue-arcade/modules/minefield) | `/baa module store download minefield` |
| Knockback | [Store](https://blueva.net/store/blue-arcade/modules/knockback) | `/baa module store download knockback` |
| Fast Zone | [Store](https://blueva.net/store/blue-arcade/modules/fast-zone) | `/baa module store download fast_zone` |
| Red Alert | [Store](https://blueva.net/store/blue-arcade/modules/red-alert) | `/baa module store download red_alert` |
| Exploding Sheep | [Store](https://blueva.net/store/blue-arcade/modules/exploding-sheep) | `/baa module store download exploding_sheep` |
| All Against All | [Store](https://blueva.net/store/blue-arcade/modules/all-against-all) | `/baa module store download all_against_all` |
| Race | [Store](https://blueva.net/store/blue-arcade/modules/race) | `/baa module store download race` |
| Chairs | [Store](https://blueva.net/store/blue-arcade/modules/chairs) | `/baa module store download chairs` |

## Requirements

- **Server**: Spigot 1.21.X or any fork (Paper, Purpur, etc.)
- **Java**: 21 or higher
- **RAM**: 2-4 GB minimum (depending on server size)

## Optional Dependencies

- **PlaceholderAPI** - For placeholder support in external plugins
- **Vault** - For economy integration (use server currency instead of credits)
- **NoteBlockAPI** - For victory music playback

## Support

- Discord: [Blueva Development](https://discord.gg/blueva)
- Issues: [GitHub](https://github.com/BluevaDevelopment/BlueArcade/issues)
