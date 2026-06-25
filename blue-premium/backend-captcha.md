# Backend & Captcha

The backend plugin protects Spigot/Paper servers after the proxy has accepted a
connection. It is also responsible for map captcha rendering.

## Why Install The Backend Plugin

Install it on every backend server players can join.

It provides:

- Access-token verification from the proxy.
- Restriction of unauthenticated players.
- Auth spawn teleport and blindness effect.
- Captcha map display.
- PlaceholderAPI expansion.
- Direct backend join protection.

## Access Token

The backend only trusts a proxy that sends the same token:

```yaml
access_token: 'same-token-as-proxy'
```

If the token is wrong, the backend will reject or restrict the player according
to the current configuration.

## Restrictions

```yaml
restrictions:
  movement: true
  interactions: true
  blindness_effect: true
  commands: true
  block_held_item_change: true
  block_hand_swap: true
  allowed_commands: []
```

| Key | Meaning |
|-----|---------|
| `movement` | Locks unauthenticated player movement |
| `interactions` | Blocks world, entity, inventory and item interactions |
| `blindness_effect` | Applies blindness while unauthenticated |
| `commands` | Blocks backend commands while unauthenticated |
| `block_held_item_change` | Prevents switching hotbar slot |
| `block_hand_swap` | Prevents off-hand swap |
| `allowed_commands` | Backend command whitelist without `/` |

On most networks, login/register commands are handled by the proxy, so
`allowed_commands` can stay empty.

## Auth Spawn

Set the auth spawn in-game:

```text
/bluepremiumbackend setspawn
```

or configure it manually:

```yaml
spawn_location: 'world,0,80,0,0,0'
force_respawn_to_spawn: false
```

Unauthenticated players are teleported to the auth spawn. If
`force_respawn_to_spawn` is enabled, all players respawn there after death.

## Captcha Map

Enable captcha on the proxy:

```yaml
auth:
  verify_captcha_code: true
```

Enable map display on the backend:

```yaml
captcha_map_slot: 1
captcha_message: '<aqua>Your captcha is <white>%captcha_code%</white>. Enter it as the last argument of /login or /register.'
```

`captcha_map_slot` uses hotbar slots `1-9`. Set it to `0` to disable map items.

Players enter the captcha as the last argument:

```text
/login <password> <captcha>
/register <password> <captcha>
```

If password confirmation is enabled:

```text
/register <password> <password> <captcha>
```

## Captcha Appearance

Default palette:

```yaml
captcha_palette:
  background_color_rgb: '#EEEEEE'
  border_color_rgb: '#222222'
  noise_color_rgb: '#AAAAAA'
  text_color_rgb: '#0D47A1'
```

For a custom background, place this file on the backend:

```text
plugins/BluePremium/captcha_background.png
```

The image is resized to 128x128 and converted to the Minecraft map palette.

## Backend Commands

```text
/bluepremiumbackend help
/bluepremiumbackend reload
/bluepremiumbackend version
/bluepremiumbackend setspawn
/bluepremiumbackend status
/bluepremiumbackend status <player>
/bluepremiumbackend migrate
```

All backend commands require:

```text
bluepremium.admin
```

## Troubleshooting

**Everyone stays restricted after login**

- Confirm the backend JAR is installed on the server the player joined.
- Confirm the access token matches proxy and backend.
- Restart proxy and backend after changing the token.

**Captcha map does not appear**

- Enable `auth.verify_captcha_code` on the proxy.
- Set `captcha_map_slot` to `1-9` on the backend.
- Confirm the player is on a backend with the Blue Premium backend JAR.

**Players can move before login**

- Enable `restrictions.movement`.
- Confirm the backend plugin is installed on that server.
- Confirm players are not bypassing through a server without the backend plugin.
