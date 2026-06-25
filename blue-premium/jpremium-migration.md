# JPremium Migration

This guide explains how to move an existing JPremium installation to Blue
Premium safely. Read it completely before replacing files on a production
network.

## Table of Contents

1. [What Migrates](#what-migrates)
2. [Before You Start](#before-you-start)
3. [Migration Strategy](#migration-strategy)
4. [Proxy Migration](#proxy-migration)
5. [Backend Migration](#backend-migration)
6. [Database Migration](#database-migration)
7. [Messages and Languages](#messages-and-languages)
8. [Command Compatibility](#command-compatibility)
9. [Known Differences](#known-differences)
10. [Runtime Validation](#runtime-validation)
11. [Post-Migration Checklist](#post-migration-checklist)
12. [Rollback Plan](#rollback-plan)
13. [Troubleshooting](#troubleshooting)

---

## What Migrates

Blue Premium was designed to keep JPremium migrations predictable.

| JPremium data | Blue Premium behavior |
|---------------|-----------------------|
| `configuration.yml` | Mapped into Blue Premium proxy configuration |
| `messages.yml` | Imported and converted from legacy color codes to MiniMessage |
| `messages_*.yml` / `messages_*.yaml` | Copied as references and converted when possible |
| `database.db` | Staged as `database.db.new` for safe replacement after restart |
| `database.sql` / `database_export.sql` | Detected and reported for manual MySQL/MariaDB import |
| Existing MySQL/MariaDB tables | Reused directly when the schema is JPremium-compatible |
| `passwords.txt` | Copied and used as the weak-password list |
| `recovery.html` | Copied as `recovery-template.legacy.html` |
| Backend `configuration.yml` | Mapped into Blue Premium backend configuration |
| Backend `image.png` | Copied as `captcha_background.png` |
| Command aliases | Migrated for supported public and admin commands |

Blue Premium also keeps compatibility bridges for common JPremium API package
variants used by third-party plugins.

---

## Before You Start

Do not migrate directly on your only production copy.

1. Stop the whole network.
2. Back up the proxy folder.
3. Back up every backend server folder.
4. Back up the JPremium database.
5. If using MySQL or MariaDB, create a database dump.
6. Copy the production folders to a test network first.

Minimum backup list:

```text
plugins/JPremium/
plugins/JPremium-Backend/        # if present
plugins/JPremiumBackend/         # if present
plugins/BluePremium/             # if you already tested Blue Premium
database.db or MySQL/MariaDB dump
```

Recommended database dump:

```bash
mysqldump -u USER -p DATABASE_NAME > jpremium-backup.sql
```

---

## Migration Strategy

There are two safe ways to migrate.

### Strategy A: Use the Existing Database Directly

Use this for MySQL/MariaDB or when you already have a trusted database backup.

1. Configure Blue Premium to point to the same database.
2. Start the proxy.
3. Run `/bluepremium migrate` to import config/messages and supporting files.
4. Test logins with existing accounts.

Blue Premium uses the same `user_profiles` shape as JPremium and supports
undashed UUID values, existing password hashes, legacy timestamps and active
TOTP secrets.

### Strategy B: Let Blue Premium Stage SQLite

Use this when your old JPremium setup used `database.db`.

1. Place the copied `JPremium/` folder next to the Blue Premium folder.
2. Run `/bluepremium migrate`.
3. Blue Premium stages the SQLite database as `database.db.new`.
4. Stop the proxy.
5. Restart the proxy so the staged database can be promoted safely.

This avoids overwriting a SQLite file while the database pool is open.

---

## Proxy Migration

Expected folder layout on the proxy before migration:

```text
plugins/
|-- BluePremium/
|   |-- configuration.yml
|   `-- messages.yml
`-- JPremium/
    |-- configuration.yml
    |-- messages.yml
    |-- database.db
    |-- passwords.txt
    `-- recovery.html
```

Steps:

1. Stop the proxy.
2. Install the Blue Premium proxy JAR.
3. Start the proxy once so `plugins/BluePremium/` is generated.
4. Stop the proxy.
5. Copy the old `plugins/JPremium/` folder next to `plugins/BluePremium/`.
6. Start the proxy.
7. Run:

```text
/bluepremium migrate
```

8. Read every warning printed by the command.
9. Restart the proxy after reviewing staged database files.

The command imports as much as possible and warns when a JPremium setting has no
safe Blue Premium equivalent.

---

## Backend Migration

If JPremium backend data exists, migrate it too.

Accepted legacy backend folder names:

```text
plugins/JPremium-Backend/
plugins/JPremiumBackend/
```

Expected layout:

```text
plugins/
|-- BluePremium/
|   `-- configuration.yml
`-- JPremium-Backend/
    |-- configuration.yml
    `-- image.png
```

Run on the backend server:

```text
/bluepremiumbackend migrate
```

This imports compatible backend settings and copies `image.png` to
`captcha_background.png` when no Blue Premium custom captcha background exists.

---

## Database Migration

### SQLite

JPremium SQLite file:

```text
plugins/JPremium/database.db
```

Blue Premium stages it as:

```text
plugins/BluePremium/database.db.new
```

On restart, Blue Premium can promote the staged file and back up the previous
Blue Premium database as:

```text
plugins/BluePremium/database.db.bak
```

### MySQL and MariaDB

If your JPremium database already lives in MySQL or MariaDB, point Blue Premium
to the same database:

```yaml
storage:
  type: MYSQL
  host: '127.0.0.1'
  port: 3306
  database: 'jpremium'
  user: 'jpremium'
  password: 'password'
```

or:

```yaml
storage:
  type: MARIADB
  host: '127.0.0.1'
  port: 3306
  database: 'jpremium'
  user: 'jpremium'
  password: 'password'
```

Blue Premium reads and writes JPremium-compatible rows, including:

- Undashed UUIDs in `uniqueId` and `premiumId`.
- Existing salted password hashes.
- JPremium timestamp formats.
- Existing session expiration values.
- Active 2FA secrets stored in legacy fields.

### SQL Dumps

If the old folder contains:

```text
database.sql
database_export.sql
```

Blue Premium reports the file path. Import SQL dumps manually into MySQL or
MariaDB before pointing Blue Premium to that database.

---

## Messages and Languages

Blue Premium imports JPremium messages and converts legacy color codes to
MiniMessage.

Examples:

| JPremium legacy | Blue Premium MiniMessage |
|-----------------|--------------------------|
| `&cWrong password` | `<red>Wrong password` |
| `&8[&a&l>&8] &7Logged in` | MiniMessage equivalent using tags |

Imported message categories include:

- Login/register prompts and errors.
- Password recovery.
- Risky command confirmations.
- Anti-bot messages.
- Hostname rejection.
- Premium nickname-change notices.
- Admin command messages.
- Legacy recovery email subject/body where present.

Extra language files are kept as references and converted variants are written
when possible. Review translated files before using them in production.

---

## Command Compatibility

Blue Premium keeps JPremium-friendly command names and aliases.

Common player aliases:

| JPremium command/alias | Blue Premium support |
|------------------------|----------------------|
| `/login` | Supported |
| `/l` | Supported |
| `/register` | Supported |
| `/changepassword` | Supported |
| `/changeemailaddress` | Supported as alias |
| `/requestpasswordrecovery` | Supported |
| `/recoverypassword` | Supported as alias |
| `/startsession` | Supported |
| `/destroysession` | Supported as alias |
| `/premium` | Supported |
| `/cracked` | Supported |
| `/unregister` | Supported |

Admin force commands:

```text
/forcelogin
/forceregister
/forceunregister
/forcechangepassword
/forcecreatepassword
/forcechangemail
/forcepremium
/forcecracked
/forcerequestpasswordrecovery
/forceconfirmpasswordrecovery
/forcestartsession
/forcedestroysession
/forcerequestsecondfactor
/forceactivatesecondfactor
/forcedeactivatesecondfactor
/forcepurgeuserprofile
/forceviewuserprofile
/forceviewuserprofiles
/forcemergepremiumuserprofile
```

Legacy force aliases:

```text
/forcechangeemailaddress
/forcerecoverypassword
/forcepurge
/forceview
```

Admin root aliases:

```text
/bluepremium
/jpremium
/jbungee
```

Run `/bluepremium help` after migration to see the available admin commands.

---

## Known Differences

Some old JPremium settings cannot be migrated safely because the underlying
behavior is intentionally different in Blue Premium.

| JPremium item | Blue Premium behavior |
|---------------|-----------------------|
| `useBackupServer` | Warned during migration. Blue Premium uses explicit server routing and disconnect redirection instead |
| Backend `accessTokenDisabled` | Warned during migration. Blue Premium keeps backend token validation enabled for security |
| Old per-message title delay keys | Blue Premium uses a global post-join title/message delay |
| Bundled PHP website | Not embedded. Blue Premium keeps the plugin-side `register_on_website` gate and message |
| Old converter plugins | Not needed for JPremium-to-Blue-Premium migration. They converted other auth plugins into JPremium |

Warnings for these items are expected. Any other warning should be reviewed
before opening the server.

---

## Runtime Validation

Repository tests can verify mappings, database compatibility and packaged
classes, but they cannot prove that the proxy, backend and Minecraft client
state behave correctly on your exact network. Always run a runtime smoke test on
a copied test network before touching production.

### Required Test Stack

Use:

- Java 21.
- One BungeeCord or Velocity proxy.
- One Paper, Spigot or compatible backend server.
- A copied JPremium proxy data folder.
- A copied JPremium backend data folder, if your old setup used one.
- A copied database or a MySQL/MariaDB dump.

The copied JPremium proxy folder should contain as many of these files as your
old installation has:

```text
configuration.yml
messages.yml
database.db
database.sql
database_export.sql
passwords.txt
recovery.html
messages_*.yml
messages_*.yaml
```

The copied backend folder should contain:

```text
configuration.yml
image.png
```

### Startup Smoke Test

1. Install the Blue Premium proxy JAR and backend JAR on the stopped test network.
2. Place the copied JPremium proxy folder next to `BluePremium`.
3. Place the copied JPremium backend folder next to the backend `BluePremium` folder.
4. Start the proxy and backend once.
5. Confirm Blue Premium detects the JPremium folder.
6. Run `/bluepremium migrate` on the proxy.
7. Run `/bluepremiumbackend migrate` on the backend if backend data exists.
8. Restart the network after reviewing `database.db.new` or any SQL dump notice.

Expected result:

- Proxy configuration values are imported.
- Proxy messages are imported and displayed as MiniMessage.
- Legacy language files are copied as references and converted variants are written.
- SQLite is staged as `database.db.new`, or SQL dump/manual database reuse is reported.
- `passwords.txt` is copied.
- `recovery.html` is copied to `recovery-template.legacy.html`.
- Backend configuration values are imported.
- Backend `image.png` is copied to `captcha_background.png`.
- The only expected unsupported-key warnings are `useBackupServer` and `accessTokenDisabled`.

### Player Flow Smoke Tests

Run these with migrated accounts, not fresh Blue Premium accounts.

| Flow | Command or action | Expected result |
|------|-------------------|-----------------|
| Cracked login | `/login <password>` | Existing JPremium password hash authenticates without reset |
| Cracked login with 2FA | `/login <password> <code>` | Existing JPremium TOTP secret works |
| TOTP fallback | `/totp verify <code>` | Pending 2FA login completes |
| Registration confirm | `/register <password> <password>` | Confirm-password behavior follows the migrated config |
| Session start | `/startsession` | Session is stored and reused |
| Session legacy alias | `/destroysession` | Alias destroys the session |
| Password recovery request | `/requestpasswordrecovery <email>` | Existing stored email is accepted |
| Password recovery alias | `/recoverypassword <email>` | Alias requests recovery |
| Password recovery confirm | `/confirmpasswordrecovery <token> <newPassword> (confirm)` | Password changes and the old hash no longer works |
| Email change alias | `/changeemailaddress <password> <email>` | Alias changes email |
| Premium switch | `/premium` | Risky-command confirmation and premium conversion work |
| Cracked switch | `/cracked` | Risky-command confirmation and cracked conversion work |
| Unregister | `/unregister <password>` | Migrated account is removed after confirmation when configured |

### Admin Flow Smoke Tests

Run these as an operator or as a user with `bluepremium.admin` or legacy
`jpremium.*` permissions.

| Flow | Command or action | Expected result |
|------|-------------------|-----------------|
| Legacy root command | `/jpremium` and `/jbungee` | Blue Premium admin root responds |
| Admin help coverage | `/bluepremium help` | JPremium `/force*` command section and legacy force aliases render |
| Force login | `/forcelogin <player>` | Player is authenticated |
| Force register | `/forceregister <player> <password>` | Profile is created |
| Force unregister | `/forceunregister <player>` | Profile is removed and online state is cleared |
| Force password | `/forcechangepassword <player> <password>` | Password changes |
| Force create password | `/forcecreatepassword <player> <password>` | Missing password is created |
| Force premium | `/forcepremium <player>` | Account switches to premium mode or reports the expected duplicate/lookup guard |
| Force cracked | `/forcecracked <player>` | Account switches back to cracked mode |
| Force recovery alias | `/forcerecoverypassword <player>` | Legacy alias works |
| Force recovery confirm | `/forceconfirmpasswordrecovery <player> <password>` | Pending recovery is applied without requiring the token text |
| Force email alias | `/forcechangeemailaddress <player> <email>` | Alias changes email |
| Force session start | `/forcestartsession <player>` | Manual session is created |
| Force session destroy | `/forcedestroysession <player>` | Session is removed across proxies |
| Force 2FA request | `/forcerequestsecondfactor <player>` | 2FA setup request is stored |
| Force 2FA activate | `/forceactivatesecondfactor <player>` | 2FA activates for the player |
| Force 2FA deactivate | `/forcedeactivatesecondfactor <player>` | 2FA disables for the player |
| Force purge alias | `/forcepurge <player>` | Legacy alias purges profile |
| Force view profile | `/forceviewuserprofile <player>` | Profile details render |
| Force view alias | `/forceview <address>` | Legacy alias lists matching profiles |
| Force merge premium profile | `/forcemergepremiumuserprofile <current> <previous>` | Premium identity merges into the previous cracked profile |
| Admin stats | `/bluepremium stats` | Runtime counters render |
| Admin audit | `/bluepremium audit` | Security audit entries render |
| Admin check | `/bluepremium check <player>` | Profile, session, TOTP and ban state render |

### Backend Smoke Tests

| Flow | Action | Expected result |
|------|--------|-----------------|
| Access token | Join backend through proxy | Backend accepts authenticated proxy state |
| Direct backend join | Join backend directly | Backend rejects or restricts according to config |
| Captcha map | Trigger login/register captcha | Backend receives the map and shows custom/migrated background |
| Restrictions | Move, chat or use inventory while unauthenticated | Configured restrictions block actions |
| Auth release | Complete login | Restrictions clear and captcha map slot is cleaned |
| PlaceholderAPI | Read `%bluepremium_account_type%` and related placeholders | Values reflect live authentication state |

The runtime validation is passed only when the selected proxy platform and
backend complete these checks without plugin errors and without raw legacy `&`
formatting appearing in chat.

---

## Post-Migration Checklist

Run this on a test network before production:

1. Start proxy and backend without plugin errors.
2. Confirm `/bluepremium migrate` finishes with only expected warnings.
3. Confirm `/bluepremiumbackend migrate` finishes with only expected warnings.
4. Log in with an existing cracked JPremium account:

```text
/login <oldPassword>
```

5. Log in with an existing 2FA account:

```text
/login <oldPassword> <code>
```

6. Test password recovery:

```text
/requestpasswordrecovery <email>
/confirmpasswordrecovery <token> <newPassword>
```

7. Test session commands:

```text
/startsession
/destroysession
```

8. Test important admin commands:

```text
/bluepremium help
/bluepremium check <player>
/forcelogin <player>
/forcedestroysession <player>
/forceviewuserprofile <player>
```

9. Confirm backend restrictions clear after login.
10. Confirm no migrated message appears with raw `&` color codes.
11. Confirm premium accounts still auto-login.
12. Confirm cracked accounts cannot bypass login by joining a backend directly.

---

## Rollback Plan

Keep rollback simple:

1. Stop the network.
2. Restore the old JPremium plugin JARs.
3. Restore the old `plugins/JPremium/` folders.
4. Restore the database backup if Blue Premium wrote to a shared database during
   testing.
5. Start the network and verify JPremium logins.

If you used a copied test database, rollback is only needed on the test network.
Production should not be touched until the test migration is verified.

---

## Troubleshooting

**Migration command does not find JPremium**

- Confirm the folder is named `JPremium`.
- Confirm it is next to `BluePremium`, not inside it.
- Restart once after placing the folder.

**Existing passwords do not work**

- Confirm Blue Premium points to the correct database.
- Confirm the old `user_profiles` table exists.
- Check whether you imported the SQL dump into the same database configured in
  `storage.database`.

**Players keep old raw color codes in messages**

- Re-run `/bluepremium migrate`.
- Review `plugins/BluePremium/messages.yml`.
- Make sure you are using the migrated Blue Premium messages file, not the old
  JPremium file directly.

**Backend rejects everyone after migration**

- Configure the same `access_token` on proxy and backend.
- Restart both proxy and backend.
- Confirm players join through the proxy.

**Captcha changed visually**

- If JPremium backend had `image.png`, run `/bluepremiumbackend migrate`.
- Confirm `plugins/BluePremium/captcha_background.png` exists on the backend.
- Restart the backend.

**MySQL import has duplicate errors**

- Import into an empty test database first.
- Check whether the JPremium dump was already imported.
- Never import the same dump twice into the same database without cleaning it.
