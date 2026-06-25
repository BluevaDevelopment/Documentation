# Placeholders

Blue Premium registers PlaceholderAPI placeholders from the backend plugin when
PlaceholderAPI is installed.

Install PlaceholderAPI on each backend server where you want to use these
placeholders in scoreboards, menus, chat formats or holograms.

## Availability

| Requirement | Value |
|-------------|-------|
| Plugin | Blue Premium backend JAR |
| Dependency | PlaceholderAPI |
| Identifier | `bluepremium` |

Placeholders are backend-side. They reflect the player's state on the server
where PlaceholderAPI is evaluating them.

## Player State Placeholders

| Placeholder | Output |
|-------------|--------|
| `%bluepremium_authenticated%` | `true` or `false` |
| `%bluepremium_logged_in%` | `true` when the proxy confirmed login |
| `%bluepremium_play_active%` | `true` when the player is fully past authentication |
| `%bluepremium_status%` | `authenticated`, `pending_login` or `unauthenticated` |
| `%bluepremium_account_type%` | `premium`, `cracked` or `unknown` |
| `%bluepremium_registered%` | `true` when the player has been seen/logged in before |
| `%bluepremium_has_session%` | `true` when the player has an active play session |

## Captcha Placeholders

| Placeholder | Output |
|-------------|--------|
| `%bluepremium_has_captcha%` | `true` when the player currently has a captcha challenge |
| `%bluepremium_captcha_code%` | Current captcha code or empty |

Use the captcha code placeholder carefully. It is useful for controlled menus or
debugging, but exposing it in public scoreboards defeats the point of captcha.

## Server And Player Placeholders

| Placeholder | Output |
|-------------|--------|
| `%bluepremium_version%` | Blue Premium backend version |
| `%bluepremium_player_name%` | Player display name |
| `%bluepremium_ip%` | Player remote IP address |
| `%bluepremium_prefix%` | Configured message prefix |
| `%bluepremium_online_count%` | Online player count on the backend |
| `%bluepremium_server_name%` | Backend plugin/server name |

## Example Uses

Scoreboard line:

```text
Auth: %bluepremium_status%
```

Menu condition:

```text
%bluepremium_play_active% == true
```

Lobby display:

```text
Account: %bluepremium_account_type%
```

## Troubleshooting

**Placeholders stay empty**

- Install PlaceholderAPI on the backend server.
- Confirm the Blue Premium backend JAR is installed.
- Restart the backend after installing PlaceholderAPI.

**State looks wrong after server transfer**

- Confirm the proxy and backend access token match.
- Confirm the backend receives Blue Premium auth state.
- Use `/bluepremiumbackend status <player>` to inspect backend state.
