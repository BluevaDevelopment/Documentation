# Global Worlds

Global worlds are server-wide worlds managed by administrators. They are accessible to all players and are ideal for lobbies, arenas, resource worlds, events, and more.

## Overview

- Managed by administrators.
- Support Nether and End dimension linking.
- Can be created from templates or generated fresh.
- Persistent and available to all players.

## Creating a Global World

### From a Template

```
/gw create template <world_name> <template_name>
```

**Examples:**
```
/gw create template spawn void
/gw create template resource survival_template
```

### Generate a New World

```
/gw create new <world_name>
```

**Example:**
```
/gw create new spawn
```

### Import an Existing World

```
/gw import <world_name>
```

**Example:**
```
/gw import existing_world
```

## Navigating Global Worlds

### Teleport to a Global World

```
/gw goto <world_name>
```

### List Global Worlds

```
/gw list
```

### View World Info

```
/gw info <world_name>
```

## Managing Global Worlds

### Delete a Global World

```
/gw delete <world_name>
```

### Reset a Global World

Reset the world back to its template, or to a different template:

```
/gw reset <world_name> [template_name]
```

## Dimension Linking

Link Nether and End dimensions to a global world for full survival gameplay.

### Auto-link Dimensions

```
/gw link <world> --all
```

**Example:**
```
/gw link world --all
```

### Manual Linking

```
/gw link <world> nether <nether_world>
/gw link <world> end <end_world>
```

**Examples:**
```
/gw link world nether world_nether
/gw link world end world_the_end
```

### View Links

```
/gw link <world> info
```

### Unlink Dimensions

```
/gw link <world> unlink nether
/gw link <world> unlink end
/gw link <world> unlink --all
```

## World Flags

Set flags on global worlds:

```
/gw flag set <flag> <value> -w <world>
```

**Examples:**
```
/gw flag set pvp true -w world
/gw flag set explosion false -w spawn
/gw flag set gamemode SURVIVAL -w world
/gw flag set spawn here -w spawn
```

See [World Flags](world-flags.md) for the full list of available flags.

## Common Global World Setups

### Protected Spawn

```
/gw create template spawn hub_template
/gw flag set break false -w spawn
/gw flag set place false -w spawn
/gw flag set pvp false -w spawn
/gw flag set gamemode ADVENTURE -w spawn
/gw flag set food false -w spawn
/gw flag set spawn here -w spawn
```

### Resource World

```
/gw create template resource survival_template
/gw flag set pvp true -w resource
/gw flag set gamemode SURVIVAL -w resource
/gw flag set difficulty NORMAL -w resource
```

### Full Survival World with Dimensions

```
/gw create template world survival_template
/gw link world --all
/gw flag set pvp true -w world
/gw flag set gamemode SURVIVAL -w world
/gw flag set difficulty NORMAL -w world
```

## Troubleshooting

**"World already exists"**
- Use a different name.
- Delete the existing world first if it is no longer needed.

**"Template not found"**
- Verify the template exists with `/bw template list`.
- Check spelling and case sensitivity.

**"Cannot link dimensions"**
- Make sure the target Nether/End world exists.
- Use `/gw link <world> info` to check current links.
