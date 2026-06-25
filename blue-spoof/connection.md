# Connection Module

**File:** `plugins/BlueSpoof/modules/connection.yml`

**Status:** Always active. This module cannot be disabled because BlueSpoof depends on it for all fake-player connection behavior.

---

## What It Does

The Connection module controls how fake players enter the server and what network data they present to the outside world. It handles batch connections, ping values, and IP addresses.

---

## Batch Connect

When you run `/bs connect 10`, the plugin does not connect all 10 fake players at the exact same tick. That would look extremely unnatural. Instead, it connects them one by one with a configurable delay between each join.

### Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `connection.batch_connect.delay_ticks` | Delay between each fake player connection (20 ticks = 1 second) | `40` |
| `connection.batch_connect.nick_reuse.enabled` | Prefer previously used nicks so the same bots reconnect over time | `true` |
| `connection.batch_connect.nick_reuse.new_player_chance` | Chance (out of 100) to pick a brand-new nick even when reusable nicks exist | `15` |
| `connection.batch_connect.progress_bossbar.enabled` | Show a progress boss bar to the command executor | `true` |

### Why Nick Reuse Matters

If you connect fake players every day, you want some of them to look like returning users, not a completely new set every single time. With nick reuse enabled, the plugin stores which nicks have already been used and prefers them. The `new_player_chance` setting still allows a small percentage of genuinely new names to keep the pool varied.

### Progress Boss Bar

When enabled, a boss bar appears while batch connect is running showing how many fake players have connected out of the total requested, plus elapsed time. This is useful for visual confirmation when connecting large numbers.

---

## Ping Spoofing

Each fake player is assigned a random ping value so they do not all show the exact same latency in the tab list.

### Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `connection.ping.milliseconds.min` | Minimum ping value | `0` |
| `connection.ping.milliseconds.max` | Maximum ping value | `350` |
| `connection.ping.update_interval` | How often the ping is randomized, in ticks | `600` (30 seconds) |

### Practical Range

If you want fake players to look like they are from different parts of the world, set a wide range (for example 20 to 300). If you want them to look local, keep the range tight (for example 10 to 60).

---

## IP Spoofing

Each fake player can be assigned a random IP address. This is useful for plugins that display player IPs or for making fake players look geographically distributed.

### Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `connection.ip.enabled` | Enable IP spoofing | `true` |
| `connection.ip.range.start` | Start of the IP range | `23.0.0.0` |
| `connection.ip.range.end` | End of the IP range | `95.255.255.255` |

### Persistence

The assigned IP is stored in `cache/bot.json`. This means the same fake player (same nick) will always keep the same IP across reconnects, making the spoofing more believable.

### Choosing a Range

The default range covers a broad selection of real-world public IPs. You can narrow it down to simulate players from a specific region. For example:

```yaml
connection:
  ip:
    enabled: true
    range:
      start: 185.0.0.0   # European IPs
      end: 185.255.255.255
```

---

## Full Example Configuration

```yaml
connection:
  batch_connect:
    delay_ticks: 40
    nick_reuse:
      enabled: true
      new_player_chance: 15
    progress_bossbar:
      enabled: true

  ping:
    milliseconds:
      max: 350
      min: 0
    update_interval: 600

  ip:
    enabled: true
    range:
      start: 23.0.0.0
      end: 95.255.255.255
```

---

## Notes

- If IP spoofing is enabled but the assigned IP conflicts with a real player's IP, the fake player will still connect normally. The plugin does not enforce IP uniqueness.
- The ping update interval is global; all fake players have their ping randomized at the same time. If you want more variation, use a shorter interval.
- The progress boss bar only appears during batch connects triggered by `/bs connect <amount>`. Single connects do not show it.
