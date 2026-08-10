# Replay System

Blue Arcade records matches automatically and lets players rewatch them afterwards. A replay is not a video: it is a recording of what happened, replayed inside a temporary copy of the arena, so you can fly around freely, follow any player, pause, rewind and change speed.

The replay system is available since 3.4.5 and is enabled by default.

Recording happens entirely inside Blue Arcade, so **every minigame is recorded without any adaptation on the module side**. Module authors do not have to add support, expose anything, or update their module: any module, official or third party, legacy or universal, is recorded the moment it runs in a dynamic arena.

## Requirements

Replays only work in **dynamic arenas**. A static arena runs inside a persistent world that may be modified before or after the match, so there is nothing reliable to rebuild the recording on. See [Why Dynamic Arenas](dynamic-arenas.md).

## Watching a Replay

| Command | Description |
|---------|-------------|
| `/ba replay` | Open the replay browser, or offer the replay of the arena you are playing in |
| `/ba replay [category]` | List the replays of a category |
| `/ba replay [category] [replay_id]` | Open a specific replay |
| `/ba replay leave` | Leave the replay and return to where you were |

After a match ends, players receive a message with a direct link to the replay of that match. If the recording is still being saved, the viewer opens automatically as soon as it is ready.

The browser groups replays into categories. Party matches appear under a single **Party** category instead of once per minigame played, and every other replay appears under the minigame it belongs to.

## Viewer Controls

While watching, the hotbar holds the playback controls:

| Slot | Control | Description |
|------|---------|-------------|
| 1 | Teleport | Open the participant list and jump to one of them |
| 3 | Slower | Reduce playback speed, down to 0.25x |
| 4 | Back | Rewind |
| 5 | Play / Pause | Freeze or resume the scene |
| 6 | Forward | Skip ahead |
| 7 | Faster | Increase playback speed, up to 4x |
| 9 | Leave | Close the replay |

Rewinding and skipping work across the whole match. In a party match, moving past the end of a round loads the next round's map automatically and drops you where that round was played.

The control items can be customized in `items.yml`, under `replay_controls`.

## What Gets Recorded

- Player position, orientation, equipment, health, food and game mode
- Entities and dropped items
- Block changes, including changes made directly by a module
- Scoreboards, world time and world border
- Chat and broadcast messages sent to the whole arena
- Victory effects

Messages addressed to a single player, such as personal titles, are not recorded. A live spectator would not have seen them either.

Eliminated players and spectators stop being recorded while they are spectating, so a replay shows the match as it was played.

## Configuration

All settings live under `replay` in `settings.yml`:

| Setting | Default | Description |
|---------|---------|-------------|
| `enabled` | `true` | Turn the whole system on or off |
| `retention_days` | `7` | Days a replay is kept before it is deleted automatically. 0 or less disables cleanup |
| `max_duration_minutes` | `60` | Maximum length of a recording |
| `max_events_per_replay` | `500000` | Maximum number of recorded events |
| `record_chat` | `true` | Record arena chat |
| `record_plugin_messages` | `true` | Record broadcast messages |
| `record_titles` | `true` | Record titles |
| `record_world_border` | `true` | Record world border changes |
| `record_victory_effects` | `true` | Record victory effects |
| `record_item_drops` | `true` | Record dropped items |
| `record_entities` | `true` | Record entities |
| `record_module_block_changes` | `true` | Record block changes a module makes through the blocks API |
| `block_scan.enabled` | `true` | Safety net for block changes that fire no event and do not use the blocks API |
| `block_scan.interval_ticks` | `1` | How often a scan pass runs |
| `block_scan.chunks_per_pass` | `0` | Chunks checked per pass. 0 or less means every loaded chunk |
| `viewer.default_speed` | `1.0` | Playback speed a replay opens with |
| `viewer.seek_seconds` | `10` | Seconds the forward and back controls jump |

A recording that reaches `max_duration_minutes` or `max_events_per_replay` is stopped and discarded. Party matches accumulate every round into a single recording, so raise these limits if your matches are long.

Recordings are stored in `plugins/BlueArcade/data/replays`, one folder per replay, compressed.

## Admin Commands

| Command | Description |
|---------|-------------|
| `/baa replay list` | List every recorded replay |
| `/baa replay open [replay_id]` | Open any replay, ignoring participation |
| `/baa replay delete [replay_id]` | Delete a replay |
| `/baa replay cleanup` | Delete every replay older than `retention_days` |

## Permissions

| Permission | Description |
|------------|-------------|
| `bluearcade.replay.view.own` | Watch replays of matches the player took part in |
| `bluearcade.replay.view.all` | Watch any replay on the server |
| `bluearcade.replay.admin` | Use the admin commands and open any replay |

## Notes

Playback runs in a temporary world created from the same template the match used, and that world is deleted when the viewer leaves. Nothing is written to your arena worlds, and a replay never affects a running match.
