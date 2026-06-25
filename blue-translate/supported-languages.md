# Supported Languages

This page lists all languages currently supported by the **official Blueva LibreTranslate instance** at [`https://translate.blueva.net`](https://translate.blueva.net), as well as information about the REST API exposed by that instance.

> If you self-host LibreTranslate or use a different translation engine, the available languages will depend on that engine's configuration. Run `/bt list` in-game to see the languages reported by the currently active engine.

---

## Table of Contents

1. [Supported Languages](#supported-languages-list)
2. [Using Language Codes](#using-language-codes)
3. [BlueTranslate API (Blueva Instance)](#bluetranslate-api-blueva-instance)
   - [GET /languages](#get-languages)
   - [POST /translate](#post-translate)
   - [POST /detect](#post-detect)

---

## Supported Languages List

| Code | Language |
|------|----------|
| `en` | English |
| `sq` | Albanian |
| `ar` | Arabic |
| `az` | Azerbaijani |
| `eu` | Basque |
| `bn` | Bengali |
| `bg` | Bulgarian |
| `ca` | Catalan |
| `zh-Hans` | Chinese (Simplified) |
| `zh-Hant` | Chinese (Traditional) |
| `cs` | Czech |
| `da` | Danish |
| `nl` | Dutch |
| `eo` | Esperanto |
| `et` | Estonian |
| `fi` | Finnish |
| `fr` | French |
| `gl` | Galician |
| `de` | German |
| `el` | Greek |
| `he` | Hebrew |
| `hi` | Hindi |
| `hu` | Hungarian |
| `id` | Indonesian |
| `ga` | Irish |
| `it` | Italian |
| `ja` | Japanese |
| `ko` | Korean |
| `ky` | Kyrgyz |
| `lv` | Latvian |
| `lt` | Lithuanian |
| `ms` | Malay |
| `nb` | Norwegian |
| `fa` | Persian |
| `pl` | Polish |
| `pt` | Portuguese |
| `pt-BR` | Portuguese (Brazil) |
| `ro` | Romanian |
| `ru` | Russian |
| `sk` | Slovak |
| `sl` | Slovenian |
| `es` | Spanish |
| `sv` | Swedish |
| `tl` | Tagalog |
| `th` | Thai |
| `tr` | Turkish |
| `uk` | Ukrainian |
| `ur` | Urdu |
| `vi` | Vietnamese |

**Total: 50 languages**

---

## Using Language Codes

Language codes follow the [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) standard (two letters) with the exception of `zh-Hans`, `zh-Hant`, and `pt-BR`, which use extended tags.

Use these codes anywhere BlueTranslate expects a language code:

```
/bt select es          → Spanish
/bt select zh-Hans     → Chinese (Simplified)
/bt select pt-BR       → Portuguese (Brazil)
/bt translate fr Good morning!
```

In `settings.yml`:
```yaml
translation:
  default_language: "en"
  source_language: "auto"
```

In manual translation files:
```yaml
my_entry:
  default: "Hello"
  es: "Hola"
  fr: "Bonjour"
  zh-Hans: "你好"
  pt-BR: "Olá"
```

---

## BlueTranslate API (Blueva Instance)

The Blueva-hosted LibreTranslate instance is publicly accessible at **`https://translate.blueva.net`**.  
It uses the standard LibreTranslate REST API. No API key is required for the public endpoints.

> **Note:** This API is provided as a free service. Please use it responsibly. Excessive automated requests from outside BlueTranslate may be rate-limited.

---

### GET /languages

Returns the list of all supported languages.

**Request:**

```http
GET https://translate.blueva.net/languages
```

**Response:**

```json
[
  { "code": "en", "name": "English" },
  { "code": "es", "name": "Spanish" },
  { "code": "fr", "name": "French" },
  ...
]
```

**Example (curl):**

```bash
curl https://translate.blueva.net/languages
```

---

### POST /translate

Translates one or more texts from a source language to a target language.

**Request:**

```http
POST https://translate.blueva.net/translate
Content-Type: application/json
```

**Body:**

```json
{
  "q": "Hello, world!",
  "source": "en",
  "target": "es",
  "format": "text",
  "api_key": ""
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `q` | Yes | Text (or array of texts) to translate. |
| `source` | Yes | Source language code, or `"auto"` for automatic detection. |
| `target` | Yes | Target language code. |
| `format` | No | `"text"` (default) or `"html"`. Use `"html"` to prevent the engine from mangling HTML/XML content. |
| `api_key` | No | Leave blank for the Blueva public instance. |

**Response:**

```json
{
  "translatedText": "¡Hola, mundo!"
}
```

**Batch translation (array input):**

```json
{
  "q": ["Hello", "Goodbye", "Thank you"],
  "source": "en",
  "target": "de",
  "api_key": ""
}
```

```json
{
  "translatedText": ["Hallo", "Auf Wiedersehen", "Danke"]
}
```

**Example (curl):**

```bash
curl -X POST https://translate.blueva.net/translate \
  -H "Content-Type: application/json" \
  -d '{"q":"Hello world","source":"en","target":"fr","api_key":""}'
```

---

### POST /detect

Detects the language of a given text.

**Request:**

```http
POST https://translate.blueva.net/detect
Content-Type: application/json
```

**Body:**

```json
{
  "q": "Bonjour tout le monde",
  "api_key": ""
}
```

**Response:**

```json
[
  { "confidence": 0.98, "language": "fr" }
]
```

Returns an array of candidates sorted by confidence (highest first).

**Example (curl):**

```bash
curl -X POST https://translate.blueva.net/detect \
  -H "Content-Type: application/json" \
  -d '{"q":"Bonjour tout le monde","api_key":""}'
```

---

## Related Guides

- **[Translation Engines](./translation-engines.md)** - Configure the active translation engine.
- **[Configuration](./configuration.md)** - `source_language` and `default_language` settings.
- **[Commands & Permissions](./commands-and-permissions.md)** - `/bt select` and `/bt list` commands.
