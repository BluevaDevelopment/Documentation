# PlaceholderAPI Placeholders

BlueSpoof provides PlaceholderAPI integration for use in scoreboards, holograms, TAB, and other plugins.

**Requirement:** [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) must be installed.

**Expansion:** `bluespoof`

---

## Server Placeholders

| Placeholder | Description |
|-------------|-------------|
| `%bluespoof_server_total%` | Total online players (real + fake) |
| `%bluespoof_server_fake%` | Number of fake players online |
| `%bluespoof_server_real%` | Number of real players online |

---

## Proxy Placeholders

These require BlueSpoof on the proxy and participating backends, with synchronization enabled and the same method selected on both sides. Fresh configurations use the default `plugin` bridge automatically. See [Proxy Sync](proxy.md) for setup and migration.

| Placeholder | Description |
|-------------|-------------|
| `%bluespoof_proxy_total%` | Total players across all backend servers (real + fake) |
| `%bluespoof_proxy_visible_total%` | Visible proxy player count including counter spoofing |
| `%bluespoof_proxy_fake%` | Total fake players across all backend servers |
| `%bluespoof_proxy_real%` | Total real players across all backend servers |
| `%bluespoof_proxy_{server_name}_fake%` | Fake players on a specific backend server |
| `%bluespoof_proxy_{server_name}_real%` | Real players on a specific backend server |
| `%bluespoof_proxy_{server_name}_total%` | Total players on a specific backend server |

**Server names:** With the default `plugin` method, replace `{server_name}` with the backend name from the BungeeCord/Velocity server list. It is detected automatically; the backend `server_id` is ignored. With TCP/Redis/SQL, use `proxy.messaging.server_id`, which must match that proxy server-list name.

---

## Player Check Placeholder

| Placeholder | Description |
|-------------|-------------|
| `%bluespoof_check_player_{name}%` | Check if a player is fake - returns `fake`, `real`, or `null` if offline |

---

## Usage Examples

### Scoreboard

```yaml
lines:
  - "&7Online: &a%bluespoof_server_total%"
  - "&7Real: &f%bluespoof_server_real%"
  - "&7Fake: &c%bluespoof_server_fake%"
```

### Hologram (using DecentHolograms or similar)

```yaml
lines:
  - "&6&lServer Status"
  - "&7Players: &a%bluespoof_server_total%"
  - "&7Real: &f%bluespoof_server_real%"
```

### Conditional formatting (using ConditionalEvents or similar)

```yaml
conditions:
  - "%bluespoof_check_player_%player_name%% equals fake"
```

---

## Discretion Mode

When Discretion mode is enabled, additional placeholder identifiers can be registered (for example `networkbridge`). All placeholders remain available under `%bluespoof_*%` for backward compatibility.
