# BlueSpoof 3.0 Documentation

Welcome to the official documentation for **BlueSpoof 3.0** - an AI-powered fake player solution for Minecraft servers.

## What's New in 3.0

BlueSpoof 3.0 is a complete rewrite of the plugin formerly known as Spoof Engine:

- **Modular Architecture**: Every feature is now a separate module that can be toggled independently
- **Movement Engine**: Fake players roam the world with human-like behavior - walking, sprinting, jumping, and idle pauses
- **Trace System**: Record real player paths in defined regions and replay them with bots so lobbies look naturally populated
- **Multi-Provider AI Chat**: Supports OpenAI, Claude, Gemini, Grok, DeepSeek, and Ollama with automatic failover
- **Discretion Mode**: Hide BlueSpoof branding in console output and command exposure
- **Ranks Module**: Automatically assign weighted permission ranks to fake players on join
- **Enhanced Proxy Sync**: TCP, Redis, SQL, and Plugin Messaging support for BungeeCord/Velocity

## Documentation Sections

### Getting Started
- [First Steps](first-steps.md) - Installation, requirements, and basic setup

### Reference
- [Commands & Permissions](commands-permissions.md) - Complete command reference
- [Placeholders](placeholders.md) - PlaceholderAPI integration

### Modules
- [Connection](connection.md) - Core fake-player connection behavior, ping spoofing, IP spoofing, and batch connect
- [Movement](movement.md) - Human-like roaming, sprinting, jumping, idle pauses, and stuck recovery
- [Trace](trace.md) - Record real player paths and replay them with fake players
- [Chat](chat.md) - AI-generated and configurable messages for fake players
- [Fluctuation](fluctuation.md) - Time-based automatic join and leave schedules
- [Events](events.md) - Commands triggered on join, quit, and timed tasks
- [Ranks](ranks.md) - Weighted rank assignment for fake players
- [Discretion](discretion.md) - Hide BlueSpoof branding in console and commands

### Proxy
- [BlueSpoofProxy](proxy.md) - BungeeCord/Velocity proxy setup

## Requirements

- **Server**: Spigot 1.21.11 or 26.1 (Paper, Purpur, Folia, and other forks supported)
- **Java**: 21 or higher
- **RAM**: 1-2 GB minimum (depending on fake player count)

## Optional Dependencies

- **PlaceholderAPI** - For placeholder support in external plugins
- **LuckPerms** - For the Ranks module
- **AuthMe** - For compatibility with authentication plugins
- **DecentHolograms** - For hologram integration

## Support

- Discord: [Blueva Development](https://discord.gg/blueva)
- Store: [blueva.net/store](https://blueva.net/store)
