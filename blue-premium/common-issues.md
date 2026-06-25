# Common Issues

Solutions to common Blue Premium installation, migration and runtime problems.

## Plugin Not Loading

### Symptoms

- Blue Premium does not appear in the plugin list.
- Console shows errors during startup.
- Commands return `Unknown command`.

### Solutions

**Check the JAR type.**

- Use the BungeeCord JAR on BungeeCord, Waterfall or XCord.
- Use the Velocity JAR on Velocity.
- Use the backend JAR on Spigot/Paper servers.

**Check Java.**

Blue Premium targets Java 21. Run:

```bash
java -version
```

**Check the console.**

Look for lines starting with `[BluePremium]` near startup.

## Backend Rejects Players

### Symptoms

- Players are kicked with an invalid session message.
- Players stay restricted after login.
- `/bluepremiumbackend status <player>` shows unauthenticated state.

### Solutions

**Match the access token.**

The proxy and every backend must use the same `access_token`.

**Restart after changing the token.**

Reload is not enough for every token-related platform state.

**Confirm players join through the proxy.**

Direct backend joins should be blocked or restricted.

## Existing JPremium Passwords Do Not Work

### Symptoms

- Migrated players cannot log in.
- New Blue Premium accounts work, old JPremium accounts do not.

### Solutions

**Check the database target.**

Confirm Blue Premium points to the migrated database, not a fresh empty one.

**Check `user_profiles`.**

The old JPremium table must exist in the selected database.

**Check SQL import order.**

Import JPremium SQL dumps into an empty test database first. Do not import the
same dump twice into the same database.

## Commands Conflict With Another Plugin

### Symptoms

- `/login`, `/register` or `/premium` is handled by another plugin.
- Blue Premium commands work only through another alias.

### Solutions

Edit proxy `configuration.yml`:

```yaml
command_aliases:
  login: [l]
  premium: []
  bluepremium: [bp, jpremium, jbungee]
```

Restart the proxy after changing aliases.

## Captcha Does Not Show

### Symptoms

- The player is asked for captcha but receives no map.
- The map appears on one backend but not another.

### Solutions

**Enable captcha on the proxy.**

```yaml
auth:
  verify_captcha_code: true
```

**Enable map slot on the backend.**

```yaml
captcha_map_slot: 1
```

**Install the backend JAR on every auth server.**

The proxy generates the code; the backend displays the map.

## Emails Are Not Sent

### Symptoms

- `/requestpasswordrecovery` succeeds but no email arrives.
- Console logs SMTP errors.

### Solutions

- Check `mail.host`, `mail.port`, `mail.user` and `mail.password`.
- Use port `587` with `use_tls: true` for most SMTP providers.
- Check spam folders.
- Confirm the player has a stored email with `/changemail`.
- Review provider rate limits.

## TOTP Codes Fail

### Symptoms

- `/login <password> <code>` rejects the code.
- `/totp verify <code>` fails.

### Solutions

- Synchronize the phone clock.
- Try the next fresh code.
- Confirm the player is using the correct account in the authenticator app.
- Use recovery codes if available.
- Staff can use force TOTP commands after verifying account ownership.

## Migrated Messages Show Raw Color Codes

### Symptoms

- Players see `&c`, `&a` or other raw legacy codes.

### Solutions

- Re-run `/bluepremium migrate` on a copy of the old folder.
- Review `plugins/BluePremium/messages.yml`.
- Convert remaining legacy colors to MiniMessage tags manually.
- Do not paste the old JPremium `messages.yml` directly over Blue Premium's file.

## Premium Players Are Sent To Limbo

### Symptoms

- Premium users do not auto-login.
- Premium users behave like cracked users.

### Solutions

- Confirm the proxy is in the expected online/mixed mode for your platform.
- Keep `auth.register_premium_users: true` when premium users should auto-register.
- Check Mojang/session-server connectivity.
- If using Floodgate, confirm Bedrock prefixes do not overlap Java names.

## Multi-Proxy Sessions Do Not Sync

### Symptoms

- A player logs in on one proxy and must log in again on another.

### Solutions

- Use MySQL or MariaDB, not SQLite.
- Point every proxy to the same database.
- Enable multi-proxy support.
- Give each proxy a stable unique `multi_proxy.proxy_id`.

## Where To Look First

Useful commands:

```text
/bluepremium version
/bluepremium check <player>
/bluepremium stats
/bluepremium audit
/bluepremiumbackend status <player>
```

Useful files:

```text
plugins/BluePremium/configuration.yml
plugins/BluePremium/messages.yml
plugins/BluePremium/database.db
```
