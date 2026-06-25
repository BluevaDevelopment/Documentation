# Blue Premium (test 6)

Blue Premium is Blueva's authentication solution for mixed Minecraft networks.
It replaces JPremium while keeping the migration path familiar for servers that
already depend on JPremium-style premium/cracked authentication.

Use Blue Premium when your network needs:

- Premium auto-login for real Mojang accounts.
- Cracked account registration and password login.
- Sessions, password recovery, TOTP two-factor authentication and captcha.
- BungeeCord or Velocity proxy support with Spigot/Paper backend protection.
- JPremium database, configuration, message and command compatibility.

## Recommended Reading Order

1. [First Steps](./first-steps.md) - Purchase, requirements, file layout and first startup.
2. [Installation](./installation.md) - Install the proxy and backend plugins.
3. [Configuration](./configuration.md) - Understand every main proxy and backend configuration area.
4. [Database](./database.md) - Choose SQLite, MySQL or MariaDB and tune the pool.
5. [Authentication & Security](./authentication-security.md) - Configure login flow, sessions, account switching, anti-bot and bans.
6. [Backend & Captcha](./backend-captcha.md) - Protect backend servers and configure map captcha.
7. [Email Recovery & TOTP](./email-totp.md) - Enable email recovery and two-factor authentication.
8. [Multi-Proxy & Floodgate](./multi-proxy-floodgate.md) - Configure shared sessions and Bedrock bypass.
9. [Messages & Languages](./messages-languages.md) - Customize MiniMessage text, titles and migrated language files.
10. [Commands & Permissions](./commands-permissions.md) - Review every player, admin and backend command.
11. [Placeholders](./placeholders.md) - Use Blue Premium state in PlaceholderAPI-compatible plugins.
12. [API & Events](./api-events.md) - Integrate plugins with Blue Premium or legacy JPremium-compatible APIs.
13. [JPremium Migration](./jpremium-migration.md) - Move an existing JPremium installation to Blue Premium safely.
14. [Runtime Validation](./runtime-validation.md) - Run the final smoke test before production cutover.
15. [Common Issues](./common-issues.md) - Troubleshoot startup, database, login and backend issues.

## Architecture

Blue Premium is split into two runtime parts:

| Part | Where it runs | Purpose |
|------|---------------|---------|
| Proxy plugin | BungeeCord or Velocity | Authentication, database access, sessions, premium detection, email recovery, TOTP, captcha generation, admin commands |
| Backend plugin | Spigot, Paper or compatible forks | Verifies the proxy access token, blocks unauthenticated actions, receives auth state and renders captcha maps |

The proxy plugin is mandatory. The backend plugin is strongly recommended for
production networks because it prevents players from bypassing the proxy and
restricts unauthenticated players on the server side.

## Important Paths

| Component | Default folder |
|-----------|----------------|
| Proxy data | `plugins/BluePremium/` on the proxy |
| Backend data | `plugins/BluePremium/` on each backend server |
| Proxy configuration | `plugins/BluePremium/configuration.yml` |
| Proxy messages | `plugins/BluePremium/messages.yml` |
| Backend configuration | `plugins/BluePremium/configuration.yml` |
| Backend messages | `plugins/BluePremium/messages.yml` |

## Support Scope

Blue Premium supports Minecraft 1.8+ networks. This wide support range is
intentional because authentication plugins often sit at the center of older
production networks.

For new installations, start with [First Steps](./first-steps.md).
For existing JPremium servers, read [JPremium Migration](./jpremium-migration.md)
before replacing any JARs, then complete [Runtime Validation](./runtime-validation.md)
on a copied test network.
