# Java Menus

This page explains how to build Java inventory menus in YAML and how to manage them from the web editor.

## Menu anatomy (YAML)
A Java menu is a YAML file in `plugins/BlueMenu/menus/java/` with a `menuName`, `menuSize` (multiple of 9 up to 54), a `type` of `CHEST`, and an optional `openCommand` players can run. Items sit under the `items:` section and can be gated with display conditions before being added to the inventory.

### Items and item stacks
Each item entry must define:

- `name`: Display name (supports MiniMessage colors and placeholders).
- `slot`: Slot number (0-indexed).
- `itemStack.material`: Bukkit material name.
- `itemStack.amount`: Stack size.
- `lore`: List of lore lines.

Player heads and model data are supported when `material` is `PLAYER_HEAD`:

- `itemStack.value`: Custom model data integer **or** a player name, **or** a base64/URL/texture hash for a custom skin. Placeholders inside `value` are resolved before applying the texture.

### Attributes
Add optional `attributes:` entries to change flags or enchants:

- `[ENCHANTMENT] namespace;level` - adds an enchantment at the desired level.
- `[BOOLEAN] FLAG_NAME;true|false` - toggles an `ItemFlag` such as `HIDE_ATTRIBUTES` or `HIDE_ENCHANTS`.

Attributes are applied after the item is built so they also work on animation frames.

### Actions and click handling
Actions are evaluated per click type. Use `[LEFT_CLICK]`, `[RIGHT_CLICK]`, or `[BOTH]` followed by a space and the action payload. Supported targets are:

- `CONSOLE;command` - runs a console command with placeholders resolved.
- `PLAYER;command` - makes the player run a command (slash automatically added).
- `MESSAGE;text` - sends a formatted chat message.
- `BROADCAST;text` - sends a message to all online players and the console.
- `SOUND;sound;volume;pitch` - plays a sound for the clicking player (volume/pitch optional, defaults to 1.0).
- `OPEN_MENU;menu_name` - opens another Java menu by name.
- `REFRESH_MENU` - reopens the currently open menu for the player.
- `CONNECT;server_name` - sends the player to a proxy server (Bungee/Velocity auto-detected).
- `CLOSE` - closes the inventory (no extra data).

Only actions that match the actual click type execute.

### Display conditions
Add `display_conditions` to hide an item unless the condition passes. Two syntaxes are available:

- Simple list: every string in the list must be true.
- Map with `all`, `any`, `none` keys for complex logic.

Conditions can use boolean operators (`&&`, `||`, `!`), comparison (`>`, `>=`, `<`, `<=`, `==`, `!=`), string checks (`contains`, `startsWith`, `endsWith`, `equals`, `equalsIgnoreCase`, `matches`), and math expressions. Placeholders are resolved before evaluation.

### Animations
Define `animations:` to cycle through frames at a given `interval` (ticks). Each frame is an item definition with its own slot and optional attributes; frames are placed into the live inventory of players who have the menu open.

### Example (YAML)
```yaml
menuName: "<bold><gradient:#55FFFF:#5555FF>Example</gradient></bold>"
menuSize: 54
type: CHEST
openCommand: /example
items:
  logo:
    name: "<aqua>Logo"
    slot: 22
    itemStack:
      material: PLAYER_HEAD
      amount: 1
      value: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUv..." # base64 skin
    lore:
      - "<gray>Welcome!"
    attributes:
      - "[ENCHANTMENT] sharpness;1"
    actions:
      - "[LEFT_CLICK] MESSAGE;<green>Hello!"
    display_conditions:
      any:
        - "{player_level} >= 5"
animations:
  corner:
    interval: 5
    frames:
      frame1:
        slot: 0
        name: "&f"
        itemStack:
          material: BLUE_STAINED_GLASS_PANE
          amount: 1
      frame2:
        slot: 0
        name: "&f"
        itemStack:
          material: BLACK_STAINED_GLASS_PANE
          amount: 1
```

## Using the web editor

1. In-game, run `/bm editor` and click the link you receive to open the browser session.
2. Copy the command shown on the site, paste it back into the server console or chat, and run it to confirm (`/bm confirm <code>`).
3. Re-open the web editor; the YAML files from `plugins/BlueMenu/menus/java/` will be listed and can be edited live.