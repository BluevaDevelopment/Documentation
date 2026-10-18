# Proxy Sync

BlueSpoof synchronizes fake players and player counts between backend Minecraft servers and **BungeeCord or Velocity**.

`plugin` is the default sync method. Install BlueSpoof on the proxy and participating backends to synchronize automatically, without extra ports, a database, Redis, or manually assigned backend IDs.

## Quick Start (Recommended)

BlueSpoof uses one universal JAR. There is no separate proxy download.

1. Install the same **BlueSpoof 3.9.2 or newer** JAR in the proxy's `plugins` folder and on every backend you want to synchronize.
2. Keep your normal BungeeCord/Velocity backend addresses and player-forwarding configuration. BlueSpoof discovers the backend names and addresses from that server list.
3. Keep backend Minecraft ports **firewalled so only your proxies can reach them**. No additional port needs to be opened for BlueSpoof.
4. Start or restart the proxy and backends. New BlueSpoof configurations already enable `plugin` synchronization.
5. Connect a fake player on a backend, for example with `/bs connect 1`. The proxy's visible online count will include it.

The proxy companion requires **Java 17 or newer**, or a newer Java version if required by your proxy software. Backends keep the Java requirements of their Minecraft version; a Java 8 legacy backend can synchronize with a proxy running newer Java.

For an existing installation, follow [Upgrading an Existing Network](#upgrading-an-existing-network) rather than assuming your previous settings were changed.

### Configuration Locations

| Server | File |
|--------|------|
| Paper/Spigot/Folia backend | `plugins/BlueSpoof/settings.yml` |
| BungeeCord proxy | `plugins/BlueSpoofProxy/settings.yml` |
| Velocity proxy | `plugins/bluespoofproxy/settings.yml` |

### Default Plugin Configuration

These are the relevant defaults, not extra settings you need to add to a fresh installation. Edit the existing sections rather than creating duplicate YAML keys.

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: plugin
```

**Proxy `settings.yml`:**
```yaml
sync:
  method: plugin
  plugin:
    timeout_millis: 1500
  poll_seconds: 2
```

Plugin mode ignores `proxy.messaging.server_id` and discovers the name automatically. You do not need to configure `tcp.host`, `tcp.port`, Redis, SQL, or `known_servers` for this method. Those optional sections can remain in the configuration without being used.

## How Plugin Mode Works

The proxy makes short service connections to each backend's **existing Minecraft port**. BlueSpoof recognizes its own protocol before Minecraft decodes the connection; ordinary Minecraft traffic passes through unchanged.

The proxy first reads the backend snapshot, then sends the updated network state. This keeps counters, fake-player assignments and enabled network rank synchronization up to date.

Both BungeeCord and Velocity use this bridge. Update the proxy and participating backends together to use it; the proxy also accepts standard snapshots from older backends for compatibility.

Updates are not instantaneous: the proxy polls every **2 seconds** by default, and backends refresh periodic snapshots every **120 ticks** (6 seconds at 20 TPS). Fake-player joins and quits also publish snapshots. Briefly unavailable backends are retried automatically.

## Security

Plugin synchronization generates keys and pairs the proxy and backends automatically.
No manual secret is required. After pairing, sessions are authenticated and encrypted;
unknown identities do not receive a BlueSpoof bridge response and follow the normal
Minecraft connection path instead. The old public bridge protocol is not accepted.

**Keep backend Minecraft ports firewalled so only your trusted proxies can reach them.**
Automatic pairing trusts the first connection, so do the initial setup on a trusted network.
An attacker who reaches an unpaired backend could pair first; automatic key generation
cannot verify the administrator's intent during that first contact.

- Keys and trusted peers are stored in `data/bridge-security/` inside each instance's plugin
  data directory. Preserve this folder across restarts, upgrades and migrations, and back
  it up securely. `identity.json` contains a private key: do not share it publicly.
- Once paired, an identity change is rejected rather than silently trusted. Restore the
  original identity from backup; do not delete security files as routine troubleshooting.
- Automatic pairing stays dormant on **online-mode standalone
  servers**. Standalone owners do not need to disable authentication.
- Sessions use fresh challenges and directional **AES-GCM encryption/authentication**.
  Captured greetings or messages cannot simply be replayed to submit state in another session.
- Invalid or unknown greetings pass unchanged to Minecraft. This avoids a custom public
  product banner after pairing, but is not a guarantee of complete undetectability:
  initial pairing, observed legitimate traffic or other plugin behavior can reveal information.

### Pairing Notices

A new successful pairing prints an **orange notice in both consoles** after startup has
finished, identifying the peer's
address and a short public identity fingerprint. The proxy also shows the configured backend
name. No private key is printed. Existing pairings are not announced again
on every poll or restart. Investigate a notice for an unexpected peer before continuing.

Plugin mode uses automatic pairing only, with no manual shared-secret setting. For
manually configured deployments, see the alternative transports below.

### Replacing an Instance or Using Multiple Proxies

A backend automatically trusts one proxy identity. For multiple authorized proxies, first
start each proxy with backends unreachable to generate its security files, then stop it.
Securely provision the same proxy `data/bridge-security/identity.json` to them before
pairing, retaining each instance's own valid `peers.json` trust file. A missing trust
file is rejected rather than silently recreated. Do not copy a backend identity
onto a proxy, or publish private identity files.

For an intentional identity replacement, stop the affected instances, protect the network,
and explicitly remove the affected peer entries from `data/bridge-security/peers.json`
inside the `peers` JSON object on both sides before pairing again, keeping `"format": 1`
and the `peers` object, even when empty. Missing or malformed
trust files fail closed. Deleting all entries resets trust for all peers in that
installation. An unexpected identity change should be investigated, not automatically approved.

## Upgrading an Existing Network

1. Update the proxy and participating backends to **3.9.2 or newer** using the same JAR.
2. Check `proxy.messaging.enabled: true` and `proxy.messaging.method: plugin` on each backend.
3. Set `sync.method: plugin` in the proxy's BlueSpoof `settings.yml`.
4. Restart the proxy and backends on a trusted network to complete automatic pairing.

Configuration updates preserve edited values, so an existing TCP/Redis/SQL installation may keep its previous method. You can continue using that method; only change it if you want to switch to the default bridge.

Old `server_id`, TCP, Redis and SQL settings do not need to be deleted when switching to `plugin`; they are ignored by that method.

## Alternative Sync Methods

Choose one method and use the same method on the proxy and participating backends. **Plugin is recommended for normal setups**; the other transports remain available for existing networks or special hosting requirements.

| Method | Use |
|--------|-----|
| **Plugin** (default) | Automatic bridge over existing backend Minecraft ports, with automatic backend discovery. |
| **TCP** | Separate backend-to-proxy socket listener on a configured port. |
| **Redis** | Shared Redis instance across the network. |
| **SQL** | Shared MySQL/MariaDB database. |

For **TCP, Redis and SQL only**, each backend needs a unique `proxy.messaging.server_id` matching its name in the proxy's server list. In the examples below, that name is `survival`.

### TCP Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: tcp
    server_id: survival
    tcp:
      host: 127.0.0.1
      port: 25780
```

**Proxy `settings.yml`:**
```yaml
sync:
  method: tcp
  tcp:
    enabled: true
    host: 127.0.0.1
    port: 25780
```

The backend's `tcp.host` points to the proxy; the proxy's `sync.tcp.host` is its listener bind address. `127.0.0.1` only works when both run on the same machine. For separate hosts, use reachable private-network addresses and restrict access to the TCP listener.

### Redis Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: redis
    server_id: survival
    redis:
      host: 127.0.0.1
      port: 6379
      password: ''
      database: 0
      key_prefix: 'bluespoof:proxy:'
```

**Proxy `settings.yml`:**
```yaml
sync:
  method: redis
  redis:
    host: 127.0.0.1
    port: 6379
    password: ''
    database: 0
    key_prefix: 'bluespoof:proxy:'
```

Use the same Redis instance, database and key prefix across the network. Replace the address and credentials as appropriate for your deployment.

### SQL Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: sql
    server_id: survival
    sql:
      jdbc_url: jdbc:mariadb://127.0.0.1:3306/bluespoof
      username: bluespoof
      password: ''
      table: bluespoof_proxy_snapshots
```

**Proxy `settings.yml`:**
```yaml
sync:
  method: sql
  sql:
    jdbc_url: jdbc:mariadb://127.0.0.1:3306/bluespoof
    username: bluespoof
    password: ''
    table: bluespoof_proxy_snapshots
```

Use the same database and table on all instances. MySQL is also supported with a `jdbc:mysql://` URL.

## Reloads and Backend Restarts

- `/bs reload` restarts the backend bridge and its synchronization tasks. Confirm the command if prompted.
- After changing transport settings, restarting the affected backends and proxy is the simplest way to apply everything consistently. A backend reload does not reload proxy configuration.
- If a backend becomes unavailable, the proxy can reassign its fake players to an available fallback backend using the proxy's configured fallback order. When their home server returns, the assignments are restored.
- This does **not** turn fake players into actual connected proxy players. Proxy plugins that require a real `ProxyServer#getPlayer(...)` connection cannot automatically treat them as real players.

## Proxy Counter Spoofing

Counter spoofing adds an extra amount to the visible proxy online count after the synchronized real + fake total is calculated. It does **not** create additional fake players on backend servers.

```yaml
counter_spoof:
  enabled: false
  min_extra: 0
  percent_extra: 0
  max_extra: -1
  max_online: -1
  hourly_percent:
    '18': 5
    '19': 10
    '20': 10
    '21': 8
```

- `min_extra` - Always add at least this many players
- `percent_extra` - Add this percentage on top of the real + fake total
- `max_extra` - Cap the extra players added (-1 = no cap)
- `max_online` - Hard cap for the final visible count (-1 = no cap)
- `hourly_percent` - Additional percentage by hour of day

## Troubleshooting

### The proxy count does not include backend fake players

1. Verify that the **same current JAR** is installed on the proxy and backends, not only on one side.
2. Check that the selected method matches on both sides and that backend messaging is enabled.
3. For plugin mode, verify the backend address/port in the proxy's normal server list is reachable from the proxy. Do not open an extra BlueSpoof port.
4. Check that the backend uses offline-mode behind your normal proxy forwarding and firewall. Online-mode standalone servers leave this bridge dormant.
5. Check both consoles and allow time for polling and snapshot updates.

### The proxy logs `Plugin sync to ... failed`

Unavailable backends are retried silently, so starting the proxy before the backends does not produce connection warnings. This message is reserved for protocol or security failures and is reported once per endpoint until recovery.

Check the backend is started, BlueSpoof loaded successfully, its configured address is reachable, existing pairing identities are intact and server clocks are synchronized. If the backend reports that the plugin bridge could not start on your server fork, use TCP/Redis/SQL on both sides instead.

### Server-specific placeholders use the wrong name

In plugin mode, use the server name from BungeeCord/Velocity's server list. The old backend `server_id` is ignored. With TCP/Redis/SQL, use the matching configured `server_id`. See [Proxy Placeholders](placeholders.md#proxy-placeholders).
