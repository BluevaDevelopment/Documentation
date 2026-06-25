# Template System

Blue Worlds uses a **template-based system** to create worlds. Instead of generating terrain on demand, worlds are copied from pre-built templates, giving better performance, consistency, and resource efficiency.

## Why Use Templates?

- **Better Performance**: Pre-generated terrain loads faster.
- **Consistency**: All worlds from the same template are identical.
- **Resource Efficiency**: No chunk generation lag.
- **Customization**: Create custom templates with pre-built structures.

## Template Storage

Templates are stored in `plugins/BlueWorlds/data/templates/` as ZIP archives. A template can include:

- Main world data
- Hidden sub-templates for linked dimensions (`<name>_nether`, `<name>_end`)
- `settings.json` with world flags, seed, generator, and environment

## Creating Templates

### From an Existing World

Stand in the world you want to use, or specify it by name:

```
/bw template create <template_name> [world_name] [-f]
```

**Examples:**
```
/bw template create void
/bw template create skyblock my_skyblock_world
/bw template create island world -f
```

Arguments:
- `<template_name>` - Name for the new template
- `[world_name]` - Source world (defaults to current world)
- `[-f]` - Force overwrite if template exists

### Import from a Folder

If you have a pre-built world folder:

```
/bw template import <folder> <template_name> [-f]
```

**Example:**
```
/bw template import world_templates/custom_island my_island
```

### Copy a Template

```
/bw template copy <source_template> <destination_template> [-f]
```

**Examples:**
```
/bw template copy skyblock skyblock_v2
/bw template copy island island_backup -f
```

## Managing Templates

### List Templates

```
/bw template list
```

### View Template Info

```
/bw template info <template_name>
```

### Export a Template

```
/bw template export <template_name> [folder]
```

**Example:**
```
/bw template export skyblock
```

### Delete a Template

```
/bw template delete <template_name> [-f]
```

## Protected Default Templates

The following default template names are protected and cannot be overwritten:

- `normal`
- `nether`
- `end`
- `void`

## Common Template Ideas

- **Void World**: Empty world for creative/skyblock
- **Flat World**: Flat terrain for building
- **Island**: Pre-built island for skyblock-style gameplay
- **Plot**: Single plot with protection
- **Arena**: PvP or minigame arena
- **Hub**: Spawn area with buildings

## Best Practices

1. **Keep templates small** - use `/worldborder set` before creating.
2. **Pre-configure gamerules and spawn points** in the source world.
3. **Include starter chests** for skyblock-style templates.
4. **Version templates** (`skyblock_v1`, `skyblock_v2`) to keep backups.
5. **Test templates** before making them available to players.

## Troubleshooting

**"Template already exists"**
- Use a different name.
- Use `-f` to force overwrite if you are sure.

**"World not found"**
- Verify the source world name with `/worlds` or server console.
- Make sure the world is loaded.

**"Cannot delete protected template"**
- Default templates (`normal`, `nether`, `end`, `void`) are protected.
