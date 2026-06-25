# Multi-Proxy & Floodgate

This page covers two advanced network setups: several proxy instances sharing
authentication state, and Bedrock players joining through Floodgate/GeyserMC.

## Multi-Proxy Requirements

Use multi-proxy support when more than one BungeeCord/Velocity proxy can accept
players for the same backend network.

Required:

- MySQL or MariaDB shared by every proxy.
- The same Blue Premium version on every proxy.
- The same `access_token` on every proxy/backend pair.
- `multi_proxy.enabled: true` on every proxy.
- A unique `multi_proxy.proxy_id` per proxy.

Do not use SQLite for multi-proxy networks.

## Multi-Proxy Configuration

On every proxy:

```yaml
storage:
  type: MYSQL
  host: '127.0.0.1'
  port: 3306
  database: 'bluepremium'
  user: 'bluepremium'
  password: 'strong-password'

multi_proxy:
  enabled: true
  proxy_id: 'proxy-1'
```

Use a different `proxy_id` for each process:

```text
proxy-1
proxy-2
proxy-eu
proxy-us
```

Blue Premium creates and uses an `active_sessions` table. When a player logs in
on one proxy, other proxies can recognize that active session through the shared
database.

## Multi-Proxy Behavior

| Action | Behavior |
|--------|----------|
| Login | Writes an active session row for the player's UUID |
| Clean logout | Removes the row owned by that proxy |
| Reconnect through another proxy | Reads the shared session row |
| Stale proxy/session | Session expires after no heartbeat is seen |
| `/forcedestroysession` | Removes the session and propagates logout state |
| `/forceunregister` | Clears profile/session state for the target |

The built-in coordinator is database-backed. It is designed for small and
medium networks that want multi-proxy session continuity without extra
infrastructure.

## Multi-Proxy Checklist

Before production:

1. Start two proxies connected to the same database.
2. Log in through proxy A.
3. Disconnect.
4. Reconnect through proxy B.
5. Confirm the player does not need to repeat login while the session is valid.
6. Run `/forcedestroysession <player>` and confirm the session is removed.
7. Stop proxy A and confirm proxy B continues to authenticate normally.

## Floodgate Support

Floodgate allows Bedrock players to join Java networks through GeyserMC.
Bedrock players authenticate through Xbox Live, so Blue Premium should not ask
them to register a cracked password.

Proxy config:

```yaml
auth:
  floodgate_support: true
  floodgate_bedrock_prefix: '.'
```

| Key | Meaning |
|-----|---------|
| `floodgate_support` | Enables Floodgate/Bedrock bypass detection |
| `floodgate_bedrock_prefix` | Prefix used by Floodgate usernames, default `.` |

## How Bedrock Detection Works

Blue Premium detects Bedrock players using:

1. Floodgate API when it is installed and reachable.
2. Floodgate UUID-space heuristic.
3. Configured username prefix during early pre-login stages.

If your Floodgate installation uses a custom prefix, set the same prefix in
`auth.floodgate_bedrock_prefix`.

Set the prefix to an empty string only if you want to disable username-prefix
detection:

```yaml
auth:
  floodgate_bedrock_prefix: ''
```

The API and UUID heuristic can still detect Bedrock players later in the login
flow when available.

## Floodgate Troubleshooting

**Bedrock players are asked to register**

- Confirm Floodgate is installed on the proxy.
- Confirm `auth.floodgate_support: true`.
- Confirm `auth.floodgate_bedrock_prefix` matches your Floodgate config.
- Check the proxy console for the Blue Premium Floodgate detection message.

**Java players with a prefix bypass auth**

- Do not allow normal Java players to use the Floodgate prefix.
- Pick a prefix that cannot overlap your Java naming policy.
- Keep `security.allowed_nickname_pattern` strict.

**Bedrock prefix changed after migration**

- Update `auth.floodgate_bedrock_prefix`.
- Restart the proxy.
- Test with one Bedrock account before production.
