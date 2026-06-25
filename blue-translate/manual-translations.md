# Manual Translations

BlueTranslate includes a manual translation system that lets you override automatic translations with curated, hand-written entries. This is useful for plugin-specific terminology, brand names, or any text where the automatic engine does not produce an acceptable result.

---

## Table of Contents

1. [Overview](#overview)
2. [File Structure](#file-structure)
3. [Entry Format](#entry-format)
4. [Placeholders Inside Entries](#placeholders-inside-entries)
5. [Inline Token Replacement](#inline-token-replacement)
6. [Managing Files with /bt manual](#managing-files-with-bt-manual)
7. [Reloading](#reloading)
8. [Fallback Behaviour](#fallback-behaviour)
9. [Examples](#examples)

---

## Overview

Manual translation files are stored in:

```
plugins/BlueTranslate/translations/
```

You can place any number of `.yml` files in this directory. Each file can contain as many translation entries as you like. On startup (and on `/bt reload`), all files are loaded and indexed.

An example file (`example.yml`) is automatically created the first time the plugin starts, so you have a ready-to-use template.

---

## File Structure

```
plugins/BlueTranslate/translations/
├── example.yml       # Provided example file
├── myplugin.yml      # Your custom translations
└── economy.yml       # Another file
```

Each `.yml` file is independent. Files are identified by their name without the `.yml` extension when using the `/bt manual` command.

---

## Entry Format

Each entry has a unique key and a set of language-specific strings plus a mandatory `default` value:

```yaml
<key>:
  default: "Exact text as sent by the server"
  <lang>: "Translation for that language"
  <lang>: "Translation for another language"
```

- **`default`** - The exact text the server sends to the player. Matching is **case-insensitive** and ignores surrounding whitespace.
- **`<lang>`** - ISO 639-1 language code (e.g. `es`, `fr`, `de`, `pt`). The value is what the player with that language will see.

> If no translation is defined for a player's language, the automatic engine is used as a fallback.

**Minimal example:**

```yaml
claim_reward:
  default: "Claim Reward"
  es: "Reclamar recompensa"
  fr: "Réclamer la récompense"
  de: "Belohnung beanspruchen"
```

---

## Placeholders Inside Entries

Placeholders (e.g. `%amount%`, `{player}`, `%vault_eco_balance%`) in the `default` text are preserved during matching and appear correctly in translated output.

```yaml
coins_received:
  default: "You received %amount% coins"
  es: "Has recibido %amount% monedas"
  fr: "Vous avez reçu %amount% pièces"
  de: "Du hast %amount% Münzen erhalten"
  pt: "Você recebeu %amount% moedas"
```

Colour codes and MiniMessage formatting are also supported:

```yaml
rank_up_message:
  default: "&aYou advanced to rank &e%rank%&a!"
  es: "&a¡Has subido al rango &e%rank%&a!"
  fr: "&aVous avez atteint le grade &e%rank%&a !"
  de: "&aDu bist auf Rang &e%rank%&a aufgestiegen!"
```

---

## Inline Token Replacement

When the `default` value of an entry is the same as the entry key (i.e. a bare token name), BlueTranslate can replace that token **anywhere** it appears inside longer strings - not just when it is the complete text.

Both forms are supported at runtime:

- `greeting_word`
- `{greeting_word}`

**Example:**

```yaml
greeting_word:
  default: "greeting_word"
  es: "Hola"
  fr: "Salut"
  de: "Hallo"
  pt: "Olá"
  en: "Hello"
```

If a plugin sends the message `"[greeting_word] Steve, welcome!"` the player will see `"Hello Steve, welcome!"` (or the appropriate greeting in their language).

---

## Managing Files with /bt manual

All manual translation management can be done in-game without editing files directly.

**Permission required:** `bluetranslate.manual` (default: `op`)

### List files

```
/bt manual files
```

### List keys in a file

```
/bt manual keys <file>
```

### Show all translations for a key

```
/bt manual show <file> <key>
```

### Set or update a translation value

```
/bt manual set <file> <key> <lang> <value...>
```

**Example:**
```
/bt manual set example claim_reward es Reclamar recompensa
/bt manual set economy coins_received fr Vous avez reçu %amount% pièces
```

### Remove a translation value

```
/bt manual remove <file> <key> [lang]
```

Remove a single language value:
```
/bt manual remove example claim_reward es
```

Remove the entire key (all language values and `default`):
```
/bt manual remove example claim_reward
```

### Create a new file

```
/bt manual createfile <name>
```

Creates `plugins/BlueTranslate/translations/<name>.yml`.

### Delete a file

```
/bt manual deletefile <name>
```

### Reload all files

```
/bt manual reload
```

---

## Reloading

Manual translation files are reloaded automatically after every `/bt manual set`, `/bt manual remove`, `/bt manual createfile`, and `/bt manual deletefile` command.

You can also reload everything (settings, language, menus, and translations) with:

```
/bt reload
```

---

## Fallback Behaviour

The lookup priority for any piece of text is:

1. **Manual translation** - if a matching `default` entry exists and a value is defined for the player's language, that value is used.
2. **Automatic engine** - if no manual translation is found, the text is sent to the configured translation engine.
3. **Original text** - if the engine is unavailable or returns an error, the original text is displayed unchanged.

---

## Examples

### Basic GUI Translations (economy.yml)

```yaml
claim_reward:
  default: "Claim Reward"
  es: "Reclamar recompensa"
  fr: "Réclamer la récompense"
  de: "Belohnung beanspruchen"
  pt: "Reivindicar recompensa"

insufficient_money:
  default: "You do not have enough money"
  es: "No tienes suficiente dinero"
  fr: "Vous n'avez pas assez d'argent"
  de: "Du hast nicht genug Geld"
  pt: "Você não tem dinheiro suficiente"

coins_received:
  default: "You received %amount% coins"
  es: "Has recibido %amount% monedas"
  fr: "Vous avez reçu %amount% pièces"
  de: "Du hast %amount% Münzen erhalten"
  pt: "Você recebeu %amount% moedas"
```

### Inventory/Menu Titles (menus.yml)

```yaml
menu_daily_rewards:
  default: "Daily Rewards"
  es: "Recompensas diarias"
  fr: "Récompenses quotidiennes"
  de: "Tägliche Belohnungen"
  pt: "Recompensas diárias"

menu_shop:
  default: "Shop"
  es: "Tienda"
  fr: "Boutique"
  de: "Shop"
  pt: "Loja"
```

### Quest Messages (quests.yml)

```yaml
quest_completed:
  default: "Quest completed!"
  es: "¡Misión completada!"
  fr: "Quête terminée !"
  de: "Quest abgeschlossen!"
  it: "Missione completata!"

quest_failed:
  default: "Quest failed"
  es: "Misión fallida"
  fr: "Quête échouée"
  de: "Quest fehlgeschlagen"
  it: "Missione fallita"
```

---

## Related Guides

- **[Commands & Permissions](./commands-and-permissions.md)** - Full `/bt manual` command reference.
- **[PlaceholderAPI](./placeholderapi.md)** - Access manual translation entries as PAPI placeholders.
- **[Configuration](./configuration.md)** - Placeholder protection settings.
