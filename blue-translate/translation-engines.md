# Translation Engines

BlueTranslate supports multiple translation engines. The active engine is configured in `settings.yml` under `translation.engine.type`.

---

## Table of Contents

1. [LibreTranslate (Default)](#libretranslate-default)
2. [Google Cloud Translate](#google-cloud-translate)
3. [DeepL](#deepl)
4. [Microsoft Azure Translator](#microsoft-azure-translator)
5. [OpenAI](#openai)
6. [Amazon Translate](#amazon-translate)
7. [Switching Engines](#switching-engines)
8. [Comparison](#comparison)

---

## LibreTranslate (Default)

**Engine type value:** `libretranslate`

LibreTranslate is a free, open-source machine translation engine. BlueTranslate uses the **Blueva-hosted public instance** (`https://translate.blueva.net`) by default - no account or API key is required.

For a full list of supported languages on the official Blueva instance, see [Supported Languages](./supported-languages.md).

### Configuration

```yaml
translation:
  engine:
    type: "libretranslate"

    libretranslate:
      url: "https://translate.blueva.net"
      api_key: ""
      timeout_seconds: 10
```

| Key | Default | Description |
|-----|---------|-------------|
| `url` | `https://translate.blueva.net` | REST API base URL. No trailing slash. Change this if you self-host LibreTranslate. |
| `api_key` | `""` | API key. Leave blank when using the Blueva public instance or a self-hosted server that does not require authentication. |
| `timeout_seconds` | `10` | Maximum seconds to wait for a response before timing out. |

### Self-Hosting LibreTranslate

You can host your own LibreTranslate instance for full privacy and control. See the official documentation:  
[https://docs.libretranslate.com/guides/installation/](https://docs.libretranslate.com/guides/installation/)

Once running, point `url` to your server:

```yaml
libretranslate:
  url: "http://your-server:5000"
  api_key: "your-optional-key"
```

---

## Google Cloud Translate

**Engine type value:** `google`

Uses Google Cloud Translation API v2. See the official [Cloud Translation setup guide](https://docs.cloud.google.com/translate/docs/setup) and the [v2 translate REST method](https://docs.cloud.google.com/translate/docs/reference/rest/v2/translate).

### Setup

1. Follow Google's [Cloud Translation setup guide](https://docs.cloud.google.com/translate/docs/setup) to create/select a project, enable billing, and enable the **Cloud Translation API**.
2. In [Google Cloud Console](https://console.cloud.google.com/), create an **API key** under *APIs & Services → Credentials*.
3. Restrict the key to the Cloud Translation API when possible.
4. Copy the API key into the configuration.

### Configuration

```yaml
translation:
  engine:
    type: "google"

    google:
      api_key: "YOUR_GOOGLE_API_KEY"
      model: "nmt"
      timeout_seconds: 10
```

| Key | Default | Description |
|-----|---------|-------------|
| `api_key` | `""` | Your Google Cloud API key. |
| `model` | `"nmt"` | Translation model. `nmt` = Neural Machine Translation (recommended). `base` = phrase-based model. |
| `timeout_seconds` | `10` | Request timeout in seconds. |

---

## DeepL

**Engine type value:** `deepl`

Uses the [DeepL API](https://developers.deepl.com/api-reference/translate). Both Free and Pro tiers are supported.

### Setup

1. Create an account at [https://www.deepl.com/pro](https://www.deepl.com/pro).
2. Navigate to *Account → API Keys* and copy your key.

### Configuration

```yaml
translation:
  engine:
    type: "deepl"

    deepl:
      api_key: "YOUR_DEEPL_API_KEY"
      tier: "free"
      timeout_seconds: 10
```

| Key | Default | Description |
|-----|---------|-------------|
| `api_key` | `""` | Your DeepL API key. |
| `tier` | `"free"` | `"free"` uses the free-tier endpoint (`api-free.deepl.com`). `"pro"` uses the paid endpoint (`api.deepl.com`). |
| `timeout_seconds` | `10` | Request timeout in seconds. |

---

## Microsoft Azure Translator

**Engine type value:** `azure` or `microsoft`

Uses [Microsoft Azure Translator](https://learn.microsoft.com/azure/ai-services/translator/) with the [Translator v3 REST API](https://learn.microsoft.com/azure/ai-services/translator/text-translation/reference/v3/translate).

### Setup

1. In the [Azure Portal](https://portal.azure.com/), create a **Translator** resource.
2. Navigate to *Keys and Endpoint* and copy your API key.
3. Copy the region too if your resource is regional or multi-service. Leave it blank only for a global Translator resource.

### Configuration

```yaml
translation:
  engine:
    type: "azure"

    azure:
      api_key: "YOUR_AZURE_API_KEY"
      region: "eastus"
      endpoint: "https://api.cognitive.microsofttranslator.com"
      timeout_seconds: 10
```

| Key | Default | Description |
|-----|---------|-------------|
| `api_key` | `""` | Your Azure Translator API key. |
| `region` | `""` | Azure region/location (e.g. `"eastus"`, `"westeurope"`). Required for regional or multi-service resources; leave blank only for global Translator resources. |
| `endpoint` | `https://api.cognitive.microsofttranslator.com` | API endpoint. Keep the default unless Azure instructs otherwise. |
| `timeout_seconds` | `10` | Request timeout in seconds. |

---

## OpenAI

**Engine type value:** `openai`

Uses the [OpenAI Chat Completions API](https://developers.openai.com/api/reference/chat-completions/overview) to perform translations. This engine produces highly natural translations but is slower and more expensive than dedicated translation services.

Compatible with any OpenAI-compatible API (e.g. local models via Ollama or similar proxies).

### Setup

1. Create an account at [https://platform.openai.com/](https://platform.openai.com/).
2. Navigate to *API Keys* and generate a key.

### Configuration

```yaml
translation:
  engine:
    type: "openai"

    openai:
      api_key: "YOUR_OPENAI_API_KEY"
      model: "gpt-4o-mini"
      endpoint: "https://api.openai.com/v1/chat/completions"
      timeout_seconds: 20
```

| Key | Default | Description |
|-----|---------|-------------|
| `api_key` | `""` | Your OpenAI API key. |
| `model` | `"gpt-4o-mini"` | OpenAI model to use. `gpt-4o-mini` is recommended for a good balance of quality and cost. |
| `endpoint` | `https://api.openai.com/v1/chat/completions` | Chat Completions API endpoint. Change this to use a proxy or a self-hosted OpenAI-compatible API. |
| `timeout_seconds` | `20` | Request timeout in seconds. Higher timeout recommended for AI models. |

---

## Amazon Translate

**Engine type value:** `amazon`

Uses [Amazon Translate](https://aws.amazon.com/translate/) via AWS Signature V4 authentication and the [TranslateText API](https://docs.aws.amazon.com/translate/latest/APIReference/API_TranslateText.html).

### Setup

1. In the [AWS Console](https://console.aws.amazon.com/), create an IAM user or role with the AWS managed [`TranslateReadOnly`](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/TranslateReadOnly.html) policy, or a custom policy granting `translate:TranslateText`.
2. Generate **access keys** for the user.

### Configuration

```yaml
translation:
  engine:
    type: "amazon"

    amazon:
      access_key_id: "YOUR_AWS_ACCESS_KEY_ID"
      secret_access_key: "YOUR_AWS_SECRET_ACCESS_KEY"
      region: "us-east-1"
      timeout_seconds: 10
```

| Key | Default | Description |
|-----|---------|-------------|
| `access_key_id` | `""` | AWS access key ID. |
| `secret_access_key` | `""` | AWS secret access key. |
| `region` | `"us-east-1"` | AWS region where the Translate service is available (e.g. `"us-east-1"`, `"eu-west-1"`). |
| `timeout_seconds` | `10` | Request timeout in seconds. |

---

## Switching Engines

To switch engines, change `translation.engine.type` in `settings.yml` and provide the credentials for the chosen engine. Only the section matching the active engine type is used; all other credential blocks are ignored.

```yaml
translation:
  engine:
    type: "deepl"    # ← Change this value
```

Run `/bt reload` to apply the change without restarting the server.

---

## Comparison

| Engine | Cost | Quality | Setup Complexity | Supported Languages |
|--------|------|---------|-----------------|---------------------|
| LibreTranslate (Blueva) | Free | Good | None (zero-config) | 56+ (see [Supported Languages](./supported-languages.md)) |
| Google Translate | Pay-per-use | Excellent | Low | 130+ |
| DeepL | Free tier / Pay-per-use | Excellent | Low | 33 |
| Azure Translator | Pay-per-use | Excellent | Low | 100+ |
| OpenAI | Pay-per-use | Very high (natural) | Low | All major languages |
| Amazon Translate | Pay-per-use | Excellent | Medium | 75+ |

---

## Related Guides

- **[Configuration](./configuration.md)** - Full `settings.yml` reference.
- **[Supported Languages](./supported-languages.md)** - Languages available on the Blueva LibreTranslate instance.
