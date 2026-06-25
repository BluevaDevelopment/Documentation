# Configuration

Complete reference for `plugins/BlueTranslate/settings.yml`.

---

## Table of Contents

1. [Metrics](#metrics)
2. [Updates](#updates)
3. [Translation: General](#translation-general)
4. [Translation Engine](#translation-engine)
5. [Translation Cache](#translation-cache)
6. [Batch Translation](#batch-translation)
7. [Packet Filtering](#packet-filtering)
8. [Placeholder & Token Protection](#placeholder--token-protection)
9. [Packet Coverage](#packet-coverage)
10. [Chat Behaviour](#chat-behaviour)
11. [Language Detection](#language-detection)

---

## Metrics

```yaml
metrics: true
```

BlueTranslate uses [bStats](https://bstats.org) to collect anonymous usage data. The following information is collected:

- Server online/offline mode
- Plugin version
- Server version, Java version, OS
- Server location (country)
- CPU core count
- Number of online players

Set to `false` to opt out.

---

## Updates

```yaml
check_for_updates: true
```

When enabled, BlueTranslate checks for a newer version on startup and notifies operators in-game. It is recommended to always run the latest version.

---

## Translation: General

```yaml
translation:
  source_language: "auto"
  default_language: "en"
```

| Key | Default | Description |
|-----|---------|-------------|
| `source_language` | `"auto"` | Source language of the text being translated. Use `"auto"` to let the engine detect it automatically, or set a fixed ISO 639-1 code (e.g. `"en"`). |
| `default_language` | `"en"` | Fallback language used for players whose locale cannot be detected. |

---

## Translation Engine

```yaml
translation:
  engine:
    type: "libretranslate"
```

Selects which engine processes translations. Available values:

| Value | Engine |
|-------|--------|
| `libretranslate` | LibreTranslate (free, default - Blueva public instance) |
| `google` | Google Cloud Translation API v2 |
| `deepl` | DeepL API (Free or Pro) |
| `azure` / `microsoft` | Microsoft Azure Translator |
| `openai` | OpenAI Chat Completions |
| `amazon` | Amazon Translate |

For detailed setup instructions for each engine, see [Translation Engines](./translation-engines.md).

---

## Translation Cache

```yaml
translation:
  cache:
    max_size: 1000
    ttl_minutes: 60
    persist_to_db: true
    db_max_load: 5000
```

BlueTranslate caches translation results in memory to avoid redundant API calls and keep the packet pipeline non-blocking. Translations are fetched asynchronously on first encounter and served from cache on every subsequent occurrence.

| Key | Default | Description |
|-----|---------|-------------|
| `max_size` | `1000` | Maximum number of translations held in memory. Oldest/least-recently-used entries are evicted when the limit is reached. |
| `ttl_minutes` | `60` | How many minutes a cached translation stays valid before being refreshed. Set to `0` to keep entries indefinitely. |
| `persist_to_db` | `true` | Persist cache entries to an SQLite database so they survive server restarts. On startup, the database is read back into memory, providing a warm cache from the first second. |
| `db_max_load` | `5000` | Maximum number of cache entries loaded from the database on startup. Lower this value on servers with limited heap space. |

---

## Batch Translation

```yaml
translation:
  batch:
    enabled: true
    max_size: 10
    interval_ms: 100
```

When enabled, cache-miss translation requests are queued and dispatched to the engine in batches. This significantly reduces the number of HTTP round-trips when many unique strings appear in a short time window (e.g. on player join, GUI open, or tab-list refresh).

| Key | Default | Description |
|-----|---------|-------------|
| `enabled` | `true` | Enable batch translation. |
| `max_size` | `10` | Maximum number of texts sent to the engine in a single API request. Reduce this if you hit request-size limits on your translation endpoint. |
| `interval_ms` | `100` | Milliseconds to wait before flushing the current batch. Lower values reduce latency at the cost of smaller (less efficient) batches. Values below 50 ms result in the batch running every server tick. |

---

## Packet Filtering

```yaml
translation:
  filter:
    min_length: 2
    max_length: 2000
    detection_cache_max_size: 500
```

Fine-grained controls to skip translation for text that is unlikely to benefit from it, reducing CPU, memory and API usage.

| Key | Default | Description |
|-----|---------|-------------|
| `min_length` | `2` | Minimum character count required for a string to enter the translation pipeline. Very short strings (single characters, punctuation) are ignored. |
| `max_length` | `2000` | Maximum character count allowed. Strings longer than this are passed through untouched to avoid excessive API latency and payload size rejections. |
| `detection_cache_max_size` | `500` | Maximum entries in the in-memory language-detection cache. Each detected language result is stored here (`text → ISO 639-1 code`) so subsequent occurrences skip the `/detect` API call. Uses LRU eviction. |

---

## Placeholder & Token Protection

```yaml
translation:
  placeholder_protection:
    ignore_words: []
    regex_patterns:
      - "https?://\\S+"
      - "www\\.\\S+"
      - "</?[a-zA-Z#][^>\\n]{0,200}>"
      - "§x(?:§[0-9a-fA-F]){6}"
      - "&#[0-9a-fA-F]{6}"
      - "%[a-zA-Z0-9_]+%"
      - "\\{\\d+}"
      - "\\{[a-zA-Z0-9_]+}"
      - "[&§][0-9a-fA-Fk-oK-OrR]"
      - "&(?:[a-zA-Z]+|#\\d+|#x[0-9a-fA-F]+);"
      - "[\\p{So}\\p{Sm}\\p{Sk}&&[^\\x00-\\x7F]]+"
      - "\\b\\d+(?:[.,]\\d+)*\\b"
```

Controls which text fragments are shielded from translation before requests are sent to the engine. Protected fragments are restored exactly as they originally appeared after translation.

| Key | Description |
|-----|-------------|
| `ignore_words` | List of literal words or tokens that must never be translated. Useful for product names, server brands, game modes, or commands. Matching is case-sensitive. |
| `regex_patterns` | Regex patterns used to detect fragments that should be protected (URLs, MiniMessage tags, colour codes, PlaceholderAPI tokens, numeric values, etc.). The built-in defaults cover the most common cases. Invalid or empty entries are silently ignored. |

**Example - protecting a server name:**
```yaml
placeholder_protection:
  ignore_words:
    - "Blueva"
    - "HyPixel"
    - "SkyBlock"
```

---

## Packet Coverage

```yaml
translation:
  packets:
    chat_messages: true
    action_bar: true
    title: true
    subtitle: true
    boss_bar: true
    tab_list_header_footer: true
    scoreboard_objective: true
    scoreboard_team_prefix_suffix: true
    scoreboard_score_entries: true
    inventory_menu_title: true
    inventory_single_item: true
    inventory_all_items: true
    entity_metadata: true
    block_entity_data: true
```

Enable or disable translation for each individual packet channel. Set a channel to `false` to skip it entirely.

| Channel | Description |
|---------|-------------|
| `chat_messages` | Player-visible chat and system messages. |
| `action_bar` | Action bar text (above the hotbar). |
| `title` | Title text displayed on screen. |
| `subtitle` | Subtitle text displayed on screen. |
| `boss_bar` | Boss bar name. |
| `tab_list_header_footer` | Tab list header and footer. |
| `scoreboard_objective` | Scoreboard objective display name. |
| `scoreboard_team_prefix_suffix` | Scoreboard team prefixes and suffixes. |
| `scoreboard_score_entries` | Individual score entry names. |
| `inventory_menu_title` | Title of open inventory/GUI menus. |
| `inventory_single_item` | Single item name and lore updates. |
| `inventory_all_items` | All items in an inventory window. |
| `entity_metadata` | Entity custom names (name tags, etc.). |
| `block_entity_data` | Block entity text (signs, etc.). |

> **Tip:** If a specific GUI or scoreboard breaks after a plugin update, try disabling only that channel while keeping all others active.

---

## Chat Behaviour

```yaml
translation:
  chat:
    mode: "delayed"
    timeout_ms: 800
```

Controls how `SYSTEM_CHAT_MESSAGE` packets are handled when a translation is not yet cached.

| Key | Default | Description |
|-----|---------|-------------|
| `mode` | `"delayed"` | `delayed` - Hold the original packet, wait for translation, then deliver one ordered message. If translation does not finish within `timeout_ms`, the best available result is sent immediately. `immediate` - Send the original packet right away, then send a translated follow-up when the async translation completes. |
| `timeout_ms` | `800` | Maximum milliseconds to wait in `delayed` mode before sending the available result. Only used when `mode` is `delayed`. |

---

## Language Detection

```yaml
translation:
  language_detection:
    enabled: true
```

When enabled, BlueTranslate asks the translation engine to detect the source language of incoming text. If the detected language already matches the player's target language, the text is returned as-is - no translation API call is made - saving resources and avoiding unnecessary API traffic. Detection results are cached in memory.

| Key | Default | Description |
|-----|---------|-------------|
| `enabled` | `true` | Enable automatic language detection. When disabled, the translation endpoint is always called for every cache miss. |

---

## Related Guides

- **[Translation Engines](./translation-engines.md)** - How to configure each engine.
- **[Manual Translations](./manual-translations.md)** - Override automatic translations.
- **[Commands & Permissions](./commands-and-permissions.md)** - Command reference.
