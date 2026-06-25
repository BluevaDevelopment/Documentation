# Fluctuation Module

**File:** `plugins/BlueSpoof/modules/fluctuation.yml`

**Status:** Optional. Enable by setting `fluctuation.enabled: true`.

---

## What It Does

The Fluctuation module automatically connects and disconnects fake players over time based on configurable schedules. This makes your server's online count vary naturally throughout the day, mimicking real player traffic patterns.

Instead of connecting a fixed number of fake players and leaving them online forever, Fluctuation adds and removes them dynamically.

---

## How It Works

1. The plugin checks the current time against your defined schedules.
2. For the matching schedule, it calculates how many fake players should be online using the threshold and multiplier.
3. Fake players are connected or disconnected gradually with randomized delays so changes look natural.

---

## Time Zone

Set the time zone to match your server's peak hours:

```yaml
fluctuation:
  time_zone: "Europe/Madrid"
```

Use any valid IANA time zone identifier (for example `America/New_York`, `Asia/Tokyo`, `UTC`).

---

## Real Player Limits

Fluctuation respects these boundaries:

| Option | Description | Default |
|--------|-------------|---------|
| `fluctuation.real_players.min` | Minimum real players required for the module to activate. `0` means always active. | `0` |
| `fluctuation.real_players.max` | When real players reach this number, no more fake players are added. | `80` |

The max limit prevents fake players from crowding a server that is already popular. The min limit prevents fake players from appearing on an empty server when no real traffic exists.

---

## Maximum Fake Players

| Option | Description | Default |
|--------|-------------|---------|
| `fluctuation.maximum` | Hard cap on how many fake players Fluctuation can add | `40` |

Even if your schedule math suggests 50 fake players, this cap limits it to 40.

---

## Schedules

Schedules define how the plugin behaves during different times of day. You can create as many as you need.

Each schedule contains:

| Field | Description |
|-------|-------------|
| `time.start` | Start time in `HH:MM` format |
| `time.end` | End time in `HH:MM` format |
| `threshold.min` | Minimum target fake players |
| `threshold.max` | Maximum target fake players |
| `multiplier` | Multiply the number of real players to determine the target |
| `delay.join.min` / `delay.join.max` | Random delay between fake player joins (in ticks) |
| `delay.leave.min` / `delay.leave.max` | Random delay before a fake player leaves (in ticks) |

### Example Schedule

```yaml
fluctuation:
  schedule:
    1:
      time:
        start: "17:00"
        end: "20:30"
      threshold:
        min: 2
        max: 10
      multiplier: 2.0
      delay:
        join:
          max: 600
          min: 200
        leave:
          max: 12000
          min: 6000
```

In this example, between 5 PM and 8:30 PM:
- The plugin tries to maintain fake players equal to `real_players × 2.0`
- But never fewer than 2 and never more than 10
- Fake players join with a random delay of 200 to 600 ticks (10 to 30 seconds)
- Fake players leave with a random delay of 6000 to 12000 ticks (5 to 10 minutes)

---

## Creating Multiple Schedules

You can add more schedules by incrementing the number:

```yaml
fluctuation:
  schedule:
    1:
      time:
        start: "17:00"
        end: "20:30"
      threshold:
        min: 2
        max: 10
      multiplier: 2.0
      delay:
        join:
          max: 600
          min: 200
        leave:
          max: 12000
          min: 6000
    2:
      time:
        start: "20:30"
        end: "23:45"
      threshold:
        min: 2
        max: 10
      multiplier: 2.0
      delay:
        join:
          max: 600
          min: 200
        leave:
          max: 12000
          min: 6000
```

If no schedule matches the current time, Fluctuation does nothing. Fake players that were already connected remain online until a matching schedule tells them to leave.

---

## Full Example Configuration

```yaml
fluctuation:
  enabled: true
  maximum: 40
  time_zone: "Europe/Madrid"
  real_players:
    max: 80
    min: 0
  schedule:
    1:
      time:
        start: "17:00"
        end: "20:30"
      threshold:
        min: 2
        max: 10
      multiplier: 2.0
      delay:
        join:
          max: 600
          min: 200
        leave:
          max: 12000
          min: 6000
    2:
      time:
        start: "20:30"
        end: "23:45"
      threshold:
        min: 2
        max: 10
      multiplier: 2.0
      delay:
        join:
          max: 600
          min: 200
        leave:
          max: 12000
          min: 6000
```

---

## Notes

- Fluctuation only manages fake players it connected itself. Fake players connected manually with `/bs connect` are ignored by Fluctuation and will stay online unless you disconnect them manually.
- The leave delay is how long a fake player stays online before being marked for disconnection. A longer leave delay makes the population more stable; a shorter delay makes it more dynamic.
- If your server uses a proxy, make sure Fluctuation's maximum plus your real player peak does not exceed your proxy's `max_online` cap.
- Schedules do not overlap cleanly. If two schedules cover the same time range, the behavior is undefined. Keep schedule time ranges distinct.
