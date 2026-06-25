# Database

Blue Premium supports SQLite, MySQL and MariaDB. The database stores profiles,
password hashes, sessions, bans, IP history, TOTP secrets and multi-proxy
session state.

## Storage Types

| Type | Recommended for | Advantages | Trade-offs |
|------|-----------------|------------|------------|
| `SQLITE` | Local testing and very small single-proxy networks | No external service, easy backup, generated automatically | Not recommended for multi-proxy or high-availability production |
| `MYSQL` | Most production networks | Shared storage, external backups, panel support, multi-proxy ready | Requires a database server |
| `MARIADB` | Production networks using MariaDB | Same production benefits as MySQL | Requires a database server |

For production, use MySQL or MariaDB unless you have a very small network and a
clear backup plan.

## SQLite

```yaml
storage:
  type: SQLITE
```

The proxy creates:

```text
plugins/BluePremium/database.db
```

Stop the proxy before copying or replacing the SQLite file.

## MySQL

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

## MariaDB

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

## JDBC Properties

Use `storage.properties` for extra JDBC parameters:

```yaml
storage:
  properties:
    - 'useSSL=false'
    - 'characterEncoding=utf8'
```

Only use properties you understand. Invalid `key=value` entries are ignored and
logged.

## Connection Pool

```yaml
storage:
  pool:
    size: 10
    idle: 10
    timeout: 5000
    lifetime: 1800000
    keepalive: 0
```

| Key | Meaning |
|-----|---------|
| `size` | Maximum pool size |
| `idle` | Maximum idle connections |
| `timeout` | Connection timeout in milliseconds |
| `lifetime` | Maximum connection lifetime in milliseconds |
| `keepalive` | HikariCP keepalive in milliseconds, `0` disables it |

Increase the pool only when the database server can handle it. More connections
are not automatically faster.

## JPremium Database Reuse

Blue Premium can reuse a JPremium database directly.

Supported compatibility details:

- Same `user_profiles` table shape.
- Undashed UUIDs in `uniqueId` and `premiumId`.
- Existing JPremium password hashes.
- Existing session expiration values.
- Legacy timestamp formats.
- Active JPremium TOTP secrets stored in legacy fields.

For SQLite migrations, `/bluepremium migrate` stages the old file as:

```text
plugins/BluePremium/database.db.new
```

Restart after reviewing the staged file.

For MySQL/MariaDB migrations, import the dump first or point Blue Premium to the
existing database after backing it up.

## Backups

Recommended backup schedule:

- SQLite: copy `database.db` while the proxy is stopped.
- MySQL/MariaDB: use `mysqldump` or your hosting panel.
- Always back up before running migration commands.

Example:

```bash
mysqldump -u USER -p DATABASE_NAME > bluepremium-backup.sql
```

## Troubleshooting

**Connection refused**

- Check host, port, firewall and database service status.
- Confirm the database user can connect from the proxy host.

**Authentication works on one proxy but not another**

- Use MySQL or MariaDB.
- Enable and configure multi-proxy support.
- Confirm all proxies point to the same database.

**JPremium users cannot log in**

- Confirm Blue Premium points to the migrated database.
- Confirm `user_profiles` exists.
- Confirm you did not import the wrong SQL dump into an empty database.
