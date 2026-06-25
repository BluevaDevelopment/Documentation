# Authentication & Security

This page explains the main authentication and security settings.

## Account Types

Blue Premium supports mixed premium and cracked networks.

| Account type | Behavior |
|--------------|----------|
| Premium | Verified through Mojang/Microsoft and can auto-login |
| Cracked | Registers a password and must authenticate |
| Bedrock | Can be bypassed through Floodgate support when enabled |

## Core Auth Settings

```yaml
auth:
  register_premium_users: true
  second_login_cracked: false
  register_on_website: false
  unique_ids_type: FIXED
  detect_premium_uuid_in_handshake: false
  disconnect_redirection: true
  last_server_redirection: false
  floodgate_support: true
  verify_captcha_code: false
  confirm_password: false
  risky_commands_confirmation: true
```

| Key | Meaning |
|-----|---------|
| `register_premium_users` | Automatically creates premium profiles |
| `second_login_cracked` | Allows the second login attempt to fall back to cracked behavior |
| `register_on_website` | Kicks new cracked users to register on an external website |
| `unique_ids_type` | Offline UUID strategy for cracked players: `FIXED`, `RANDOM` or `MOJANG` |
| `detect_premium_uuid_in_handshake` | Uses signed profile data when available |
| `disconnect_redirection` | Sends kicked players to fallback servers when possible |
| `last_server_redirection` | Returns players to their last server after login |
| `floodgate_support` | Bypasses password prompts for Bedrock players when detected |
| `verify_captcha_code` | Requires captcha code in login/register flows |
| `confirm_password` | Requires repeated password arguments in register/change/create flows |
| `risky_commands_confirmation` | Uses `/confirm` for `/premium`, `/cracked` and `/unregister` |

## Sessions

```yaml
sessions:
  manual_session_minutes: 720
  automatic_session_minutes: 10
```

Manual sessions are created with `/startsession`. Automatic sessions are created
after login. Players can remove a manual session with `/stopsession` or the
legacy alias `/destroysession`.

## Login Limits

```yaml
security:
  max_login_tries: 3
  address_ban_duration_minutes: 10
  max_profiles_per_ip: 50
  max_authorisation_seconds: 90
```

| Key | Meaning |
|-----|---------|
| `max_login_tries` | Failed login attempts before disconnect or temporary ban; `0` disables this check |
| `address_ban_duration_minutes` | Temporary IP ban duration after the limit is reached |
| `max_profiles_per_ip` | Maximum profiles allowed from one IP address |
| `max_authorisation_seconds` | Time allowed to authenticate after joining |

## Password Policy

```yaml
security:
  password_hashing_algorithm: SHA256
  rehash_password_when_using_different_algorithm: true
  safe_password_pattern: '[\S]{6,25}'
```

Supported hashing algorithms:

- `SHA256`
- `SHA512`
- `BCRYPT`

Blue Premium can authenticate existing JPremium hashes. When rehashing is
enabled, an old hash is upgraded after a successful login if it uses a different
algorithm than the configured one.

The optional `passwords.txt` file blocks weak/common passwords.

## Nickname Protection

```yaml
security:
  allowed_nickname_pattern: '[a-zA-Z0-9_]{3,16}'
  block_dangerous_unicode_in_nickname: true
  reject_duplicate_connections: true
  accepted_hostnames: []
```

Keep dangerous Unicode blocking enabled unless you intentionally support unusual
nickname formats. It blocks invisible and bidirectional control characters that
can make staff moderation confusing.

## Anti-Bot Guard

```yaml
anti_bot:
  enabled: false
  max_connections_per_address_per_minute: 6
  cooldown_seconds: 120
  global_lockdown_threshold: 0
  global_lockdown_seconds: 30
  trust_known_addresses: true
```

Enable this when the proxy is exposed to mass connection attacks. Use:

```text
/bluepremium antibot
/bluepremium antibot reset
```

to inspect or clear runtime guard state.

## Protected Accounts

```yaml
protected_accounts: []
```

Add UUIDs that staff commands must not mutate. This protects important accounts
from `/forceunregister`, `/forcecracked`, `/forcepremium` and similar commands.

## Account Type Switching

```yaml
allow_switch_account_type: true
```

When enabled, players can use:

```text
/premium <password>
/cracked <password>
```

Disable this if your network wants account type to be controlled only by staff.
