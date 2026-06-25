# PlaceholderAPI

BlueTranslate provides a [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) expansion that exposes player language information and manual translation entries as placeholders usable in any PAPI-compatible plugin.

---

## Table of Contents

1. [Requirements](#requirements)
2. [Available Placeholders](#available-placeholders)
3. [Manual Translation Placeholders](#manual-translation-placeholders)
4. [Examples](#examples)

---

## Requirements

- **PlaceholderAPI** must be installed on your server.
- The BlueTranslate PAPI expansion is registered automatically when PlaceholderAPI is detected - no additional setup is needed.

---

## Available Placeholders

All placeholders use the identifier `bluetranslate`.

| Placeholder | Description |
|-------------|-------------|
| `%bluetranslate_language%` | The active language code for the player (e.g. `es`, `fr`, `en`). If the player has not chosen a language, this returns the code determined by their client locale. |
| `%bluetranslate_language_mode%` | Returns `"manual"` if the player has set a specific language with `/bt select`, or `"auto"` if the language is derived from their client locale. |
| `%bluetranslate_translation_disabled%` | Returns `"true"` if the player has disabled translation (via `/bt disable`), or `"false"` otherwise. |
| `%bluetranslate_default_language%` | The server-wide default language as configured in `settings.yml` (`translation.default_language`). This placeholder does **not** depend on a specific player. |

---

## Manual Translation Placeholders

BlueTranslate can expose individual manual translation entries as PAPI placeholders. This lets you use curated translations from your YAML files directly in scoreboards, menus, holograms, and anywhere PAPI is supported.

**Format:**

```
%bluetranslate_manual_<file>_<key>_<lang>%
```

| Segment | Description |
|---------|-------------|
| `<file>` | Name of the manual translation file **without** the `.yml` extension. |
| `<key>` | Translation key inside the file. |
| `<lang>` | ISO 639-1 language code of the desired translation. |

**Example:**

Given a file `economy.yml` containing:

```yaml
claim_reward:
  default: "Claim Reward"
  es: "Reclamar recompensa"
  fr: "Réclamer la récompense"
```

The placeholder `%bluetranslate_manual_economy_claim_reward_es%` returns `"Reclamar recompensa"`.

> **Note:** If the requested language is not defined in the entry, BlueTranslate automatically translates the `default` value using the active engine and returns the result.

### Parsing Logic

When both the file name and the key contain underscores, BlueTranslate uses the following algorithm to resolve ambiguity:

1. The last segment is treated as the **language code** (must match `[a-z]{2,3}`).
2. The remaining portion is matched against all loaded file names using a prefix search. The longest matching file name wins.
3. Anything after the file prefix is treated as the **key**.

This means that, as long as your file names do not overlap in a confusing way, the placeholders resolve correctly even with underscores in file and key names.

---

## Examples

### Scoreboard showing the player's current language

```
Language: %bluetranslate_language%
```

### Tab list footer showing whether translation is active

```
Translation: %bluetranslate_translation_disabled%
```

### Menu button label from a manual translation file

Given `menus.yml`:

```yaml
shop_button:
  default: "Open Shop"
  es: "Abrir Tienda"
  fr: "Ouvrir la Boutique"
  de: "Shop öffnen"
```

In your menu configuration:
```
display_name: "%bluetranslate_manual_menus_shop_button_es%"
```

### Conditional display based on language mode

With a plugin that supports PAPI conditions (e.g. HolographicDisplays, CMI):

```
{condition: %bluetranslate_language_mode% == manual} Language set manually
{condition: %bluetranslate_language_mode% == auto}   Language auto-detected
```

---

## Related Guides

- **[Manual Translations](./manual-translations.md)** - Create and manage manual translation files.
- **[Commands & Permissions](./commands-and-permissions.md)** - `/bt select` and `/bt disable` commands.
- **[Configuration](./configuration.md)** - `default_language` setting.
