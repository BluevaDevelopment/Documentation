# Runtime Validation

This is the final validation step before replacing JPremium in production.
Repository tests and migration coverage prove the mappings and packaged classes,
but only a real proxy/backend test network proves command dispatch, backend
restrictions, chat rendering and Minecraft client behavior.

Use a copied test network. Do not run this first on production.

## Required Test Stack

- Java 21.
- One BungeeCord or Velocity proxy.
- One Paper, Spigot or compatible backend server.
- Blue Premium proxy JAR for the selected proxy platform.
- Blue Premium backend JAR.
- A copied JPremium proxy data folder.
- A copied JPremium backend data folder if your old setup used one.
- A copied SQLite file, MySQL/MariaDB database, or SQL dump.

## Data Folder Inputs

Proxy JPremium folder:

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

Not every installation has every file. Missing optional files are acceptable,
but the migration should report clearly what was found.

Backend JPremium folder:

```text
configuration.yml
image.png
```

## Startup Validation

1. Stop the test network.
2. Install the Blue Premium proxy JAR.
3. Install the Blue Premium backend JAR.
4. Start the proxy and backend once so `plugins/BluePremium/` is generated.
5. Stop the network again.
6. Copy `JPremium/` next to the proxy `BluePremium/` folder.
7. Copy `JPremium-Backend/` or `JPremiumBackend/` next to the backend `BluePremium/` folder if applicable.
8. Start the network.
9. Confirm the proxy logs that a JPremium folder was detected.
10. Run `/bluepremium migrate`.
11. Run `/bluepremiumbackend migrate` on backend servers that have legacy backend data.
12. Restart the network after reviewing any `database.db.new` or SQL dump warning.

Expected result:

- Proxy config is imported.
- Proxy messages are imported and rendered as MiniMessage.
- Language files are copied as references and converted variants are written when possible.
- SQLite is staged safely, or SQL/manual database reuse is reported.
- `passwords.txt` is copied when present.
- `recovery.html` is copied as `recovery-template.legacy.html` when present.
- Backend config is imported.
- Backend `image.png` is copied as `captcha_background.png` when present.
- Only documented unsupported warnings appear: `useBackupServer` and `accessTokenDisabled`.

## Player Flow Tests

Run these with migrated JPremium accounts.

| Flow | Command or action | Expected result |
|------|-------------------|-----------------|
| Cracked login | `/login <password>` | Existing JPremium password hash authenticates |
| Cracked login with 2FA | `/login <password> <code>` | Existing JPremium TOTP secret works |
| TOTP fallback | `/totp verify <code>` | Pending 2FA login completes |
| Register confirm | `/register <password> <password>` | Confirm-password behavior follows migrated config |
| Manual session | `/startsession` | Session is stored and reused |
| Session alias | `/destroysession` | Legacy alias removes the session |
| Recovery request | `/requestpasswordrecovery <email>` | Stored email is accepted |
| Recovery alias | `/recoverypassword <email>` | Legacy alias requests recovery |
| Recovery confirm | `/confirmpasswordrecovery <token> <newPassword> (confirm)` | Password changes |
| Email alias | `/changeemailaddress <password> <email>` | Legacy alias changes recovery email |
| Premium switch | `/premium <password>` | Confirmation and premium conversion work |
| Cracked switch | `/cracked <password>` | Confirmation and cracked conversion work |
| Unregister | `/unregister <password>` | Account is removed after confirmation when configured |

## Admin Flow Tests

Run these as an operator or a user with `bluepremium.admin` or accepted legacy
JPremium admin permissions.

| Flow | Command or action | Expected result |
|------|-------------------|-----------------|
| Legacy root | `/jpremium` and `/jbungee` | Blue Premium admin root responds |
| Help coverage | `/bluepremium help` | JPremium `/force*` section and aliases render |
| Force login | `/forcelogin <player>` | Player is authenticated |
| Force register | `/forceregister <player> <password>` | Profile is created |
| Force unregister | `/forceunregister <player>` | Profile is removed and online state clears |
| Force password | `/forcechangepassword <player> <password>` | Password changes |
| Force create password | `/forcecreatepassword <player> <password>` | Missing password is created |
| Force premium | `/forcepremium <player>` | Account switches or expected duplicate/lookup guard appears |
| Force cracked | `/forcecracked <player>` | Account switches back to cracked |
| Force recovery alias | `/forcerecoverypassword <player>` | Legacy alias works |
| Force recovery confirm | `/forceconfirmpasswordrecovery <player> <password>` | Pending recovery applies without token text |
| Force email alias | `/forcechangeemailaddress <player> <email>` | Legacy alias works |
| Force session start | `/forcestartsession <player>` | Manual session is created |
| Force session destroy | `/forcedestroysession <player>` | Session is removed |
| Force 2FA request | `/forcerequestsecondfactor <player>` | 2FA setup request is stored |
| Force 2FA activate | `/forceactivatesecondfactor <player>` | 2FA activates |
| Force 2FA deactivate | `/forcedeactivatesecondfactor <player>` | 2FA disables |
| Force purge alias | `/forcepurge <player>` | Legacy alias purges profile |
| Force view profile | `/forceviewuserprofile <player>` | Profile details render |
| Force view alias | `/forceview <address>` | Legacy alias lists matching profiles |
| Force merge | `/forcemergepremiumuserprofile <current> <previous>` | Premium identity merges into previous cracked profile |
| Stats | `/bluepremium stats` | Runtime counters render |
| Audit | `/bluepremium audit` | Security audit entries render |
| Check | `/bluepremium check <player>` | Profile, session, TOTP and ban state render |

## Backend Tests

| Flow | Action | Expected result |
|------|--------|-----------------|
| Access token | Join backend through proxy | Backend accepts authenticated proxy state |
| Direct backend join | Join backend directly | Backend rejects or restricts according to config |
| Captcha map | Trigger login/register captcha | Backend receives map and shows custom/migrated background |
| Restrictions | Move, chat or use inventory while unauthenticated | Configured restrictions block actions |
| Auth release | Complete login | Restrictions clear and captcha map slot is cleaned |
| PlaceholderAPI | Read `%bluepremium_account_type%` and related placeholders | Values reflect live authentication state |

## Message And Alias Checks

Confirm migrated custom messages appear in:

- Login errors.
- Register errors.
- Password recovery.
- Risky-command confirmation.
- Anti-bot kick.
- Hostname rejection.
- Update/admin notices.

Confirm fallback aliases work:

```text
/l
/destroysession
/recoverypassword
/changeemailaddress
/forcerecoverypassword
/forcechangeemailaddress
/forcepurge
/forceview
```

## Pass Criteria

The runtime validation is passed when:

- Proxy and backend start without Blue Premium errors.
- Migration commands finish with only documented unsupported-key warnings.
- Existing migrated users can authenticate with old JPremium credentials.
- Existing migrated TOTP users can complete 2FA.
- Password recovery works with migrated templates/messages.
- Admin `/force*` flows and legacy aliases work.
- `/bluepremium help` exposes the JPremium force-command suite.
- Backend restrictions and proxy-to-backend auth state clear correctly after login.
- No migrated message renders raw legacy `&` formatting in chat.

Only after this checklist passes should you repeat the migration on production.
