# Commands & Permissions

Complete reference of all commands and permission nodes in BlueTranslate.

**Note:** `[required]` - Required argument · `(optional)` - Optional argument

---

## Table of Contents

1. [Command Overview](#command-overview)
2. [Command Reference](#command-reference)
   - [/bt (base)](#bt-base)
   - [/bt help](#bt-help)
   - [/bt reload](#bt-reload)
   - [/bt list](#bt-list)
   - [/bt select](#bt-select)
   - [/bt disable](#bt-disable)
   - [/bt translate](#bt-translate)
   - [/bt manual](#bt-manual)
3. [Permission Nodes](#permission-nodes)
4. [Quick Reference Table](#quick-reference-table)

---

## Command Overview

BlueTranslate uses a single root command with subcommands:

- **`/bluetranslate`** (alias: **`/bt`**) - root command for all BlueTranslate features.

---

## Command Reference

### `/bt` (base)

**Permission:** `bluetranslate.info` (default: `true`)  
**Description:** Displays plugin information (version, engine status, loaded languages).

**Usage:**
```
/bt
```

---

### `/bt help`

**Permission:** `bluetranslate.help` (default: `true`)  
**Description:** Shows available commands.

**Usage:**
```
/bt help
```

---

### `/bt reload`

**Permission:** `bluetranslate.reload` (default: `op`)  
**Description:** Reloads all configuration files (`settings.yml`, `language.yml`, menu files) and manual translation files without restarting the server.

**Usage:**
```
/bt reload
```

---

### `/bt list`

**Permission:** `bluetranslate.list` (default: `true`)  
**Description:** Lists all languages supported by the currently active translation engine.

**Usage:**
```
/bt list
```

---

### `/bt select`

**Permission:** `bluetranslate.select` (default: `true`)  
**Permission (others):** `bluetranslate.select.others` (default: `op`)  
**Description:** Sets the preferred translation target language for a player.

Running `/bt select` with no arguments opens an interactive language selector GUI (players only).

**Usage:**
```
/bt select
/bt select [language_code]
/bt select [language_code] (player)
/bt select auto
/bt select auto (player)
```

**Arguments:**
- `[language_code]` - ISO 639-1 code of the target language (e.g. `es`, `fr`, `de`). See [Supported Languages](./supported-languages.md).
- `(player)` - Target player name. Requires `bluetranslate.select.others`.
- `auto` - Resets the language to automatic detection (client locale).

**Examples:**
```
/bt select              → Opens language selector GUI
/bt select es           → Set your language to Spanish
/bt select fr Steve     → Set Steve's language to French (admin)
/bt select auto         → Reset your language to auto-detection
/bt select auto Steve   → Reset Steve's language (admin)
```

---

### `/bt disable`

**Permission:** `bluetranslate.disable` (default: `true`)  
**Permission (others):** `bluetranslate.disable.others` (default: `op`)  
**Description:** Disables automatic translation for a player. The player will see all server text in its original language until they use `/bt select` again.

**Usage:**
```
/bt disable
/bt disable (player)
```

**Arguments:**
- `(player)` - Target player name. Requires `bluetranslate.disable.others`.

**Examples:**
```
/bt disable          → Disable translation for yourself
/bt disable Steve    → Disable translation for Steve (admin)
```

---

### `/bt translate`

**Permission:** `bluetranslate.translate` (default: `op`)  
**Description:** Utility command to test the active translation engine. Translates the provided text to the target language and prints the result in chat.

**Usage:**
```
/bt translate [target_lang] [text...]
```

**Arguments:**
- `[target_lang]` - ISO 639-1 language code to translate into.
- `[text...]` - The text to translate (can be multiple words).

**Examples:**
```
/bt translate es Hello world
/bt translate fr Good morning, have a great day!
/bt translate de This is a test
```

---

### `/bt manual`

**Permission:** `bluetranslate.manual` (default: `op`)  
**Description:** Manages manual translation files and keys from in-game. Full reference below.

**Usage:**
```
/bt manual files
/bt manual keys [file]
/bt manual show [file] [key]
/bt manual set [file] [key] [lang] [value...]
/bt manual remove [file] [key] [lang]
/bt manual createfile [file]
/bt manual deletefile [file]
/bt manual reload
```

**Subcommands:**

| Subcommand | Description |
|------------|-------------|
| `files` | List all loaded manual translation files. |
| `keys [file]` | List all translation keys in a file. |
| `show [file] [key]` | Display all language values for a key. |
| `set [file] [key] [lang] [value...]` | Set or update a translation value. |
| `remove [file] [key] [lang]` | Remove a translation value. Omit `lang` to remove the entire key (all language values and `default`). |
| `createfile [file]` | Create a new empty translation file. |
| `deletefile [file]` | Delete a translation file. |
| `reload` | Reload all manual translation files from disk. |

**Examples:**
```
/bt manual files
/bt manual keys example
/bt manual show example claim_reward
/bt manual set example claim_reward es Reclamar recompensa
/bt manual remove example claim_reward es
/bt manual remove example claim_reward
/bt manual createfile myplugin
/bt manual deletefile old_file
/bt manual reload
```

See [Manual Translations](./manual-translations.md) for the full guide.

---

## Permission Nodes

| Permission | Default | Description |
|------------|---------|-------------|
| `bluetranslate.*` | `op` | Grants all BlueTranslate permissions. |
| `bluetranslate.info` | `true` | Use `/bt` to view plugin info. |
| `bluetranslate.help` | `true` | Use `/bt help`. |
| `bluetranslate.reload` | `op` | Use `/bt reload`. |
| `bluetranslate.list` | `true` | Use `/bt list` to view supported languages. |
| `bluetranslate.select` | `true` | Use `/bt select` to change own language. |
| `bluetranslate.select.others` | `op` | Use `/bt select <lang> <player>` to change another player's language. |
| `bluetranslate.disable` | `true` | Use `/bt disable` for yourself. |
| `bluetranslate.disable.others` | `op` | Use `/bt disable <player>`. |
| `bluetranslate.translate` | `op` | Use `/bt translate` to run direct translation tests. |
| `bluetranslate.manual` | `op` | Use `/bt manual` to manage manual translation files. |
| `bluetranslate.admin` | `op` | Admin-level access to change other players' translation settings. |

---

## Quick Reference Table

| Command | Permission | Default |
|---------|-----------|---------|
| `/bt` | `bluetranslate.info` | Everyone |
| `/bt help` | `bluetranslate.help` | Everyone |
| `/bt reload` | `bluetranslate.reload` | OP |
| `/bt list` | `bluetranslate.list` | Everyone |
| `/bt select` | `bluetranslate.select` | Everyone |
| `/bt select <lang> <player>` | `bluetranslate.select.others` | OP |
| `/bt disable` | `bluetranslate.disable` | Everyone |
| `/bt disable <player>` | `bluetranslate.disable.others` | OP |
| `/bt translate <lang> <text>` | `bluetranslate.translate` | OP |
| `/bt manual ...` | `bluetranslate.manual` | OP |

---

## Related Guides

- **[First Steps](./first-steps.md)** - Installation and initial setup.
- **[Supported Languages](./supported-languages.md)** - All valid language codes.
- **[Manual Translations](./manual-translations.md)** - Managing manual translation files.
- **[Configuration](./configuration.md)** - Full `settings.yml` reference.
