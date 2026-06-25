# Configuration

Blue Premium uses two configuration files: one on the proxy and one on each
backend server. Both files must keep `file_version` as the first key.

## Proxy Configuration

Proxy path:

```text
plugins/BluePremium/configuration.yml
```

Main sections:

| Section | Purpose |
|---------|---------|
| `storage` | SQLite, MySQL or MariaDB connection and pool settings |
| `access_token` | Shared secret used by backend servers |
| `servers` | Limbo and main server routing |
| `auth` | Login flow, premium detection, sessions, captcha, Floodgate and multi-proxy toggles |
| `anti_bot` | Connection-rate protection |
| `multi_proxy` | Cross-proxy session synchronization |
| `sessions` | Manual and automatic session duration |
| `security` | Login attempts, IP limits, password policy, nickname policy and hostnames |
| `protected_accounts` | Accounts that `/force*` commands cannot mutate |
| `mail` | SMTP password recovery |
| `totp` | Two-factor display name |
| `logout_commands` | Commands allowed before authentication |
| `command_aliases` | Player and admin command aliases |

After changing proxy config, run:

```text
/bluepremium reload
```

Restart the proxy after changing command aliases, storage type, access token or
platform-sensitive login behavior.

## Backend Configuration

Backend path:

```text
plugins/BluePremium/configuration.yml
```

Main sections:

| Key or section | Purpose |
|----------------|---------|
| `access_token` | Must match the proxy token |
| `captcha_map_slot` | Hotbar slot for map captcha, or `0` to disable maps |
| `captcha_message` | Chat message shown with captcha |
| `captcha_palette` | Default map captcha colors |
| `restrictions` | Movement, interaction, command and inventory protection |
| `spawn_location` | Auth spawn location |
| `force_respawn_to_spawn` | Respawn behavior |
| `invalid_token_message` | Kick/restriction message for invalid token state |

After changing backend config, run:

```text
/bluepremiumbackend reload
```

Restart the backend after changing the access token, captcha map assets or
restriction behavior that did not refresh as expected.

## Access Token

The token is the most important shared setting. Generate one:

```bash
openssl rand -hex 32
```

Set the same value on the proxy and every backend:

```yaml
access_token: 'paste-token-here'
```

Never use the default token in production.

## Server Routing

```yaml
servers:
  limbo: [limbo]
  main: [lobby]
```

`servers.limbo` is used for unauthenticated cracked players. `servers.main` is
used after authentication and for disconnect redirection. Names must match the
proxy server names exactly.

## Messages

Messages live in:

```text
plugins/BluePremium/messages.yml
```

Blue Premium uses MiniMessage. Use tags such as:

```text
<red>Wrong password.
<green>Login successful.
<gray>Use <white>/login <password></white>.
```

Do not use legacy `&` color codes in Blue Premium messages.

## Reload Safety

Most text, limits and toggles can be reloaded. Restart when changing:

- Database type or database credentials.
- Access token.
- Command aliases.
- Proxy platform/server routing changes.
- Backend captcha background image.
- Floodgate or mixed-mode behavior.
