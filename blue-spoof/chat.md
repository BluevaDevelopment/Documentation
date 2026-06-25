# Chat Module

**File:** `plugins/BlueSpoof/modules/chat.yml`

**Status:** Optional. Enable by setting `chat.enabled: true`.

---

## What It Does

The Chat module makes fake players send messages in chat. There are two independent subsystems:

- **AI Messages** — Generate realistic chat messages using cloud AI providers
- **Configurable Messages** — Use manually written messages from predefined lists

Both subsystems can be enabled at the same time, but they serve different purposes. AI messages adapt dynamically to context, while configurable messages are predictable and require no external API.

---

## AI Messages

### Supported Providers

BlueSpoof supports multiple AI providers with automatic failover. If one provider fails (invalid key, rate limit, timeout), the next in the priority list is tried.

| Provider | Model Example | Endpoint |
|----------|--------------|----------|
| OpenAI | `gpt-4.1-mini` | `https://api.openai.com/v1/responses` |
| Claude | `claude-sonnet-4-20250514` | `https://api.anthropic.com/v1/messages` |
| Gemini | `gemini-2.5-flash` | `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent` |
| Grok | `grok-4-fast-reasoning` | `https://api.x.ai/v1/chat/completions` |
| DeepSeek | `deepseek-chat` | `https://api.deepseek.com/chat/completions` |
| Ollama | `llama3.1` | `http://localhost:11434/api/chat` |

### Provider Priority

The `provider_priority` list defines the failover order. The plugin only moves to the next provider when the current one actually fails, so you do not burn tokens by sending the same prompt to all providers at once.

```yaml
provider_priority:
  - "openai"
  - "claude"
  - "gemini"
  - "grok"
  - "deepseek"
  - "ollama"
```

### Enabling a Provider

Each provider has its own `enabled`, `secret_key`, `model`, and `endpoint` settings. Set `enabled: true` and provide a valid API key for the providers you want to use.

**Warning:** Cloud providers may charge for API usage. Monitor your usage and set spending limits in your provider dashboard.

### Random Messages

Periodically sends AI-generated chat messages that look like natural player talk.

| Option | Description | Default |
|--------|-------------|---------|
| `chat.sub_modules.ai_messages.random_messages.enabled` | Enable random AI messages | `true` |
| `chat.sub_modules.ai_messages.random_messages.required_real_players` | Minimum real players online to trigger | `10` |
| `chat.sub_modules.ai_messages.random_messages.interval` | Ticks between message attempts | `200` (10 seconds) |
| `chat.sub_modules.ai_messages.random_messages.chance` | Chance (0-100) to send a message when the interval fires | `30` |
| `chat.sub_modules.ai_messages.random_messages.cache_message` | Save generated messages in cache for later reuse | `true` |

#### Prompts

You can define multiple prompts. The plugin picks one at random for each message generation. Good prompts should:
- Ask for short messages (3-12 words)
- Request casual gamer style (lowercase, small typos, no periods)
- Explicitly forbid mentions of AI, bots, or instructions

Example prompts are included in the default configuration for common scenarios: mining, building, PvP, asking for help, and economy chat.

### Interactions

When a real player sends a message in chat, the AI can generate a reply from a fake player.

| Option | Description | Default |
|--------|-------------|---------|
| `chat.sub_modules.ai_messages.interactions.enabled` | Enable AI replies to public chat | `true` |
| `chat.sub_modules.ai_messages.interactions.required_real_players` | Minimum real players online | `10` |
| `chat.sub_modules.ai_messages.interactions.chance` | Chance to reply when triggered | `30` |
| `chat.sub_modules.ai_messages.interactions.wait_per_letter` | Simulated typing delay per character (seconds) | `0.5` |

The `wait_per_letter` setting makes the fake player look like they are actually typing. A 10-character message with `0.5` seconds per letter will wait 5 seconds before sending.

### Private Messages

Fake players can reply when real players send them private messages via `/msg` or `/tell`.

| Option | Description | Default |
|--------|-------------|---------|
| `chat.sub_modules.ai_messages.private_messages.enabled` | Enable PM replies | `true` |
| `chat.sub_modules.ai_messages.private_messages.required_real_players` | Minimum real players online | `1` |
| `chat.sub_modules.ai_messages.private_messages.chance` | Chance to reply | `50` |
| `chat.sub_modules.ai_messages.private_messages.wait_per_letter` | Typing delay per character | `0.5` |
| `chat.sub_modules.ai_messages.private_messages.commands` | Command labels considered private messages | `msg`, `tell` |

---

## Configurable Messages

If you prefer not to use AI APIs, you can define static messages and interaction triggers manually.

### Random Messages

A simple list of messages sent periodically.

| Option | Description | Default |
|--------|-------------|---------|
| `chat.sub_modules.configurable_messages.random_messages.enabled` | Enable static random messages | `false` |
| `chat.sub_modules.configurable_messages.random_messages.required_real_players` | Minimum real players online | `10` |
| `chat.sub_modules.configurable_messages.random_messages.interval` | Ticks between attempts | `200` |
| `chat.sub_modules.configurable_messages.random_messages.chance` | Chance to send | `30` |
| `chat.sub_modules.configurable_messages.random_messages.list` | List of messages to send | (empty) |

### Interactions

Reply with preset answers when real players say specific trigger phrases.

| Option | Description | Default |
|--------|-------------|---------|
| `chat.sub_modules.configurable_messages.interactions.enabled` | Enable static interactions | `false` |
| `chat.sub_modules.configurable_messages.interactions.required_real_players` | Minimum real players online | `5` |

Each interaction entry contains:
- `trigger` — List of phrases that trigger the reply (case-insensitive partial match)
- `answers` — List of possible replies (one is chosen at random)
- `options.response_chance` — Probability (0-100) that a fake player will reply
- `options.wait_per_letter` — Simulated typing delay

Example:

```yaml
chat:
  sub_modules:
    configurable_messages:
      interactions:
        enabled: true
        required_real_players: 5
        1:
          trigger:
            - 'How do I get money?'
            - 'I need money'
          answers:
            - '{player_name} you can sell items at /shop'
            - '{player_name} try /sell'
          options:
            response_chance: 30
            wait_per_letter: 0.3
```

The `{player_name}` placeholder is replaced with the real player's name who triggered the interaction.

---

## Notes

- AI messages and configurable messages are independent. You can enable one, both, or neither.
- `required_real_players` exists to prevent fake players from talking to each other in an empty server, which looks suspicious.
- Cached AI messages are stored in `cache/chat.json` and reused when the same context comes up, saving API tokens.
- If all AI providers fail, no message is sent. The plugin does not fall back to configurable messages automatically.
- `/r` (reply) cannot be intercepted reliably for private message replies because the target player is not present in the command string.
