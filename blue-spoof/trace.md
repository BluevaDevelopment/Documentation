# Trace Module

**File:** `plugins/BlueSpoof/modules/trace.yml`

**Status:** Optional. Enable by setting `trace.enabled: true`.

---

## What It Does

The Trace module records real player movement inside defined regions and replays those recordings with fake players. This is ideal for lobbies, spawn areas, or any location where you want fake players to follow believable, human-captured paths instead of random roaming.

---

## How It Works

1. **Define a region** — Mark a cuboid area where recording should happen.
2. **Real players enter** — When a real player walks into the region, the plugin records their movement frame by frame.
3. **Recording is saved** — The captured path is stored with a unique ID.
4. **Fake players replay** — When a fake player enters the same region (or when you manually trigger it), they replay a random recording from that region.

---

## Regions

Regions are cuboid areas defined by two corner positions. You can create them in two ways:

### Using the Magic Stick

1. Run `/bs trace stick` to receive the Magic Stick.
2. **Left-click** a block to set position 1.
3. **Right-click** a block to set position 2.
4. Run `/bs trace region add [name]` to create the region from your selection.

### Using Coordinates

If you already know the coordinates, you can skip the stick:

```
/bs trace region add lobby -100 50 -100 100 256 100
```

### Managing Regions

| Command | Description |
|---------|-------------|
| `/bs trace region add [name] (x1 y1 z1 x2 y2 z2)` | Add a new region |
| `/bs trace region remove [name]` | Remove a region |
| `/bs trace region list` | List all defined regions |

---

## Recording

### Auto-Record

When `trace.auto_record` is enabled, recording starts automatically the moment a real player enters any trace region. Recording stops when the player leaves the region or when the maximum frame count is reached.

### Manual Recording

If auto-record is disabled, you can start and stop recordings via admin commands (coming in future updates). For now, auto-record is the primary method.

### Recording Limits

| Option | Description | Default |
|--------|-------------|---------|
| `trace.max_frames` | Maximum frames (ticks) per recording. At 20 ticks per second, 72000 = 1 hour, 24000 = 20 minutes, 6000 = 5 minutes. | `72000` |

Set this high enough to capture full sessions but low enough to prevent excessive disk usage.

---

## Playback

### Bot Auto-Play

When `trace.bot_auto_play` is enabled, fake players that enter a trace region automatically start replaying a random recording captured in that same region.

### Manual Playback

You can manually make any fake player replay a recording:

```
/bs trace play [bot] [recording_id]
/bs trace play [bot] [recording_id] loop
```

The `loop` argument makes the bot repeat the recording indefinitely.

### Stopping Playback

```
/bs trace stop [bot]
```

---

## End Actions

When a non-looping playback reaches its final frame, you can choose what the bot does next:

| Action | Description | Best For |
|--------|-------------|----------|
| `disconnect` | The bot disconnects from the server | Lobbies (looks like the player joined a game mode) |
| `teleport` | The bot is silently teleported to a configured destination | Survival servers (looks like the player used a warp) |
| `none` | The bot stays in place | Any scenario where you want the bot to remain online |

### Configuration

```yaml
trace:
  bot_end_action: disconnect
  bot_end_teleport:
    world: world
    x: 0.0
    y: 64.0
    z: 0.0
    yaw: 0.0
    pitch: 0.0
```

The `bot_end_teleport` settings are ignored unless `bot_end_action` is set to `teleport`.

---

## Commands

| Command | Description |
|---------|-------------|
| `/bs trace stick` | Get the Magic Stick for region selection |
| `/bs trace region add [name] (x1 y1 z1 x2 y2 z2)` | Add a trace region |
| `/bs trace region remove [name]` | Remove a trace region |
| `/bs trace region list` | List all regions |
| `/bs trace recordings list` | List all stored recordings |
| `/bs trace recordings delete [id]` | Delete a recording |
| `/bs trace play [bot] [id] (loop)` | Make a bot replay a recording |
| `/bs trace stop [bot]` | Stop a bot's playback |

---

## Full Example Configuration

```yaml
trace:
  enabled: true
  max_frames: 72000
  auto_record: true
  bot_auto_play: true
  bot_end_action: disconnect
  bot_end_teleport:
    world: world
    x: 0.0
    y: 64.0
    z: 0.0
    yaw: 0.0
    pitch: 0.0
```

---

## Notes

- Recordings are stored in `plugins/BlueSpoof/trace_recordings/` by default.
- A recording cannot be deleted while it is actively being played back by any bot.
- The Trace module and Movement module can coexist. If a bot is playing a Trace recording, Movement roaming is paused. When playback ends (and the bot does not disconnect), roaming resumes.
- Frame-perfect accuracy is not guaranteed across different server versions or physics changes. Recordings work best on flat ground and simple terrain.
- Recordings capture position, rotation, and movement state (sneak, sprint). They do not capture block interactions, chat, or inventory actions.
