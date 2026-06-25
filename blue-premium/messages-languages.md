# Messages & Languages

Blue Premium uses MiniMessage for all user-facing text.

Main proxy messages:

```text
plugins/BluePremium/messages.yml
```

Backend messages:

```text
plugins/BluePremium/messages.yml
```

The proxy and backend each have their own data folder, so edit the file on the
server where the message is shown.

## MiniMessage Basics

Use MiniMessage tags:

```text
<red>Wrong password.
<green>Successfully logged in.
<gray>Use <white>/login <password></white>.
```

Common tags:

| Tag | Meaning |
|-----|---------|
| `<red>` | Red text |
| `<green>` | Green text |
| `<gray>` | Gray text |
| `<white>` | White text |
| `<bold>` | Bold text |
| `<gradient:#4FC3F7:#1565C0>` | Gradient start/end colors |

Do not use legacy `&` color codes in Blue Premium messages.

## Prefix

The `prefix` key can be reused with `<prefix>`:

```yaml
prefix: '<gradient:#4FC3F7:#1565C0><bold>BluePremium</bold></gradient> <dark_gray>></dark_gray> '

login:
  success: '<prefix><green>Successfully logged in.'
```

Change the prefix once instead of editing every message manually.

## Login And Register Blocks

After login or registration, Blue Premium can send multi-line chat blocks:

```yaml
login:
  success_chat: |-
    <gray>  <white>/startsession</white> - start a session.
    <gray>  <white>/totp enable</white> - enable two-factor authentication.

register:
  success_chat: |-
    <gray>Here are some useful commands:
    <gray>  <white>/changemail <password> <email></white>
```

Set the value to an empty string to disable a block.

## Join Titles

Join title sections include:

```text
join_require_login
join_require_register
join_require_security_login
join_session_premium
join_session_cracked
join_session_bedrock
```

Example:

```yaml
join_require_login:
  title: '<red>Log in'
  subtitle: '<gray>with <white>/login <password></white>'
  fade_in: 10
  stay: 60
  fade_out: 10
  chat: ''
```

Set `title`, `subtitle` and `chat` to empty strings to disable that output.

## Boss Bar And Action Bar

Login/register countdown messages are configurable:

```yaml
login:
  boss_bar_title: '<red>Log in to continue <dark_gray>|</dark_gray> <white>%time_short%</white>'
  boss_bar_color: 2
  boss_bar_division: 2
  action_bar: '<gray>Type <white>/login <password></white> <dark_gray>|</dark_gray> <red>%time%s left</red>'
```

Useful placeholders:

| Placeholder | Meaning |
|-------------|---------|
| `%time%` | Remaining seconds |
| `%time_ms%` | Remaining milliseconds |
| `%time_short%` | Short formatted time |
| `%progress%` | Progress value for boss bar |

## Migrated JPremium Messages

During `/bluepremium migrate`, Blue Premium imports JPremium messages and
converts common legacy colors to MiniMessage.

It also copies extra language files:

```text
messages_*.yml
messages_*.yaml
```

Converted variants may be written as reference files:

```text
jpremium-converted-messages_*.yml
```

Review migrated files before using them in production. Automated conversion can
handle normal color codes, but server-specific formatting should still be
checked by a human.

## Message Categories

Important proxy sections:

| Section | Purpose |
|---------|---------|
| `login` | Login prompt, success, failures, reminders, bars |
| `register` | Register prompt, success, password validation |
| `join_*` | Titles/chat shown after join |
| `session` | Manual session messages |
| `change_password` | Password change |
| `create_password` | First password creation |
| `change_mail` | Recovery email |
| `recovery` | Password recovery |
| `totp` | Two-factor authentication |
| `premium_login` | Premium verification |
| `captcha` | Captcha prompts |
| `admin_force` | `/force*` command suite |
| `anti_bot` | Anti-bot kicks |
| `disconnect_redirection` | Fallback routing notices |
| `ban` | IP/nickname ban messages |
| `update` | Admin update notifications |
| `stats` | `/bluepremium stats` |

Backend messages include command output, invalid-token text and migration
messages for backend config import.

## Reloading Messages

Proxy:

```text
/bluepremium reload
```

Backend:

```text
/bluepremiumbackend reload
```

Restart if a message is used during early startup or if another plugin caches
the old text.

## Troubleshooting

**Raw `&c` or `&a` appears in chat**

- Convert that line to MiniMessage.
- Do not paste the old JPremium file over Blue Premium's file.
- Re-run migration on a copy if needed.

**A MiniMessage tag appears as plain text**

- Check tag spelling.
- Close tags when needed.
- Test with a simple line first, then add gradients or hover text.

**A translated language file is incomplete**

- Compare it with the current generated `messages.yml`.
- Copy missing keys from the default file.
- Keep `file_version` as the first key.
