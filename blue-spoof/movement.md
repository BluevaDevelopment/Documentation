# Movement Module

**File:** `plugins/BlueSpoof/modules/movement.yml`

**Status:** Optional. Enable by setting `movement.enabled: true`.

---

## What It Does

The Movement module makes fake players move around the world instead of standing still. Bots pick random destinations, walk (or sprint) to them, pause to look around, and occasionally jump. This creates the illusion of real players exploring or idling in your world.

---

## How It Works

The movement engine runs in a continuous loop for each fake player:

1. **Roam** — Pick a random waypoint within the configured radius and walk to it.
2. **Observe** — Stop and look around for a random duration.
3. **Repeat** — Go back to roaming.

Humanization features are layered on top to avoid robotic behavior:

- **Sprint chance** — Bots sometimes sprint instead of walking.
- **Sprint-jump chance** — Occasional sprint-jumps while moving.
- **Micro-pauses** — Tiny random stops during movement.
- **Waypoint stop chance** — Chance to stop at a waypoint instead of immediately moving on.

---

## Auto-Roam on Join

When enabled, fake players automatically start roaming shortly after connecting. This is useful for survival and lobby servers where you want bots to spread out naturally.

### Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `movement.auto_roam.on_fake_join.enabled` | Start roaming automatically when a fake player joins | `false` |
| `movement.auto_roam.on_fake_join.delay_ticks.min` | Minimum delay before roaming starts | `20` (1 second) |
| `movement.auto_roam.on_fake_join.delay_ticks.max` | Maximum delay before roaming starts | `80` (4 seconds) |
| `movement.auto_roam.on_fake_join.skip_if_trace_playing` | Do not start roam if the bot is currently replaying a Trace recording | `true` |

The random delay prevents all bots from moving at the exact same moment, which would look synchronized and unnatural.

---

## Isolation

When enabled, newly joined fake players are teleported to a hidden bedrock isolation world instead of spawning in the main world. This prevents them from interfering with real players or appearing in unwanted locations until you manually move them.

### Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `movement.isolation.on_fake_join.enabled` | Isolate fake players on join | `true` |
| `movement.isolation.on_fake_join.delay_ticks` | Small re-apply delay for compatibility with login plugins | `5` |

The isolation world is generated automatically by the plugin. You can move bots out of it manually with `/bs movement isolate [bot]` or by teleporting them via commands.

---

## Roaming Settings

| Option | Description | Default |
|--------|-------------|---------|
| `movement.roam.radius.min` | Minimum roam radius in blocks | `4.0` |
| `movement.roam.radius.max` | Maximum roam radius in blocks | `20.0` |
| `movement.roam.idle_ticks.min` | Minimum ticks to wait between waypoints | `8` |
| `movement.roam.idle_ticks.max` | Maximum ticks to wait between waypoints | `30` |
| `movement.roam.waypoints_before_observe` | How many waypoints before a look-around pause | `6` |
| `movement.roam.waypoint_stop_chance` | Chance to stop at each waypoint | `0.45` |
| `movement.roam.sprint_chance` | Chance to sprint instead of walk | `0.40` |

The radius is relative to the bot's current location. A radius of 4 to 20 blocks means the bot will wander in a small area, ideal for lobbies or spawn regions. For larger worlds you can increase the radius.

---

## Observe Settings

| Option | Description | Default |
|--------|-------------|---------|
| `movement.observe.ticks.min` | Minimum look-around duration | `15` |
| `movement.observe.ticks.max` | Maximum look-around duration | `50` |

During observe state the bot stands still and rotates its head, mimicking a player looking around.

---

## Humanization

| Option | Description | Default |
|--------|-------------|---------|
| `movement.humanization.sprint_jump_chance` | Chance per tick to perform a sprint-jump | `0.007` |
| `movement.humanization.micro_pause.chance` | Chance per tick to perform a micro-pause | `0.003` |
| `movement.humanization.micro_pause.ticks.min` | Minimum micro-pause duration | `2` |
| `movement.humanization.micro_pause.ticks.max` | Maximum micro-pause duration | `8` |

These values are intentionally low (fractions of a percent per tick) so they happen rarely but add just enough irregularity to make movement feel human.

---

## Stuck Detection

| Option | Description | Default |
|--------|-------------|---------|
| `movement.stuck.check_interval_ticks` | How often to check if the bot is stuck | `10` |
| `movement.stuck.min_progress` | Minimum distance the bot must move to not be considered stuck | `0.05` |
| `movement.stuck.max_retries` | How many recovery attempts before giving up | `2` |
| `movement.stuck.recover_wait_ticks` | Ticks to wait between recovery attempts | `10` |

If a bot walks into a wall, water, or an enclosed space, the stuck detector notices the lack of progress and attempts to recover by picking a new waypoint.

---

## Commands

| Command | Description |
|---------|-------------|
| `/bs movement walk [bot] here\|<x> <y> <z>` | Walk a bot to a location |
| `/bs movement sprint [bot] here\|<x> <y> <z>` | Sprint a bot to a location |
| `/bs movement sneak [bot] here\|<x> <y> <z>` | Sneak a bot to a location |
| `/bs movement roam [bot]` | Start autonomous roaming |
| `/bs movement stop [bot]` | Stop all movement for a bot |
| `/bs movement status (bot)` | Show controller state(s) |
| `/bs movement roamall` | Start roaming for all online fake players |
| `/bs movement stopall` | Stop movement for all active controllers |
| `/bs movement isolate [bot]` | Move a fake player to the isolation world |
| `/bs movement autoroam [on\|off\|reset]` | Toggle auto-roam globally |
| `/bs movement autoroam [bot] [on\|off\|reset]` | Toggle auto-roam for a specific bot |

---

## Full Example Configuration

```yaml
movement:
  enabled: true

  auto_roam:
    on_fake_join:
      enabled: false
      delay_ticks:
        min: 20
        max: 80
      skip_if_trace_playing: true

  isolation:
    on_fake_join:
      enabled: true
      delay_ticks: 5

  roam:
    radius:
      min: 4.0
      max: 20.0
    idle_ticks:
      min: 8
      max: 30
    waypoints_before_observe: 6
    waypoint_stop_chance: 0.45
    sprint_chance: 0.40

  observe:
    ticks:
      min: 15
      max: 50

  humanization:
    sprint_jump_chance: 0.007
    micro_pause:
      chance: 0.003
      ticks:
        min: 2
        max: 8

  stuck:
    check_interval_ticks: 10
    min_progress: 0.05
    max_retries: 2
    recover_wait_ticks: 10
```

---

## Notes

- Movement requires the bot to be in a loaded chunk. If a bot roams too far from players, chunk unloading may freeze it until a player gets close again.
- Trace playback and manual movement commands (`walk`, `sprint`, `sneak`) override roaming. When they finish, the bot resumes roam if it was active.
- The isolation world is created automatically on first use. It is a small void world with a single bedrock platform.
- Auto-roam is disabled by default to prevent bots from immediately leaving spawn areas before you configure them.
