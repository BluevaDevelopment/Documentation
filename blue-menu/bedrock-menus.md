# Bedrock Menus

This page explains Floodgate/Geyser-compatible menus for Bedrock players, both in YAML and through the web editor.

## Menu anatomy (YAML)
Bedrock menus live in `plugins/BlueMenu/menus/bedrock/` and must define `type` as `SIMPLE`, `MODAL`, or `CUSTOM`. The menu title and content support colors and placeholders. Menu files are loaded and opened when referenced by name.

### Shared actions
Button/component actions accept the same targets as Java menus. Actions run in order after the player makes a selection:

- `CONSOLE;cmd` — runs a console command with placeholders resolved.
- `PLAYER;cmd` — makes the player run a command (slash automatically added).
- `MESSAGE;text` — sends a formatted chat message.
- `BROADCAST;text` — sends a message to all online players and the console.
- `SOUND;sound;volume;pitch` — plays a sound for the player (volume/pitch optional, defaults to 1.0).
- `OPEN_MENU;menu_name` — opens another Bedrock menu by name.
- `REFRESH_MENU` — reopens the currently open menu for the player.
- `CONNECT;server_name` — sends the player to a proxy server (Bungee/Velocity auto-detected).
- `CLOSE` — closes the menu (no extra data).

### Display conditions
`display_conditions` can wrap buttons or components. Use a simple list (all true) or an `all`/`any`/`none` map. Conditions support the same operators as Java menus and are evaluated before the element is added; hidden elements are skipped entirely.

## SIMPLE forms
SIMPLE forms show a list of buttons and optional images.

- `content`: A list of lines combined into the body text.
- `buttons`: Each entry defines `text`, optional `image` (URL), `actions`, and optional `display_conditions`.

Only buttons that pass their conditions are shown; clicks are matched by text to trigger the associated actions.

### Example (SIMPLE)
```yaml
type: SIMPLE
menuName: "&bWelcome"
content:
  - "&7Pick an option"
buttons:
  start:
    text: "&aStart"
    actions:
      - "PLAYER;spawn"
  store:
    text: "&eStore"
    image: "https://example.com/icon.png"
    actions:
      - "CONSOLE;warp store {player_name}"
    display_conditions:
      any:
        - "{vault_eco_balance} >= 100"
```

## MODAL forms
Modal forms always show two buttons and run their actions only when the related conditions pass.

- `content`: Body text list.
- `buttons.button1` and `buttons.button2`: Each contains `text`, `actions`, and optional `display_conditions` (controls action execution, not visibility).

Actions fire only when the matching button is clicked **and** its conditions evaluate to true.

## CUSTOM forms
Custom forms let you mix components. Each component has `type`, `text`, optional defaults, `actions`, and optional `display_conditions`.

Supported component types:
- `DROPDOWN`: `options` list and `default` index.
- `INPUT`: `placeholder` and `default` text.
- `TOGGLE`: `default` boolean.
- `SLIDER`: `min`, `max`, `step`, and `default` values.
- `STEPSLIDER`: `steps` list and `default` index.
- `LABEL`: Static text, no actions.

Actions can reference player placeholders and `{component_key}` values (plus `{component_key_text}` for dropdown/stepslider labels) collected from the submitted form.

### Example (CUSTOM)
```yaml
type: CUSTOM
menuName: "&dFeedback"
components:
  username:
    type: INPUT
    text: "Name"
    placeholder: "Your IGN"
    actions:
      - "MESSAGE;&aThanks, {username}!"
  rate:
    type: STEPSLIDER
    text: "Rate us"
    steps:
      - "Bad"
      - "Okay"
      - "Great"
    default: 1
    actions:
      - "MESSAGE;&eYou picked {rate_text}"
      - "CONSOLE;eco give {player_name} 5"
```

## Using the web editor
1. Run `/bm editor` in-game and open the link in your browser.
2. Copy the verification command from the site, paste it in-game or console (`/bm confirm <code>`), and run it.
3. Reopen the browser tab; your Bedrock menu YAML files will be available to edit.