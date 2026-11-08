# Virtual Arenas (Experimental)

Virtual arenas are a new **experimental** arena type added in 3.6.0. Instead of loading a world per match, the map is sent directly to players as packets: terrain is rendered on the client on top of one shared empty carrier world, and every match keeps only its own block changes in memory.

> **Dynamic arenas remain the recommended type.** Virtual arenas are open for testing, but their performance still needs improvement and the full match flow has not been validated end-to-end with real players yet. If you are setting up a production server, use [dynamic arenas](arenas.md).

## Requirements

- **Paper** with Mojang mappings (tested on 1.21.5). Spigot and non-Mojang-mapped servers are not supported; the plugin refuses to start virtual matches there.
- **Universal modules only.** Every minigame configured in a virtual arena must be a universal `.bamodule`. Legacy `.jar` modules are rejected when adding them to the arena. See [Universal Modules](universal-modules.md).

## How They Work

- The server keeps **one shared empty world** (`ba-virtual`) that acts as the physical anchor for players. No per-match world is ever created, loaded, or deleted.
- Each match's map is streamed to players as chunk packets, and block changes made during the match are tracked per session instead of being written to a world on disk.
- For players, a virtual arena looks and plays like any other arena: join, countdown, match, podium, back to the lobby.

## Creating a Virtual Arena

Add the `virtual` type when creating the arena:

```
/baa create [id] <standalone|party> virtual
```

or, with the new contextual syntax:

```
/baa arena create [id] <standalone|party> virtual
```

Setup works exactly like a dynamic arena: the plugin creates temporary setup worlds, you build the lobby and the maps, and saving a template also converts it to the Slime format (`world.slime`) used to stream the map. A virtual arena cannot be enabled until every configured minigame has a saved map.

Differences from dynamic arenas:

- `setlobby skip` is not available; virtual arenas always use a waiting lobby.
- **Arena On Demand is not available** for virtual arenas.
- Matches in virtual arenas are **not recorded** by the [replay system](replay-system.md) yet.

## Converting Between Dynamic and Virtual

Existing dynamic arenas can be converted to virtual, and virtual arenas back to dynamic, with the migrate command. It runs **from the console only**, for safety:

```
/baa arena migrate [id] virtual
/baa arena migrate [id] virtual confirm
```

The confirmation must be run within 30 seconds of the first command. The default target is `dynamic`; static is never a target (a static arena must be migrated to dynamic first, and from there to virtual if wanted).

Before migrating, the arena must be **disabled, empty and stopped**, with no on-demand instances running.

What the migration does:

- The current arena folder is moved to `plugins/BlueArcade3/backup/arena_[id]_mode_migration_...`, and the backup is never modified afterwards.
- Saved maps are converted asynchronously (to Slime when migrating to virtual, back to world templates when migrating to dynamic). Do not edit or enable the arena until the conversion finishes.
- Migrating to virtual fails with a clear list if any configured minigame does not have its universal version loaded.
- The arena is left **disabled** after migrating so you can test it before enabling it again.

## Configuration

There is one setting, in `settings.yml`:

```yaml
virtual:
 fast_paths: true
```

Virtual arenas skip work for players standing still, cache block collision shapes and send chunk edits as block updates. Set `fast_paths` to `false` to turn those shortcuts off; it costs server time but gives the plain reference behaviour, which can be useful when testing or reporting issues.

## Current Limitations

- Legacy `.jar` modules are rejected; only universal modules run in virtual arenas.
- Seats do not move with vehicles; boats, minecarts and steering are not supported.
- Bows and crossbows use a plain power curve without enchantment modifiers.
- Fireballs do not break blocks and do not set incendiary fire.
- The waiting lobby is inert: only Core lobby items work there.
- No replays, no Arena On Demand.
- The complete waiting, starting, round and end flow still needs live validation with real clients.

If you test virtual arenas, report anything that behaves differently from a dynamic arena.

## Recommendation

Use **dynamic arenas** for production. They are the default, they support every feature (replays, on-demand instances, legacy and universal modules), and they are the type that receives the most testing. Treat virtual arenas as a preview of where the platform is heading: no per-match world loading at all.
