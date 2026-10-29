# First Steps

This guide will walk you through the initial setup of BlueTranslate on your server.

## Table of Contents

1. [Buy the Plugin](#buy-the-plugin)
2. [Requirements](#requirements)
3. [Installation](#installation)
4. [License](#license)
5. [Initial Configuration](#initial-configuration)
6. [Selecting a Language](#selecting-a-language)
7. [Next Steps](#next-steps)

---

## Buy the Plugin

First of all, you will need to purchase the plugin from one of the following stores:

- [BuiltByBit](https://builtbybit.com/resources/blue-translate.103628/)
- [SpigotMC](https://www.spigotmc.org/resources/blue-translate.134317/)
- [Blueva Store](https://blueva.net/store/blue-translate)
- Polymart (SOON)

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

## License

If you bought or downloaded the plugin from the [Blueva Store](https://blueva.net/store), your license key is already filled in and there is nothing for you to do.

If you bought it anywhere else (BuiltByBit, PixelEast, Polymart, etc.), the key starts empty and the plugin reminds you in the console every time it starts. To obtain and activate your license:

1. Create a free account at [blueva.net](https://blueva.net/register), or sign in if you already have one.
2. Open your [account settings](https://blueva.net/account?tab=settings) and find the **License** section.
3. Click **Show**, then **Copy** to copy your license key.
4. Paste the key into the `license.key` field of `plugins/BlueTranslate/license.yml` and restart the server.

```yaml
license:
  key: "your-license-key"
```

> **Note:** The product will be permanently linked to the Blueva account used during activation. Once linked, it cannot be transferred or linked to a different Blueva account.

- The same key works for every Blueva product you own.
- Your license can be used from up to **six different IP addresses**, with no limit on the number of servers running under each IP. Authorized IPs can be viewed and managed from your account settings.
- If your key is lost or compromised, you can replace it from the same page and paste the new one into `license.yml`. The old key stops working everywhere at once.
- Do not edit the `purchase` block in `license.yml`. It is filled in by the store you downloaded from and is only used to confirm your purchase.

For help, visit [blueva.net/support](https://blueva.net/support).

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
