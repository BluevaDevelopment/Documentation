# Discretion Module

**File:** `plugins/BlueSpoof/modules/discretion.yml`

**Status:** Optional. Enable by setting `discretion.enabled: true`.

---

## What It Does

The Discretion module hides BlueSpoof branding from public-facing areas of your server. This is useful for networks that prefer not to advertise which plugins they are running.

When enabled, the plugin can:
- Replace "BlueSpoof" with a custom name in console output
- Register alternative command labels that do not expose the plugin name
- Register alternative PlaceholderAPI identifiers

---

## Console Name

| Option | Description | Default |
|--------|-------------|---------|
| `discretion.console_name` | Name shown in console instead of "BlueSpoof" | `NetworkBridge` |

All console messages that normally contain "BlueSpoof" will use this custom name instead.

---

## Placeholder Identifiers

| Option | Description | Default |
|--------|-------------|---------|
| `discretion.placeholders.identifiers` | List of custom PAPI identifiers | `[networkbridge]` |

When discretion mode is enabled, these identifiers are registered in addition to the default `bluespoof`. This allows you to use placeholders like `%networkbridge_server_total%` in public configurations without revealing the plugin name.

**Note:** The default `%bluespoof_*%` placeholders always remain registered for backward compatibility, even when discretion is enabled.

---

## Hidden Commands

| Option | Description | Default |
|--------|-------------|---------|
| `discretion.command_labels` | Alternative command labels handled by event listeners | `[networkbridge, nb, nbridge]` |

These labels work as aliases for `/bluespoof` but are handled through event listeners rather than being registered as formal commands. This means:
- They are not visible in the plugin.yml command list
- They do not appear in most plugin enumeration tools
- They are not tab-completable unless the formal command is also registered

### Authorized UUIDs

| Option | Description | Default |
|--------|-------------|---------|
| `discretion.authorized_uuids` | List of player UUIDs allowed to use hidden commands | `[]` |

When discretion mode is enabled, only players whose UUID is on this list can execute the hidden command labels. This adds an extra layer of access control.

---

## Full Example Configuration

```yaml
discretion:
  enabled: false
  console_name: NetworkBridge
  placeholders:
    identifiers:
      - networkbridge
  authorized_uuids: []
  command_labels:
    - networkbridge
    - nb
    - nbridge
```

---

## Notes

- Discretion mode does not rename the plugin JAR file or its internal classes. It only affects visible output and command exposure.
- The `/bs` command and its aliases (`bluespoof`, `bspoof`) still work regardless of discretion mode. Hidden commands are additional aliases, not replacements.
- If you want completely invisible operation, combine Discretion with careful permission management so regular players cannot use `/bs`.
- Some server monitoring tools may still detect the plugin through packet signatures or class names. Discretion mode is not an anti-cheat or anti-detection system.
