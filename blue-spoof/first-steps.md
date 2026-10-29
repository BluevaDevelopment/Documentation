# Getting Started

## Purchase the Plugin

Download BlueSpoof from one of the official stores:

- [Blueva Store](https://blueva.net/store/blue-spoof)
- [BuiltByBit]([https://builtbybit.com/resources/blue-spoof-ai-powered-fake-players.36987/])
- [voxel.shop]([https://voxel.shop/product/1912/blue-spoof])
- [Magmaro]([https://magmaro.com/resources/blue-spoof-ai-powered-fake-players-FIouNjt4yQWD?tab=description])
- [PixelEast]([https://pixeleast.com/product/spoof])

## Server Requirements

Before installing, ensure your server meets these requirements:

- **Spigot** or any fork (Paper, Purpur, Folia, etc.)
- **Minecraft 1.8.8 to 26.2** a single JAR works on every supported version
- **Java 8** or higher for 1.8.8–1.16.5 servers; **Java 21** or higher for 1.17+ servers
- **1-2 GB RAM** minimum (excluding other plugins and players)
- **Internet access on first startup** the plugin downloads its required libraries (database drivers, etc.) once into `plugins/BlueSpoof/libraries/`

## Installation

1. **Stop your server** if it is running
2. **Download** the plugin JAR from your purchase location
3. **Place** the JAR file in your server's `plugins` folder
4. **Start** the server
5. The plugin will generate all configuration files automatically

## License

If you bought or downloaded the plugin from the [Blueva Store](https://blueva.net/store), your license key is already filled in and there is nothing for you to do.

If you bought it anywhere else (BuiltByBit, PixelEast, Polymart, etc.), the key starts empty and the plugin reminds you in the console every time it starts. To obtain and activate your license:

1. Create a free account at [blueva.net](https://blueva.net/register), or sign in if you already have one.
2. Open your [account settings](https://blueva.net/account?tab=settings) and find the **License** section.
3. Click **Show**, then **Copy** to copy your license key.
4. Paste the key into the `license.key` field of `plugins/BlueSpoof/license.yml` and restart the server.

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

## Using BungeeCord or Velocity?

Install the **same BlueSpoof 3.9.2 or newer JAR** on the proxy and every backend you want to synchronize, then restart them. Fresh configurations already enable the default `plugin` bridge, which generates keys and pairs automatically on first contact.

The bridge uses the existing backend Minecraft ports and detects backend names automatically. No separate proxy download, extra listening port, Redis or database is needed.

Keep your normal proxy forwarding setup and backend firewall rules. Existing installations may retain their previous transport settings; follow [Proxy Sync](proxy.md) for transport selection, automatic pairing and troubleshooting.

Standalone servers do not need a proxy installation or a change to their authentication settings. Automatic pairing stays dormant on online-mode standalone servers.

## First Fake Player Connection

### 1. Open the Admin GUI (recommended)

If the Admin GUI is enabled (default), simply run:

```
/bs
```

This opens an interactive menu where you can connect, disconnect, and manage fake players without memorizing commands.

### 2. Connect a Fake Player via Command

```
/bs connect 1
```

This connects one fake player to the server using a random nick from `nicks.yml`.

### 3. Check That It Worked

```
/bs list
```

You should see the fake player's name in the list.

### 4. Disconnect

```
/bs disconnect all
```

This removes all fake players from the server.

## Configuration Files

After the first start, BlueSpoof creates the following files inside `plugins/BlueSpoof/`:

| File | Purpose |
|------|---------|
| `settings.yml` | Global settings - language, cache, proxy sync, GUI toggle |
| `nicks.yml` | Pool of names used for fake players |
| `modules/connection.yml` | Ping spoofing, IP spoofing, batch connect behavior |
| `modules/movement.yml` | Human-like roaming, sprinting, jumping, idle pauses |
| `modules/chat.yml` | AI-generated and configurable messages |
| `modules/fluctuation.yml` | Time-based automatic join/leave schedules |
| `modules/events.yml` | Commands triggered on join, quit, and timed tasks |
| `modules/trace.yml` | Recording regions and playback settings |
| `modules/ranks.yml` | Weighted rank assignment for fake players |
| `modules/discretion.yml` | Hide BlueSpoof branding |
| `languages/en_UK.yml` | All plugin messages and GUI text |

## Quick Command Reference

| Action | Command |
|--------|---------|
| Open GUI | `/bs` |
| Connect fake players | `/bs connect [amount]` |
| Disconnect fake players | `/bs disconnect [amount\|all\|name]` |
| List fake/real players | `/bs list (fake\|real)` |
| Check if a player is fake | `/bs check [player]` |
| Send chat as a fake player | `/bs chat [name] [message]` |
| Force a fake player to run a command | `/bs sudo [name] [command]` |
| Take control of a fake player | `/bs control [name]` |
| Stop controlling | `/bs control stop` |
| Reload plugin | `/bs reload` |

## Enabling Modules

All modules are controlled through their respective YAML files. To enable one:

1. Open the module's config file in `plugins/BlueSpoof/modules/`
2. Set `enabled: true` (or the equivalent key for that module)
3. Save the file
4. Run `/bs reload` or restart the server

The **Connection** module is always active because it handles core fake-player connection behavior.

## Troubleshooting

**"No fake players connected" after running `/bs connect`**
- Check the console for errors
- On a proxy network, verify the normal proxy forwarding setup and backend firewall rules
- Verify that `nicks.yml` is not empty

**"Insufficient permissions"**
- You need `bluespoof.admin` or OP to use most commands
- Individual permissions are listed in [Commands & Permissions](commands-permissions.md)

**Fake players disconnect immediately**
- Check for authentication plugin conflicts (AuthMe, etc.)
- On a proxy network, check that forwarding and backend authentication settings match your proxy setup
- Review the Events module; some example commands are disabled by default

**High CPU usage with many fake players**
- Disable the Movement module or reduce the number of roaming bots
- Lower the `max_frames` value in `trace.yml` if using Trace recordings
- Enable isolation in `movement.yml` to keep bots in a hidden world
