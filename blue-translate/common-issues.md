# Common Issues & Troubleshooting

Solutions to frequently encountered problems with BlueTranslate.

---

## Table of Contents

1. [Plugin Not Loading](#plugin-not-loading)
2. [Commands Not Working](#commands-not-working)
3. [Translation Not Working](#translation-not-working)
4. [Translation Engine Errors](#translation-engine-errors)
5. [Specific Channels Not Translating](#specific-channels-not-translating)
6. [Placeholders / Formatting Broken After Translation](#placeholders--formatting-broken-after-translation)
7. [Manual Translations Not Applying](#manual-translations-not-applying)
8. [Performance Issues](#performance-issues)
9. [General Troubleshooting](#general-troubleshooting)

---

## Plugin Not Loading

### Symptoms
- BlueTranslate does not appear in `/plugins`.
- Console shows an error during startup.

### Solutions

**1. Check that PacketEvents is installed.**

BlueTranslate requires [PacketEvents](https://www.spigotmc.org/resources/packetevents-api.80279/) to be present in your `plugins/` folder and loaded **before** BlueTranslate.

```
plugins/
├── packetevents-*.jar    ← Required
└── BlueTranslate.jar
```

**2. Verify your server version.**

BlueTranslate requires a Spigot/Paper fork running **Minecraft 1.21 or higher**. Bukkit is not supported.

**3. Verify your Java version.**

Run `java -version` in your server console. Java 17 or higher is required.

**4. Check the console for errors.**

Look for lines beginning with `[BlueTranslate]` or `[ERROR]` near the startup section.

---

## Commands Not Working

### Symptoms
- `/bt` or `/bluetranslate` returns "Unknown command".
- No response from commands.

### Solutions

**1. Confirm the plugin loaded successfully.**

Run `/plugins` and check that `BlueTranslate` appears in green.

**2. Try the full command alias.**

```
/bluetranslate
```

If another plugin has registered `/bt`, there may be a conflict. Use the full command name or configure a command alias in your server's `commands.yml`.

**3. Check your permissions.**

Most player-facing commands require `bluetranslate.select`, `bluetranslate.disable`, etc. Make sure your permission group has the correct nodes. See [Commands & Permissions](./commands-and-permissions.md).

**4. Reload the plugin.**

```
/bt reload
```

Or perform a full server restart.

---

## Translation Not Working

### Symptoms
- Players see the original server text in all languages.
- `/bt select es` succeeds but text is still in the original language.

### Solutions

**1. Check that the translation engine is reachable.**

Run:

```
/bt translate es Hello world
```

If you see an error or "pending" message instead of a translated result, the engine is unavailable. Check your `settings.yml` engine configuration and network connectivity.

**2. Verify the player has a language set.**

Run `/bt list` to confirm languages are loaded. Ask the player to run `/bt select es` (replacing `es` with their desired language).

**3. Check the packet channel is enabled.**

Open `settings.yml` and verify that the relevant packet channel is `true` under `translation.packets`. For example, if chat is not translating:

```yaml
translation:
  packets:
    chat_messages: true
```

**4. Check the filter settings.**

Text shorter than `translation.filter.min_length` or longer than `translation.filter.max_length` is skipped. Make sure the texts you expect to be translated fall within the configured range.

**5. Language detection skipping translation.**

If `translation.language_detection.enabled` is `true` and the source text is already in the player's target language, BlueTranslate will skip the translation. This is expected behaviour.

---

## Translation Engine Errors

### Symptoms
- Console logs errors from the translation engine.
- `/bt translate` returns errors or pending messages.

### Solutions

**LibreTranslate (default):**
- Verify the URL is reachable: `curl https://translate.blueva.net/languages`
- If using a self-hosted instance, ensure the service is running and accessible from the server.

**Google / DeepL / Azure / OpenAI / Amazon:**
- Verify the API key is correct and has not expired.
- Check that your account has billing enabled and the API is activated.
- Increase `timeout_seconds` if you are on a slow network.

**All engines:**
- Increase `translation.filter.max_length` if you suspect very long strings are being rejected.
- Check the server console for HTTP status codes in error messages (e.g. `401 Unauthorized`, `429 Too Many Requests`).

---

## Specific Channels Not Translating

### Symptoms
- Chat translates correctly but scoreboards, boss bars, or inventory titles do not.

### Solutions

**1. Enable the channel in `settings.yml`:**

```yaml
translation:
  packets:
    boss_bar: true
    scoreboard_objective: true
    inventory_menu_title: true
```

**2. If enabling the channel causes issues (broken UI), disable only that channel:**

Sometimes a plugin's GUI relies on non-standard formatting that breaks after translation. Disable only the problematic channel:

```yaml
translation:
  packets:
    inventory_menu_title: false    # Disable only this one
```

**3. Reload after changing settings:**

```
/bt reload
```

---

## Placeholders / Formatting Broken After Translation

### Symptoms
- PlaceholderAPI tokens (e.g. `%player_name%`) are translated or garbled.
- Colour codes (e.g. `&a`, `&#FF5500`) are broken after translation.
- URLs appear modified.

### Solutions

**1. Verify the regex protection patterns are correct.**

Open `settings.yml` and check `translation.placeholder_protection.regex_patterns`. The default patterns cover the most common cases (`%placeholder%`, `{placeholder}`, `&colour`, MiniMessage tags, URLs, etc.).

If you have custom placeholder formats, add a regex pattern for them:

```yaml
placeholder_protection:
  regex_patterns:
    - "%[a-zA-Z0-9_]+%"          # PAPI style
    - "\\{[a-zA-Z0-9_]+}"        # {placeholder} style
    - "\\[my_custom_token\\]"    # Custom format
```

**2. Protect specific words or tokens.**

Use `ignore_words` to protect literal strings:

```yaml
placeholder_protection:
  ignore_words:
    - "MyServer"
    - "SkyBlock"
```

**3. Reload after changes.**

```
/bt reload
```

---

## Manual Translations Not Applying

### Symptoms
- You added entries to a YAML file in `translations/` but the automatic translation is still used.

### Solutions

**1. Reload the translations.**

```
/bt reload
```

or

```
/bt manual reload
```

**2. Verify the `default` value matches exactly.**

The `default` field is compared against the text the server sends - case-insensitively and with leading/trailing whitespace ignored, but otherwise exactly. Copy the exact text from a plugin's source or config file.

**3. Check the file is in the correct folder.**

Files must be placed in `plugins/BlueTranslate/translations/` with a `.yml` extension.

**4. Validate the YAML syntax.**

Indentation errors or missing quotes can cause the file to load incorrectly. Use a YAML validator (e.g. [https://www.yamllint.com/](https://www.yamllint.com/)) to check your file.

**5. List loaded files.**

```
/bt manual files
```

If your file is not listed, it was not loaded. Check the console for YAML parse errors.

---

## Performance Issues

### Symptoms
- Server TPS drops when players join or open GUIs.
- High API request volume.

### Solutions

**1. Enable and tune batch translation.**

```yaml
translation:
  batch:
    enabled: true
    max_size: 10
    interval_ms: 100
```

**2. Enable cache persistence.**

```yaml
translation:
  cache:
    persist_to_db: true
    db_max_load: 5000
```

This warms the cache from disk on startup, so common strings are served instantly.

**3. Increase `min_length` to skip trivial strings.**

```yaml
translation:
  filter:
    min_length: 4
```

**4. Disable packet channels you don't need.**

```yaml
translation:
  packets:
    entity_metadata: false     # Skip entity name translation if not needed
    block_entity_data: false   # Skip sign translation if not needed
```

**5. Switch to a faster engine.**

LibreTranslate (especially a self-hosted instance) is typically the fastest option. Google and Azure also have low latency. OpenAI is the slowest due to chat-completions overhead.

---

## General Troubleshooting

### Check the plugin version

```
/bt
```

Always run the latest version for the best experience.

### Check the console

Most issues produce descriptive error messages in the server console prefixed with `[BlueTranslate]`.

### Reload the plugin

```
/bt reload
```

### Check for command conflicts

If `/bt` is claimed by another plugin, use `/bluetranslate` or configure an alias in `commands.yml`.

### Contact Support

If the issue persists, visit [https://blueva.net](https://blueva.net) and provide:

- Plugin version (`/bt`)
- Server version
- Console error messages (full stack trace if available)
- Steps to reproduce the issue

---

## Quick Reference - Common Error Messages

| Error / Symptom | Likely Cause | Solution |
|----------------|--------------|----------|
| Engine unavailable | Network issue or wrong URL/key | Check engine config and connectivity |
| Text not translated | Channel disabled or filter skipping it | Check `packets` and `filter` settings |
| Placeholders garbled | Missing regex pattern | Add pattern to `regex_patterns` |
| Manual entry not applied | YAML error or wrong `default` value | Validate YAML, check `default` text |
| Plugin not loading | Missing PacketEvents | Install PacketEvents |
| `/bt` unknown command | Plugin not loaded or command conflict | Check `/plugins`, use `/bluetranslate` |

---

## Related Guides

- **[First Steps](./first-steps.md)** - Installation and setup.
- **[Configuration](./configuration.md)** - Full `settings.yml` reference.
- **[Translation Engines](./translation-engines.md)** - Engine-specific setup and troubleshooting.
- **[Manual Translations](./manual-translations.md)** - Manual translation system.
- **[Commands & Permissions](./commands-and-permissions.md)** - Command and permission reference.
