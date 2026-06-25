# Events Module

**File:** `plugins/BlueSpoof/modules/events.yml`

**Status:** Optional. Enable by setting `events.enabled: true`.

---

## What It Does

The Events module runs commands automatically when specific things happen to fake players:

- **Join Event** — When a fake player connects to the server
- **Quit Event** — When a fake player disconnects
- **Task Event** — Repeatedly while fake players are online

This is useful for integrating fake players with other plugins. For example, you can broadcast a welcome message, assign a permission group, or trigger a custom event in another plugin.

---

## Important: Disabled by Default

All example commands in the default configuration are set to `enabled: false`. This prevents unintended side effects on first install. You must explicitly enable the entries you want.

---

## Command Entries

Each event contains a numbered list of command entries. Every entry supports:

| Field | Description |
|-------|-------------|
| `enabled` | Whether this entry is active (`true` / `false`) |
| `chance` | Probability (0-100) that this entry executes |
| `delay` | Ticks to wait before executing (join/quit events only) |
| `ticks` | Interval in ticks (task events only) |
| `execute` | Who runs the command: `CONSOLE` or `FAKE_PLAYER` |
| `list` | List of commands to execute |

### Execute Modes

- **CONSOLE** — The command runs from the server console. Use this for commands like `broadcast`, `lp user ... parent set ...`, or `give`.
- **FAKE_PLAYER** — The command runs as if the fake player typed it. Use this for player commands like `msg`, `home`, or plugin-specific player commands.

---

## Join Event

Runs when a fake player connects.

### Available Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{fake_player}` | The fake player's name |
| `{random_player}` | A random online real player |
| `{random_fake_player}` | A random online fake player |

### Example

```yaml
events:
  join_event:
    commands:
      1:
        enabled: true
        chance: 100
        delay: 20
        execute: CONSOLE
        list:
          - 'broadcast &eWelcome &b{fake_player}'
      2:
        enabled: false
        chance: 100
        delay: 10
        execute: CONSOLE
        list:
          - 'lp user {fake_player} parent set vip'
      3:
        enabled: false
        chance: 100
        delay: 500
        execute: FAKE_PLAYER
        list:
          - 'msg {random_player} Hello friend!'
```

---

## Quit Event

Runs when a fake player disconnects.

### Available Placeholders

Same as join event, plus `{player}` which refers to the context player if applicable.

### Example

```yaml
events:
  quit_event:
    commands:
      1:
        enabled: false
        chance: 30
        delay: 90
        execute: CONSOLE
        list:
          - 'bluespoof chat {random_fake_player} Bye {player}!'
      2:
        enabled: false
        chance: 10
        delay: 90
        execute: CONSOLE
        list:
          - 'bluespoof chat {random_fake_player} We will miss you {player}!'
```

---

## Task Event

Runs periodically for each fake player while they are online. This is a looping timer per bot.

### Available Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{fake_player}` | The fake player's name |
| `{random_fake_player}` | A random online fake player |
| `{random_player}` | A random online real player |

### Example

```yaml
events:
  task_event:
    commands:
      1:
        enabled: false
        ticks: 80
        chance: 30
        execute: CONSOLE
        list:
          - 'lp user {random_fake_player} parent set vip'
      2:
        enabled: false
        ticks: 20
        chance: 15
        execute: CONSOLE
        list:
          - 'lp user {random_fake_player} parent set mvp'
```

---

## Full Example Configuration

```yaml
events:
  enabled: true
  join_event:
    commands:
      1:
        enabled: false
        chance: 100
        delay: 20
        execute: CONSOLE
        list:
          - 'broadcast &a----------'
          - 'broadcast &f'
          - 'broadcast &eWelcome &b{fake_player}'
          - 'broadcast &f'
          - 'broadcast &a----------'
  quit_event:
    commands:
      1:
        enabled: false
        chance: 30
        delay: 90
        execute: CONSOLE
        list:
          - 'bluespoof chat {random_fake_player} Bye {player}!'
  task_event:
    commands:
      1:
        enabled: false
        ticks: 80
        chance: 30
        execute: CONSOLE
        list:
          - 'lp user {random_fake_player} parent set vip'
```

---

## Notes

- Task events run per fake player. If you have 10 fake players online and a task runs every 80 ticks, it executes 10 times every 80 ticks (once per bot).
- Delay and chance values make events feel organic. A 100% chance with 0 delay is instant and robotic; a 30% chance with a 90-tick delay feels more human.
- Commands executed as `FAKE_PLAYER` only work if the fake player has permission to run them. Most permission plugins allow you to grant permissions to offline/fake players via groups.
- If you use the `bluespoof chat` command inside an event, make sure the Chat module is enabled or the command will have no effect beyond the raw text display.
