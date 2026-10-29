# First Steps

This guide covers the first decisions before installing Blue Premium on a
network.

## Purchase the Plugin

Download Blue Premium from the official Blueva store:

- [Blueva Store](https://blueva.net/store)

After purchase, download the correct JARs for your platform:

- Proxy JAR for BungeeCord, Waterfall, XCord or Velocity.
- Backend JAR for Spigot, Paper or compatible backend servers.

## Requirements

| Requirement | Value |
|-------------|-------|
| Java | Java 21 or higher |
| Proxy | BungeeCord, Waterfall, XCord, Velocity or compatible fork |
| Backend | Spigot, Paper or compatible fork |
| Minecraft | 1.8+ networks |
| Database | SQLite, MySQL or MariaDB |
| Optional dependency | PlaceholderAPI on backend servers |
| Optional support | Floodgate/GeyserMC for Bedrock players |

Blue Premium is a proxy authentication plugin. The proxy JAR is mandatory. The
backend JAR is strongly recommended on every server players can join.

## What Each JAR Does

| JAR | Install on | Purpose |
|-----|------------|---------|
| Proxy JAR | BungeeCord/Waterfall/XCord or Velocity | Login/register flow, premium detection, database access, sessions, recovery, TOTP, admin commands |
| Backend JAR | Spigot/Paper backend servers | Access-token verification, unauthenticated-player restrictions, captcha maps, PlaceholderAPI state |

Do not install the BungeeCord JAR on Velocity, and do not install the backend
JAR on the proxy.

## First Install Summary

1. Stop the proxy and backend servers.
2. Put the proxy JAR in the proxy `plugins/` folder.
3. Put the backend JAR in each backend server `plugins/` folder.
4. Start everything once so files are generated.
5. Stop everything again.
6. Configure the same `access_token` on proxy and backend.
7. Configure `servers.limbo`, `servers.main` and the database.
8. Start the network and test `/login`, `/register` and `/bluepremium help`.

For the detailed installation walkthrough, continue with
[Installation](./installation.md).

## License

If you bought or downloaded the plugin from the [Blueva Store](https://blueva.net/store), your license key is already filled in and there is nothing for you to do.

If you bought it anywhere else (BuiltByBit, PixelEast, Polymart, etc.), the key starts empty and the plugin reminds you in the console every time it starts. To obtain and activate your license:

1. Create a free account at [blueva.net](https://blueva.net/register), or sign in if you already have one.
2. Open your [account settings](https://blueva.net/account?tab=settings) and find the **License** section.
3. Click **Show**, then **Copy** to copy your license key.
4. Paste the key into the `license.key` field of `plugins/BluePremium/license.yml` on each backend server (the proxy JAR does not use a license file) and restart the server.

```yaml
license:
  key: "your-license-key"
```

> **Note:** The product will be permanently linked to the Blueva account used during activation. Once linked, it cannot be transferred or linked to a different Blueva account.

- The same key works for every Blueva product you own.
- Your license can be used from up to **six different IP addresses**, with no limit on the number of servers running under each IP. Authorized IPs can be viewed and managed from your account settings.
- If your key is lost or compromised, you can replace it from the same page and paste the new one into `license.yml`. The old key stops working everywhere at once.
- Do not edit the `purchase` block in `license.yml`. It is filled in by the store you downloaded from and is only used to confirm your purchase.

For help, visit [blueva.net/support](https://blueva.net/support).

## Generated Files

Proxy:

```text
plugins/BluePremium/
|-- configuration.yml
|-- messages.yml
`-- database.db        # only when SQLite is used
```

Backend:

```text
plugins/BluePremium/
|-- configuration.yml
`-- messages.yml
```

## Recommended Production Setup

For a production network, use:

- One authentication or limbo server for unauthenticated cracked players.
- One normal lobby or hub server for authenticated players.
- Backend Blue Premium on both limbo and normal servers.
- MySQL or MariaDB instead of SQLite.
- A unique 64-character access token shared between proxy and backend.
- `bluepremium.admin` only for trusted staff.

SQLite is fine for local tests and very small single-proxy networks, but MySQL
or MariaDB is the safer default for production.

## Existing JPremium Server

If you are replacing JPremium, do not start by changing production files.

1. Copy the old JPremium proxy folder.
2. Copy the old JPremium backend folder if it exists.
3. Copy the database or create a MySQL/MariaDB dump.
4. Build a test network.
5. Follow [JPremium Migration](./jpremium-migration.md).

Blue Premium can reuse the JPremium database schema, password hashes, many
configuration values, messages, aliases and legacy API package names.

## Next Steps

1. [Installation](./installation.md) - Install proxy and backend JARs.
2. [Database](./database.md) - Choose and configure storage.
3. [Authentication & Security](./authentication-security.md) - Tune login, sessions and protection.
4. [Backend & Captcha](./backend-captcha.md) - Protect backend servers.
5. [Commands & Permissions](./commands-permissions.md) - Review staff and player commands.
