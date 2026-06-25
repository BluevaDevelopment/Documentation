# Blue Arcade Documentation

Welcome to the official documentation for **Blue Arcade** - the modular minigames and party-games platform for Minecraft servers.

## What is Blue Arcade?

Blue Arcade is a **generic minigame API and runtime**, not a fixed collection of games. Minigames are delivered as separate modules that can be downloaded, installed, and updated independently. The plugin handles arena lifecycle, matchmaking, parties, statistics, leaderboards, cosmetics, economy, and cross-server proxy support so each module only has to implement its own game rules.

## Key Features

- **Dynamic Arenas**: every match runs in a fresh temporary world copied from a saved template. This is the recommended and default arena type.
- **Arena On Demand**: busy dynamic arenas can automatically spin up extra instances so more players can join without manual setup.
- **Proxy Mode**: lobby servers can list arenas hosted on separate game servers and send players with `/ba join server:arena`.
- **Redis, MySQL and MariaDB Messaging**: alternatives to socket mode for proxy communication.
- **Shared Database Profiles**: stats sync and proxy SQL messaging can use the same database connection.
- **Modular Minigames**: Bed Wars, Build Battle, Guess The Build, Speed Builders, SkyWars, Battle Royale, and many more.
- **AdvancedSlimePaper Support**: dynamic runtime worlds can load through ASP when it is installed.
- **Spectator Mode**, fixed game order, disabled voting, per-module reward overrides, adaptive namespaced sounds, and more.

## Documentation Sections

### Getting Started
- [First Steps](first-steps.md) - Installation, requirements, and initial setup
- [Creating Arenas](arenas.md) - Dynamic arena creation, templates, and configuration

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

---

## Minigame Modules

Minigames are separate modules. Each module has its own store page with detailed setup instructions. You can install them in-game or manually.

### Minigames

| Module | Store Page | In-Game Install |
|--------|-----------|-----------------|
| SkyWars | [Store](https://blueva.net/store/blue-arcade/modules/skywars) | `/baa module store download skywars` |
| Battle Royale | [Store](https://blueva.net/store/blue-arcade/modules/battle-royale) | `/baa module store download battle_royale` |
| Bed Wars | [Store](https://blueva.net/store/blue-arcade/modules/bed_wars) | `/baa module store download bed_wars` |
| Build Battle | [Store](https://blueva.net/store/blue-arcade/modules/build_battle) | `/baa module store download build_battle` |
| TNT Run | [Store](https://blueva.net/store/blue-arcade/modules/tnt-run) | `/baa module store download tnt_run` |
| Block Party | [Store](https://blueva.net/store/blue-arcade/modules/block-party) | `/baa module store download block_party` |
| Guess The Build | [Store](https://blueva.net/store/blue-arcade/modules/guess_the_build) | `/baa module store download guess_the_build` |
| Speed Builders | [Store](https://blueva.net/store/blue-arcade/modules/speed_builders) | `/baa module store download speed_builders` |
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

---

## Requirements

- **Server**: Spigot 1.18 or any fork (Paper, Purpur, Folia, etc.)
- **Java**: 17 or higher
- **RAM**: 2–4 GB minimum depending on player count, arena size, and number of modules

## Optional Dependencies

- **PlaceholderAPI** - For placeholder support in external plugins
- **Vault** - Use server economy instead of Blue Arcade credits
- **NoteBlockAPI** - For victory music playback
- **Floodgate** - Bedrock player support
- **WorldEdit** - Schematic generation and large-region operations
- **AdvancedSlimePaper** - Faster in-memory dynamic arena worlds

## Support

- Discord: [Blueva Development](https://discord.gg/blueva)
- Store: [blueva.net](https://blueva.net)
