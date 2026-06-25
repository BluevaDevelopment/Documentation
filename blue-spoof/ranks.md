# Ranks Module

**File:** `plugins/BlueSpoof/modules/ranks.yml`

**Status:** Optional. **Disabled by default.** You must configure rank definitions before enabling it.

---

## What It Does

The Ranks module automatically assigns permission ranks to fake players when they join the server. This ensures fake players show the correct prefix, color, or group in tab lists, chat, and permission-based plugins.

---

## Why It Is Disabled by Default

Every server and network uses different rank names, commands, and permission plugins. The default configuration contains only example entries that will not work on your server until you edit them. Enable the module only after you have configured the rank definitions to match your setup.

---

## Rank Definitions

Each rank entry contains:

| Field | Description |
|-------|-------------|
| `weight` | How often this rank is assigned. Higher weight = more common. |
| `command` | The command to execute when this rank is assigned. Use `{fake_player}` as the placeholder for the bot's name. |

### Example

```yaml
ranks:
  definitions:
    default:
      weight: 70
      command: ""
    vip:
      weight: 20
      command: "lp user {fake_player} parent set vip"
    mvp:
      weight: 8
      command: "lp user {fake_player} parent set mvp"
    elite:
      weight: 2
      command: "lp user {fake_player} parent set elite"
```

In this example:
- 70% of fake players get no special rank (default)
- 20% get VIP
- 8% get MVP
- 2% get Elite

The weights do not need to add up to 100. The plugin normalizes them automatically.

---

## Apply on Join

| Option | Description | Default |
|--------|-------------|---------|
| `ranks.apply_on_join` | Run the rank command when a fake player joins | `true` |
| `ranks.apply_delay_ticks` | Delay before running the command | `20` (1 second) |

The delay is useful because some permission plugins need a moment after player login before they accept group changes.

---

## Default Rank

| Option | Description | Default |
|--------|-------------|---------|
| `ranks.default_rank` | Rank assigned when weighted selection fails | `default` |

If for some reason the plugin cannot pick a weighted rank (for example all ranks have weight 0), it falls back to this rank ID.

---

## Network Sync

| Option | Description | Default |
|--------|-------------|---------|
| `ranks.sync_between_network.enabled` | Share cached fake-player ranks across backend servers | `false` |

When enabled, BlueSpoof shares the cached rank assignments through the configured proxy sync method (TCP, Redis, SQL, or Plugin Messaging). This ensures that if a fake player reconnects to a different backend server in your network, they keep the same rank.

**Only enable this if every backend server has exactly the same rank identifiers and command behavior.** If Server A has `vip` but Server B calls it `donor`, the rank command will fail on Server B.

---

## Full Example Configuration

```yaml
ranks:
  enabled: false
  default_rank: default
  apply_on_join: true
  apply_delay_ticks: 20
  sync_between_network:
    enabled: false
  definitions:
    default:
      weight: 70
      command: ""
    vip:
      weight: 20
      command: "lp user {fake_player} parent set vip"
    mvp:
      weight: 8
      command: "lp user {fake_player} parent set mvp"
    elite:
      weight: 2
      command: "lp user {fake_player} parent set elite"
```

---

## Notes

- The `command` field supports any console command. You are not limited to LuckPerms; any plugin that accepts a player name in a console command will work.
- If a rank has an empty `command`, no command is executed. This is useful for the default rank.
- Rank assignments are cached per bot. If the same nick reconnects, it keeps the same rank unless you clear the cache.
- If you change rank definitions, existing fake players do not retroactively update. Only newly connected fake players use the new weights.
