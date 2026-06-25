# Installation

This guide explains how to install Blue Premium from a clean network setup. If
you are replacing JPremium, read [JPremium Migration](./jpremium-migration.md)
before following this page on your production files.

## Table of Contents

1. [Requirements](#requirements)
2. [Install the Proxy Plugin](#install-the-proxy-plugin)
3. [Install the Backend Plugin](#install-the-backend-plugin)
4. [Configure the Access Token](#configure-the-access-token)
5. [Configure Server Routing](#configure-server-routing)
6. [Choose a Database](#choose-a-database)
7. [Core Authentication Settings](#core-authentication-settings)
8. [Backend Restrictions](#backend-restrictions)
9. [Captcha Setup](#captcha-setup)
10. [Email Recovery](#email-recovery)
11. [TOTP Two-Factor Authentication](#totp-two-factor-authentication)
12. [Floodgate and Bedrock](#floodgate-and-bedrock)
13. [Multi-Proxy Networks](#multi-proxy-networks)
14. [First Login Test](#first-login-test)
15. [Troubleshooting](#troubleshooting)

---

## Requirements

| Requirement | Value |
|-------------|-------|
| Java | Java 21 or newer |
| Proxy | BungeeCord, Waterfall, XCord, Velocity or compatible fork |
| Backend | Spigot, Paper or compatible fork |
| Minecraft | 1.8+ |
| Database | SQLite, MySQL or MariaDB |

You need at least one proxy plugin. Install the backend plugin on every server
that should enforce authentication state, captcha maps or direct-join
protection.

## Install the Proxy Plugin

1. Stop the proxy.
2. Put the correct Blue Premium proxy JAR in the proxy `plugins/` folder.
   - BungeeCord/Waterfall/XCord: use the BungeeCord JAR.
   - Velocity: use the Velocity JAR.
3. Start the proxy once.
4. Wait until Blue Premium creates its files.
5. Stop the proxy again before editing the configuration.

After the first start, the proxy creates:

```text
plugins/BluePremium/
|-- configuration.yml
|-- messages.yml
`-- database.db        # only when SQLite is used
```

---

## Install the Backend Plugin

Install the backend plugin on every Spigot/Paper server that players can join
after authentication.

1. Stop the backend server.
2. Put the Blue Premium backend JAR in `plugins/`.
3. Start the backend once.
4. Wait until it creates `plugins/BluePremium/`.
5. Stop the backend again before editing the configuration.

The backend creates:

```text
plugins/BluePremium/
|-- configuration.yml
`-- messages.yml
```

The backend plugin does not replace the proxy plugin. It only trusts the proxy
after the shared access token matches.

---

## Configure the Access Token

The access token is the shared secret between the proxy and every backend
server. If it does not match, the backend will reject the connection or keep the
player restricted.

Generate a random 64-character token:

```bash
openssl rand -hex 32
```

Set the same value in the proxy configuration:

```yaml
access_token: 'paste-your-random-64-character-token-here'
```

Set the same value in every backend configuration:

```yaml
access_token: 'paste-your-random-64-character-token-here'
```

Rules:

- Never leave the default `change-me-to-a-random-64-char-string` in production.
- Use the exact same token on the proxy and all backend servers.
- Do not publish the token in logs, screenshots or public configuration files.
- Restart proxy and backend servers after changing it.

---

## Configure Server Routing

Blue Premium needs to know where unauthenticated and authenticated players
should be sent.

```yaml
servers:
  limbo: [limbo]
  main: [lobby]
```

| Key | Meaning |
|-----|---------|
| `servers.limbo` | Servers used while a cracked player still needs to log in or register |
| `servers.main` | Servers used after authentication |

The names must match your proxy server names.

Example BungeeCord `config.yml`:

```yaml
servers:
  limbo:
    address: 127.0.0.1:25566
  lobby:
    address: 127.0.0.1:25567
```

Example Blue Premium routing:

```yaml
servers:
  limbo: [limbo]
  main: [lobby]
```

Recommended setup:

- Use a small limbo/lobby server for unauthenticated cracked players.
- Install the backend plugin on the limbo server and all normal servers.
- Keep the names stable. If the proxy calls the server `hub`, Blue Premium must
  also use `hub`.

---

## Choose a Database

Blue Premium supports `SQLITE`, `MYSQL` and `MARIADB`.

| Type | Best for | Advantages | Trade-offs |
|------|----------|------------|------------|
| `SQLITE` | Local testing, very small single-proxy networks | No external service, instant setup, one local file | Not recommended for multi-proxy or high-availability production |
| `MYSQL` | Most production networks | Shared database, external backups, works with multi-proxy setups | Requires a database server |
| `MARIADB` | Production networks using MariaDB | Same benefits as MySQL, common in hosting panels | Requires a database server |

### SQLite

```yaml
storage:
  type: SQLITE
```

Use SQLite when you are testing or running a simple single-proxy setup. Blue
Premium will create `plugins/BluePremium/database.db`.

### MySQL

```yaml
storage:
  type: MYSQL
  host: '127.0.0.1'
  port: 3306
  database: 'bluepremium'
  user: 'bluepremium'
  password: 'strong-password'
  properties: []
```

### MariaDB

```yaml
storage:
  type: MARIADB
  host: '127.0.0.1'
  port: 3306
  database: 'bluepremium'
  user: 'bluepremium'
  password: 'strong-password'
  properties: []
```

### Connection Pool

```yaml
storage:
  pool:
    size: 10
    idle: 10
    timeout: 5000
    lifetime: 1800000
    keepalive: 0
```

| Key | Description |
|-----|-------------|
| `size` | Maximum database connections in the pool |
| `idle` | Idle connections kept ready |
| `timeout` | Maximum time in milliseconds to wait for a connection |
| `lifetime` | Maximum connection lifetime in milliseconds |
| `keepalive` | Optional keepalive interval in milliseconds; `0` disables it |

For most networks, keep the defaults unless your database host asks for
specific pool limits.

---

## Core Authentication Settings

```yaml
auth:
  register_premium_users: true
  second_login_cracked: false
  register_on_website: false
  unique_ids_type: FIXED
  detect_premium_uuid_in_handshake: false
  disconnect_redirection: true
  last_server_redirection: false
  delay_titles_after_join_ms: 1000
  floodgate_support: true
  floodgate_bedrock_prefix: '.'
  multi_proxy_support: false
  verify_captcha_code: false
  confirm_password: false
  risky_commands_confirmation: true
```

| Key | Recommended default | Description |
|-----|---------------------|-------------|
| `register_premium_users` | `true` | Automatically creates profiles for premium players |
| `second_login_cracked` | `false` | Controls whether cracked users need a second login flow in specific premium-detection cases |
| `register_on_website` | `false` | Blocks in-game registration and asks users to register on your website |
| `unique_ids_type` | `FIXED` | Offline UUID strategy for cracked players. `FIXED` is the recommended stable mode |
| `detect_premium_uuid_in_handshake` | `false` | Enables Mojang signed-profile detection where supported |
| `disconnect_redirection` | `true` | Sends players to a safe server when a backend disconnects them |
| `last_server_redirection` | `false` | Sends players back to their last server after login |
| `verify_captcha_code` | `false` | Requires captcha during login/register when enabled |
| `confirm_password` | `false` | Requires repeated password arguments for register/create/change flows |
| `risky_commands_confirmation` | `true` | Requires `/confirm` for risky commands such as `/premium`, `/cracked` and `/unregister` |

---

## Backend Restrictions

Backend restrictions protect the server while a player is not authenticated.

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

Recommended production setup:

- Keep `movement`, `interactions` and `commands` enabled.
- Keep `block_held_item_change` enabled if captcha maps are used.
- Only add commands to `allowed_commands` if they must be usable before login.

If your login/register commands are handled on the proxy, `allowed_commands` can
usually stay empty.

---

## Captcha Setup

Captcha has two parts:

1. The proxy generates the captcha code.
2. The backend renders it on a map item.

Proxy:

```yaml
auth:
  verify_captcha_code: true
```

Backend:

```yaml
captcha_map_slot: 1
captcha_message: '<aqua>Your captcha is <white>%captcha_code%</white>. Enter it as the last argument of /login or /register.'
```

Slot values:

| Value | Meaning |
|-------|---------|
| `0` | Disable captcha map item |
| `1` to `9` | Hotbar slot where the captcha map is placed |

Optional custom background:

1. Create a 128x128 image or any square image.
2. Name it `captcha_background.png`.
3. Place it in the backend `plugins/BluePremium/` folder.
4. Restart the backend.

Blue Premium will resize and convert it to the Minecraft map palette.

---

## Email Recovery

Configure SMTP on the proxy:

```yaml
mail:
  host: 'smtp.example.com'
  port: 587
  user: 'no-reply@example.com'
  password: 'smtp-password'
  name: 'Your Server'
  use_tls: true
  recovery_subject: 'Password Recovery'
  recovery_delay_minutes: 60
```

Players use:

```text
/requestpasswordrecovery <email>
/confirmpasswordrecovery <token> <newPassword>
```

Recommended setup:

- Use a real mailbox or transactional email provider.
- Use TLS on port `587` unless your provider requires something else.
- Set SPF/DKIM/DMARC on your domain to avoid recovery emails going to spam.
- Test recovery with a normal player account before opening the server.

---

## TOTP Two-Factor Authentication

Set the display name used by authenticator apps:

```yaml
totp:
  server_name: 'My Server'
```

Player commands:

```text
/totp enable
/totp confirm <code>
/totp verify <code>
/totp disable <code>
/totp recoverycodes
/totp recover <code>
```

The login command also accepts the code directly:

```text
/login <password> <code>
```

---

## Floodgate and Bedrock

If your network uses Geyser/Floodgate, keep Bedrock support enabled:

```yaml
auth:
  floodgate_support: true
  floodgate_bedrock_prefix: '.'
```

Use the same prefix configured in Floodgate. If your Floodgate setup does not
prefix Bedrock player names, set this to an empty string:

```yaml
auth:
  floodgate_bedrock_prefix: ''
```

Blue Premium also attempts safe runtime detection when Floodgate is installed.

---

## Multi-Proxy Networks

For several proxies sharing the same database:

```yaml
multi_proxy:
  enabled: true
  proxy_id: 'proxy-1'
```

Rules:

- Use MySQL or MariaDB.
- Every proxy must use the same database.
- Every proxy must have a unique `proxy_id`.
- Every proxy and backend must use the same `access_token`.

This allows active sessions to be shared through the database so a player can
reconnect through another proxy without being forced through unnecessary login
steps while the session is still valid.

---

## First Login Test

After configuring everything:

1. Start the database.
2. Start all backend servers.
3. Start the proxy.
4. Join with a cracked test account.
5. Confirm you are sent to the limbo/login server.
6. Run:

```text
/register testPassword testPassword
```

or, when `confirm_password` is disabled:

```text
/register testPassword
```

7. Disconnect and reconnect.
8. Run:

```text
/login testPassword
```

9. Confirm restrictions disappear after login and you are sent to the main server.
10. Join with a premium account and confirm premium auto-login behaves as expected.

Admin checks:

```text
/bluepremium check <player>
/bluepremium stats
/bluepremium help
```

---

## Troubleshooting

**Players are rejected by the backend**

- The backend `access_token` does not match the proxy.
- The backend was not restarted after changing the token.
- The player joined the backend directly instead of through the proxy.

**Players stay in limbo after login**

- Check `servers.main`.
- Make sure the server name exists in the proxy configuration.
- Check that the target backend is online.

**SQLite works in testing but production feels unstable**

- Move to MySQL or MariaDB.
- SQLite is convenient, but an external database is safer for production backups
  and multi-proxy growth.

**Captcha map does not appear**

- Install the backend plugin on the login/limbo server.
- Set `auth.verify_captcha_code: true` on the proxy.
- Set `captcha_map_slot` to a value from `1` to `9` on the backend.
- Keep held-item restrictions enabled so players cannot move the map away.

**Premium users are not detected**

- Make sure the proxy is configured correctly for your online/offline mode setup.
- Review `auth.detect_premium_uuid_in_handshake`.
- Check the proxy console for Mojang lookup or handshake warnings.

**Email recovery does not send**

- Check SMTP host, port, user and password.
- Use TLS when required by your provider.
- Check spam folders and mail provider logs.
