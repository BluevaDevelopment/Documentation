# BlueSpoofProxy

BlueSpoofProxy synchronizes fake player counts between backend Minecraft servers and your proxy (BungeeCord or Velocity). This makes the proxy's online player count include fake players.

## Installation

1. **Download** `BlueSpoofProxy-Bungee.jar` or `BlueSpoofProxy-Velocity.jar` depending on your proxy software
2. **Place** the JAR in your proxy's `plugins` folder
3. **Restart** the proxy

## Supported Proxy Software

- BungeeCord
- Waterfall
- Velocity

## Sync Methods

Choose exactly one sync method. The same method must be configured on **every** backend server and on the proxy.

| Method | Best For |
|--------|----------|
| **TCP** (default) | Single machine or trusted private network. Simplest setup. |
| **Redis** | Multiple machines with a shared Redis instance. |
| **SQL** | MySQL/MariaDB available. No extra services needed. |
| **Plugin** | Legacy BungeeCord plugin-message transport. Requires at least one real player online to carry messages. |

### TCP Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: tcp
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

### Redis Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: redis
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

### SQL Setup

**Backend `settings.yml`:**
```yaml
proxy:
  messaging:
    enabled: true
    method: sql
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

## Proxy Counter Spoofing

In addition to syncing backend fake players, the proxy can add extra fake players to the visible online count.

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

- `min_extra` — Always add at least this many players
- `percent_extra` — Add this percentage on top of the real + fake total
- `max_extra` — Cap the extra players added (-1 = no cap)
- `max_online` — Hard cap for the final visible count (-1 = no cap)
- `hourly_percent` — Additional percentage by hour of day

## Important Notes

- The `server_id` in each backend's `settings.yml` must match the server name configured in the proxy config.
- If you change the sync method, restart both the backend servers and the proxy.
- TCP is the recommended method for most setups because it does not require external services and works even when no real players are online.
