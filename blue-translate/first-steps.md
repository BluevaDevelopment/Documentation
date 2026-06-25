# First Steps

This guide will walk you through the initial setup of BlueTranslate on your server.

## Table of Contents

1. [Buy the Plugin](#buy-the-plugin)
2. [Requirements](#requirements)
3. [Installation](#installation)
4. [Initial Configuration](#initial-configuration)
5. [Selecting a Language](#selecting-a-language)
6. [Next Steps](#next-steps)

---

## Buy the Plugin

First of all, you will need to purchase the plugin from one of the following stores:

- [BuiltByBit](https://builtbybit.com/resources/blue-translate.103628/)
- [SpigotMC](https://www.spigotmc.org/resources/blue-translate.134317/)
- [Blueva Store](https://blueva.net/store/blue-translate)
- [Polymart](https://polymart.org/resource) (SOON)

Once you have purchased it, you must wait for the payment to be processed. Usually it is instantaneous, but there may be exceptions. If the payment takes more than 24 h to be processed, please contact us.

---

## Requirements

Before installing BlueTranslate, make sure your server meets the following requirements:

- **Minecraft Server**: Spigot, Paper, or any compatible fork (Bukkit is **not** supported).
- **Server Version**: 1.21 or higher.
- **Java Version**: Java 17 or higher.
- **Server RAM**: At least ~1 GB available for the plugin.
- **Dependency**: [PacketEvents](https://www.spigotmc.org/resources/packetevents-api.80279/) must be installed.

> **Optional**: [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/).

---

## Installation

### Step 1: Download the Required Dependencies

BlueTranslate requires **PacketEvents**. Download it from SpigotMC and place it in your `plugins/` folder before starting the server.

### Step 2: Download BlueTranslate

Download the `BlueTranslate.jar` file from the store where you purchased it.

### Step 3: Install the Plugin

1. Stop your server if it is running.
2. Place `BlueTranslate.jar` in your server's `plugins/` folder.
3. Start the server. BlueTranslate will generate its configuration files automatically.
4. Stop the server once it has fully started.

### Step 4: Verify Installation

Start the server again and run the following command in-game or in the console:

```
/bt
```

You should see the plugin information displayed. BlueTranslate is now ready to use.

After first installation, BlueTranslate creates the following directory structure:

```
plugins/BlueTranslate/
├── settings.yml          # Main configuration file
├── language.yml          # Plugin messages
├── menus/
│   └── select_language.yml   # Language selector GUI configuration
└── translations/         # Manual translation YAML files (optional)
    └── example.yml
```

---

## Initial Configuration

Open `plugins/BlueTranslate/settings.yml`. The most important section to review on first install is the `translation` block:

```yaml
translation:
  source_language: "auto"    # Let the engine detect the source language automatically
  default_language: "en"     # Fallback language for players with undetected locales

  engine:
    type: "libretranslate"   # Default engine - works with zero configuration

    libretranslate:
      url: "https://translate.blueva.net"   # Official Blueva public instance (free)
      api_key: ""                            # Leave blank for the public instance
      timeout_seconds: 10
```

> BlueTranslate uses the **Blueva-hosted LibreTranslate instance** (`https://translate.blueva.net`) by default. No account or API key is needed. This is the recommended setup for most servers.

After editing the file, run `/bt reload` or restart the server to apply changes.

---

## Selecting a Language

Players can choose their preferred language at any time:

- **Open the language selector GUI** - run `/bt select` with no arguments.
- **Set a specific language directly** - run `/bt select <language_code>` (e.g. `/bt select es` for Spanish).
- **Reset to automatic detection** - run `/bt select auto`.

For a full list of supported language codes, see [Supported Languages](./supported-languages.md).

---

## Next Steps

Now that BlueTranslate is running, explore these guides:

1. **[Commands & Permissions](./commands-and-permissions.md)** - Full command reference.
2. **[Configuration](./configuration.md)** - Tune the cache, batch settings, packet coverage, and more.
3. **[Translation Engines](./translation-engines.md)** - Switch to Google, DeepL, Azure, OpenAI, or Amazon.
4. **[Manual Translations](./manual-translations.md)** - Override automatic translations with curated YAML files.
5. **[Supported Languages](./supported-languages.md)** - Browse all available languages and the Blueva REST API.
6. **[PlaceholderAPI](./placeholderapi.md)** - Use BlueTranslate data in scoreboards, menus, and more.
7. **[Common Issues](./common-issues.md)** - Solutions to the most frequent problems.

---

## Troubleshooting

**Plugin not loading:**
- Check the console for error messages.
- Make sure PacketEvents is installed and loaded correctly.
- Verify you are running Java 17+ and a compatible server version (1.21+).

**Commands not working:**
- Check that you have the required permission (see [Commands & Permissions](./commands-and-permissions.md)).
- Try the alias `/bluetranslate` if `/bt` conflicts with another plugin.

**Need more help?** Visit [https://blueva.net](https://blueva.net).
