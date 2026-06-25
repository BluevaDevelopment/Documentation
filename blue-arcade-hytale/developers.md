# Creating a Module

This guide explains how to create a custom minigame module for Blue Arcade 3.0 (Hytale Edition).

## Overview

Blue Arcade uses a modular architecture where each minigame is a separate JAR file that implements the `GameModule` interface.

**Requirements:**
- Java 21
- Maven or Gradle
- IntelliJ IDEA (recommended)

---

## Project Setup

### 1. Create a New Maven Project

Create a new Maven project in IntelliJ IDEA with the following structure:

```
my-module/
├── pom.xml
└── src/
    └── main/
        ├── java/
        │   └── com/example/mymodule/
        │       └── MyModule.java
        └── resources/
            ├── module.yml
            └── files/
                └── language.yml   ← config files go here
```

> **Important:** All resource files that should be extracted to the module's data folder must be placed inside `src/main/resources/files/`. On first load, the core extracts them automatically and never overwrites existing files.

### 2. Configure pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-module</artifactId>
    <version>1.0.0</version>

    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <repositories>
        <repository>
            <id>hytale-release</id>
            <url>https://maven.hytale.com/release</url>
        </repository>
        <repository>
            <id>hytale-pre-release</id>
            <url>https://maven.hytale.com/pre-release</url>
        </repository>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>

    <dependencies>
        <!-- Hytale Server API -->
        <dependency>
            <groupId>com.hypixel.hytale</groupId>
            <artifactId>Server</artifactId>
            <version>2026.02.19-1a311a592</version>
            <scope>provided</scope>
        </dependency>

        <!-- BlueArcade API -->
        <dependency>
            <groupId>com.github.BluevaDevelopment</groupId>
            <artifactId>BlueArcadeAPI</artifactId>
            <version>3.2.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
</project>
```

### 3. Create module.yml

Create `src/main/resources/module.yml`:

```yaml
id: my_module
name: My Module
version: 1.0.0
type: MINIGAME        # MINIGAME (2–10 min) or MICROGAME (< 2 min)
api-version: 3.2
platform: HYTALE
main: com.example.mymodule.MyModule

authors:
  - YourName

description: "A fun minigame!"
```

---

## Implementing GameModule

### Basic Module Structure

The Hytale edition uses the following concrete types for the `GameModule` generics:

| Type parameter | Hytale type |
|---|---|
| `P` (Player) | `com.hypixel.hytale.server.core.entity.entities.Player` |
| `L` (Location) | `com.hypixel.hytale.math.vector.Location` |
| `W` (World) | `com.hypixel.hytale.server.core.universe.world.World` |
| `M` (Material) | `String` (block/item identifier) |
| `I` (ItemStack) | `com.hypixel.hytale.server.core.inventory.ItemStack` |
| `S` (Sound) | `String` (sound event identifier) |
| `B` (BlockState) | `com.hypixel.hytale.server.core.universe.world.meta.BlockState` |
| `E` (Entity) | `com.hypixel.hytale.server.core.entity.Entity` |
| `Ls` (Listener) | `net.blueva.arcade.api.events.EventSubscription<?>` |
| `Pr` (Priority) | `Short` |

Create your main module class implementing `GameModule`:

```java
package com.example.mymodule;

import com.hypixel.hytale.math.vector.Location;
import com.hypixel.hytale.server.core.entity.Entity;
import com.hypixel.hytale.server.core.entity.entities.Player;
import com.hypixel.hytale.server.core.inventory.ItemStack;
import com.hypixel.hytale.server.core.universe.world.World;
import com.hypixel.hytale.server.core.universe.world.meta.BlockState;
import net.blueva.arcade.api.ModuleAPI;
import net.blueva.arcade.api.config.CoreConfigAPI;
import net.blueva.arcade.api.config.ModuleConfigAPI;
import net.blueva.arcade.api.events.CustomEventRegistry;
import net.blueva.arcade.api.events.EventSubscription;
import net.blueva.arcade.api.game.GameContext;
import net.blueva.arcade.api.game.GameModule;
import net.blueva.arcade.api.game.GameResult;
import net.blueva.arcade.api.module.ModuleInfo;

import java.util.Map;

public class MyModule implements GameModule<Player, Location, World, String,
        ItemStack, String, BlockState, Entity, EventSubscription<?>, Short> {

    private ModuleInfo moduleInfo;
    private ModuleConfigAPI moduleConfig;
    private CoreConfigAPI coreConfig;

    @Override
    public void onLoad() {
        // Get module info from module.yml
        moduleInfo = ModuleAPI.getModuleInfo("my_module");

        if (moduleInfo == null) {
            throw new IllegalStateException("ModuleInfo not available");
        }

        // Get configuration APIs
        moduleConfig = ModuleAPI.getModuleConfig(moduleInfo.getId());
        coreConfig = ModuleAPI.getCoreConfig();

        // Register configuration files (optional)
        moduleConfig.register("language.yml", 1);
    }

    @Override
    public void onStart(GameContext<Player, Location, World, String,
            ItemStack, String, BlockState, Entity> context) {
        // Called when the game starts (countdown begins)
        // Initialize game state here
    }

    @Override
    public void onCountdownTick(GameContext<Player, Location, World, String,
            ItemStack, String, BlockState, Entity> context, int secondsLeft) {
        // Called every second during countdown
        // Send titles, sounds, etc.
    }

    @Override
    public void onCountdownFinish(GameContext<Player, Location, World, String,
            ItemStack, String, BlockState, Entity> context) {
        // Called when countdown finishes
        // Send "GO!" title, etc.
    }

    @Override
    public void onGameStart(GameContext<Player, Location, World, String,
            ItemStack, String, BlockState, Entity> context) {
        // Called after countdown, game is now active
        // Start game timers, spawn entities, etc.
    }

    @Override
    public void onEnd(GameContext<Player, Location, World, String,
            ItemStack, String, BlockState, Entity> context, GameResult<Player> result) {
        // Called when game ends
        // Clean up, record stats, etc.
    }

    @Override
    public void onDisable() {
        // Called when module is unloaded
        // Clean up resources
    }

    @Override
    public void registerEvents(CustomEventRegistry<EventSubscription<?>, Short> registry) {
        // Register event subscriptions (see Event Handling section)
    }

    @Override
    public boolean freezePlayersOnCountdown() {
        // Return true to freeze players during countdown
        return true;
    }

    @Override
    public Map<String, String> getCustomPlaceholders(Player player) {
        // Return custom placeholders for this module
        return Map.of();
    }
}
```

---

## GameContext API

The `GameContext` provides access to all game data and APIs:

```java
// Get arena ID
int arenaId = context.arenaId();

// Get all players
Collection<Player> players = context.players().all();
Collection<Player> alive = context.players().alive();
Collection<Player> spectators = context.players().spectators();

// Get game configuration
GameConfig config = context.config();
int duration = config.durationMinutes();
List<Location> spawns = config.spawns();
WorldRegion bounds = config.bounds();

// Send messages
context.messages().broadcast("Game starting!");
context.messages().send(player, "You are alive!");

// Send titles
context.titles().broadcast("GO!", "The game has started", 10, 40, 10);
context.titles().send(player, "Eliminated!", "", 0, 40, 10);

// Play sounds
context.sounds().broadcast("hytale:sound.player.level_up", 1.0f, 1.0f);

// Manage players
context.players().eliminate(player, "Fell out of bounds");
context.players().setSpectator(player);

// End the game
context.endGame();
```

---

## Registering Statistics

Track custom stats for your module:

```java
import net.blueva.arcade.api.stats.StatsAPI;
import net.blueva.arcade.api.stats.StatDefinition;
import net.blueva.arcade.api.stats.StatScope;

public class MyStatsService {
    private final StatsAPI statsAPI;
    private final ModuleInfo moduleInfo;

    public MyStatsService(StatsAPI statsAPI, ModuleInfo moduleInfo) {
        this.statsAPI = statsAPI;
        this.moduleInfo = moduleInfo;
    }

    public void registerStats() {
        if (statsAPI == null) return;

        statsAPI.registerModuleStat(moduleInfo.getId(),
            new StatDefinition("wins", "Wins", "Games won", StatScope.MODULE));

        statsAPI.registerModuleStat(moduleInfo.getId(),
            new StatDefinition("games_played", "Games Played",
                "Games played", StatScope.MODULE));
    }

    public void recordWin(Player player) {
        if (statsAPI == null) return;
        statsAPI.addModuleStat(player, moduleInfo.getId(), "wins", 1);
        statsAPI.addGlobalStat(player, "wins", 1);
    }

    public void recordGamePlayed(Collection<Player> players) {
        if (statsAPI == null) return;
        for (Player player : players) {
            statsAPI.addModuleStat(player, moduleInfo.getId(), "games_played", 1);
        }
    }
}
```

Use in your module:

```java
@Override
public void onLoad() {
    // ... other initialization ...

    StatsAPI statsAPI = ModuleAPI.getStatsAPI();
    statsService = new MyStatsService(statsAPI, moduleInfo);
    statsService.registerStats();
}

@Override
public void onEnd(GameContext<...> context, GameResult<Player> result) {
    // Record game played for all players
    statsService.recordGamePlayed(context.players().all());

    // Record win for the winner
    if (result.winner() != null) {
        statsService.recordWin(result.winner());
    }
}
```

---

## Registering in Vote Menu

Add your game to the vote menu:

```java
@Override
public void onLoad() {
    // ... other initialization ...

    VoteMenuAPI voteMenu = ModuleAPI.getVoteMenuAPI();

    if (voteMenu != null && moduleConfig != null) {
        voteMenu.registerGame(
            moduleInfo.getId(),
            "hytale:item.diamond",  // Item identifier (String)
            "My Module",            // Display name
            List.of("A fun minigame!", "Click to vote")  // Lore
        );
    }
}
```

---

## Custom Setup Commands

Register custom setup commands for arena configuration:

```java
import net.blueva.arcade.api.setup.GameSetupHandler;
import net.blueva.arcade.api.setup.SetupContext;

public class MySetup implements GameSetupHandler {

    @Override
    public boolean handle(SetupContext context) {
        String[] args = context.args();

        if (args.length < 1) {
            return false;
        }

        switch (args[0].toLowerCase()) {
            case "customsetting":
                return handleCustomSetting(context, args);
            default:
                return false;
        }
    }

    private boolean handleCustomSetting(SetupContext context, String[] args) {
        if (args.length < 2) {
            context.sendMessage("Usage: customsetting <value>");
            return true;
        }

        // Save the setting
        context.data().set("custom_setting", args[1]);
        context.sendMessage("Custom setting saved!");
        return true;
    }
}
```

Register in onLoad:

```java
ModuleAPI.getSetupAPI().registerHandler(moduleInfo.getId(), new MySetup());
```

---

## Registering Achievements

```java
@Override
public void onLoad() {
    // ... other initialization ...

    // Register achievement config file
    moduleConfig.register("achievements.yml", 1);

    // Load achievements
    AchievementsAPI achievementsAPI = ModuleAPI.getAchievementsAPI();
    if (achievementsAPI != null) {
        achievementsAPI.registerModuleAchievements(
            moduleInfo.getId(),
            "achievements.yml"
        );
    }
}
```

Create `achievements.yml`:

```yaml
categories:
  my_module:
    display_name: "My Module"
    icon: DIAMOND
    achievements:
      first_win:
        display_name: "First Victory"
        description: "Win your first game"
        phases:
          1:
            condition: "%bluearcade_stat_module_my_module_wins% >= 1"
            rewards:
              exp: 100
              credits: 50
```

---

## Event Handling

In the Hytale edition events are registered using `EventSubscription<E>` rather than a Bukkit `Listener`. Each subscription pairs an event class with a handler lambda.

```java
import com.hypixel.hytale.server.core.event.events.player.PlayerInteractEvent;
import net.blueva.arcade.api.events.EventSubscription;

@Override
public void registerEvents(CustomEventRegistry<EventSubscription<?>, Short> registry) {
    registry.register(new EventSubscription<>(PlayerInteractEvent.class, event -> {
        Player player = event.getPlayer();

        // Check if player is in your game
        if (!gameManager.isPlaying(player)) {
            return;
        }

        // Handle interaction
    }));
}
```

You can register as many subscriptions as needed:

```java
@Override
public void registerEvents(CustomEventRegistry<EventSubscription<?>, Short> registry) {
    registry.register(new EventSubscription<>(PlayerInteractEvent.class, this::onInteract));
    registry.register(new EventSubscription<>(PlayerConnectEvent.class, this::onConnect));
}
```

---

## Building and Installing

### Build the JAR

```bash
mvn clean package
```

### Install the Module

1. Copy the JAR to `mods/Blueva_BlueArcade3/modules/`
2. Run `/baa module load [module_id]` to load the module (e.g., `/baa module load my_module`)

---

## Module Lifecycle

1. **onLoad()** - Called once when server starts
   - Register stats, vote menu, setup commands
   - Initialize configuration

2. **onStart()** - Called when arena selects this game
   - Initialize game state
   - Prepare arena

3. **onCountdownTick()** - Called every second during countdown
   - Send countdown titles/sounds

4. **onCountdownFinish()** - Called when countdown ends
   - Send "GO!" message

5. **onGameStart()** - Called after countdown
   - Start game timers
   - Enable game mechanics

6. **onEnd()** - Called when game ends
   - Record statistics
   - Clean up game state

7. **onDisable()** - Called when module unloads
   - Clean up resources

---

## Best Practices

1. **Use the Context** - The `GameContext` provides all necessary APIs
2. **Clean up properly** - Reset state in `onEnd()` and `onDisable()`
3. **Handle null checks** - APIs may return null if disabled
4. **Register stats early** - Do it in `onLoad()`
5. **Use configuration** - Make your module configurable
6. **Test thoroughly** - Test with multiple players and edge cases

---

## Example Modules

Check out official module source code for reference (Minecraft edition — structure and API calls are the same, only Bukkit types differ):

- [Race](https://github.com/BluevaDevelopment/BlueArcade_Race) - Simple race game
- [Spleef](https://github.com/BluevaDevelopment/BlueArcade_Spleef) - Block breaking
- [Snowball Fight](https://github.com/BluevaDevelopment/BlueArcade_SnowballFight) - PvP combat

---

## API Reference

The BlueArcade API is available at:
- **JitPack**: `com.github.BluevaDevelopment:BlueArcadeAPI:3.2.0`
- **GitHub**: [BlueArcadeAPI](https://github.com/BluevaDevelopment/BlueArcadeAPI)

The Hytale Server API is available at:
- **Maven**: `https://maven.hytale.com/release` — `com.hypixel.hytale:Server:<version>`

Key interfaces and classes:
- `GameModule` - Main module interface
- `GameContext` - Game state and APIs
- `ModuleAPI` - Access to all APIs
- `StatsAPI` - Statistics tracking
- `VoteMenuAPI` - Vote menu registration
- `AchievementsAPI` - Achievement registration
- `GameSetupHandler` - Setup command handling
- `EventSubscription<E>` - Hytale event subscription wrapper
