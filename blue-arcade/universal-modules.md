# Universal Modules

Blue Arcade is moving to a new module format called **universal modules**, distributed as `.bamodule` files. A universal module ships its game logic as sandboxed Lua scripts instead of compiled Java, and the same file runs on every platform Blue Arcade supports.

Universal modules require **Blue Arcade 3.5 or higher**. They are **experimental**: they load next to your current modules and can be tested today, but they are not the recommended choice for a production server yet.

## Legacy Modules

The `.jar` modules used until now are called **legacy modules**. Nothing changes for them right now: they keep loading, keep working and keep receiving updates.

| Term | Format | Meaning |
|------|--------|---------|
| Universal module | `.bamodule` | Lua source in a sandboxed archive. One file runs on every platform |
| Legacy module | `.jar` | Compiled Java, one build per platform, full server privileges |

All 27 official minigames have already been ported to the universal format, each one verified against the behaviour of its legacy version. The universal versions live at [BlueArcade_Modules](https://github.com/BluevaDevelopment/BlueArcade_Modules).

Legacy modules will keep being updated for several version branches, probably until 3.9. Only after that, once the universal versions have proven stable on real servers, will the legacy format be removed. When that happens, Blueva will port third party modules to the universal format **for free**, so no module is left behind.

## Why the Change

### One module, every platform

A legacy module has to be written once per platform. Several official minigames already ship a Minecraft build and a Hytale build of the same game, and every new backend multiplies that cost.

A universal module is written against a platform neutral API, so it is written once and runs on every backend. Only the binding layer inside Blue Arcade has to be ported, and that is work Blue Arcade has to do anyway.

### Sandboxing

A legacy module runs as arbitrary Java code with full plugin privileges: filesystem, network, reflection, process execution. There is no technical barrier between a module you install and your server.

A universal module runs inside a Lua sandbox with a curated set of capabilities. It can only do what the API allows, and every capability it needs is declared in its manifest and enforced at runtime. This is also a requirement of the marketplaces we want to distribute through.

### Anyone can write a minigame

Writing a legacy module requires Java or Kotlin, a build system, and familiarity with the Bukkit event model. Writing a universal module requires a text editor. That difference decides how many people can build for Blue Arcade at all.

### Version support stops being a per module problem

Supporting a range of Minecraft versions from a legacy module is painful, and every module pays that cost separately: renamed materials, renamed sounds and particles, block data replacing data values, item and entity changes, and reflection wherever the API falls short. Across the official modules alone there are hundreds of places where a version difference can break something.

A universal module never names a platform value. It says `minecraft:snow_block` and `minecraft:block.snow.break`, and Blue Arcade resolves those to whatever the running server version calls them. Supporting a new Minecraft release becomes one Blue Arcade update instead of 27 module updates, and a module written today keeps working years later without its author touching it.

### Virtual worlds need it

Blue Arcade is working towards virtual worlds: arenas held in memory and rendered to players through packets instead of being real server worlds. In a virtual world the server fires no Bukkit events, and every legacy module is built on exactly those events.

Virtual worlds therefore cannot work with legacy modules. They need modules that listen to Blue Arcade's own event stream, which is the same thing cross platform modules need. One design solves both.

## Using Universal Modules

Universal modules install and behave like any other module. Put the `.bamodule` file in the modules folder, or use the module commands:

```text
/baa module list
/baa module info [module_id]
/baa module reload
```

They appear in `/baa module list` next to legacy modules, are added to arenas the same way, and use the same settings, language and achievement files.

Because they are experimental, run them on a test arena first and report anything that behaves differently from the legacy version.

## Creating a Universal Module

Universal modules are authored with `bacli`, a standalone command line tool that scaffolds, validates, builds and tests a module without needing the Blue Arcade source:

```text
bacli init
bacli check
bacli build
bacli test
```

A module is a folder with a `module.toml` manifest, Lua sources under `src/`, and its settings and language files under `resources/`. Full authoring documentation will follow as the format leaves the experimental stage.
