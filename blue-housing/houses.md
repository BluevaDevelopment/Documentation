# Houses

A house is a world, a folder and a set of settings. Houses are **not** kept in memory: they load on demand and unload once nobody is inside.

## Creating

```
/h create               # opens the template picker
/h create <template>    # skips it
```

Each house gets a persistent slot number for its owner and a nanoid that never changes. Both work as an address:

```
/h goto 2               # your own slot 2
/h goto Whiron 1        # somebody else's slot 1
```

## Templates

A template is a world folder plus a `template.json`. Two ship with the plugin:

| Template | What it is |
|----------|------------|
| `void` | An empty world with a small glass platform |
| `default` | A ready-made housing island with a Carpenter NPC |

Both are regenerated on start if missing, so deleting one gets it back.

### Making your own

```
/ha template create <id>        # from the world you are standing in
/ha template edit <id>          # opens it for editing
/ha template save <id>          # saves what you changed
/ha template cancel <id>        # discards it
/ha template setdisplay <id> <name>
/ha template setdesc <id> <text>
/ha template flag <id> <flag> <value>     # default flags for houses made from it
/ha template npc <id>                     # where the Carpenter stands (or `clear`)
/ha template delete <id>
/ha template list
```

Template ids are lowercase `a-z0-9_`.

## Lifecycle

A house loads when somebody enters and unloads once it has been empty for `world_unloading.empty_grace_seconds` (60 by default). The grace exists because unloading copies the world to disk, so without it stepping out and back in copies the whole house twice.

Anything created created in the world at runtime (holograms, NPCs, particle emitters) is destroyed on unload and rebuilt from saved data on load. Blocks you place stay, entities the plugin draws do not exist until they are drawn again.

## Inventories

Each house keeps its own inventory in its `player_data.json`. The inventory a player carries **outside** houses lives in `data/players/<uuid>.json` and is handed back on the way out.

That outside snapshot is the fragile one, so it is guarded: a snapshot is marked pending until it is restored, a second save refuses to overwrite a pending one, and the last ten distinct snapshots are kept under `default_inventory.backups` so a lost inventory is recoverable by hand.

## Reset

```
/h reset
```

Puts the house back to the day it was made: the build **and** everything arranged around it: NPCs, holograms, emitters, music, regions, roles, flags, the scoreboard and every script.

`backups/` is the one thing a reset does not touch, because a backup is the only thing that can undo one.

## Delete

```
/h delete           # opens the picker
/h confirm          # confirms
```

Deleting frees the slot. With `trash.enabled` the folder is moved to `data/trash/` instead of being removed, so a mistake is recoverable.

## Backups

```
/h backup create            # snapshot this house
/h backup list              # opens the menu: left click restores, right click deletes
```

An archive carries the world **and** the house: `house.json`, `player_data.json` and `scripts/`. Restoring replaces each of them, so the NPCs of the restored build are the ones that belong to it. An archive taken before this feature restores its world and leaves the rest alone.

Restoring walks everybody out first and lets them back in afterwards, because the files cannot be replaced under a world the server still has open.

Scheduled backups are configured under `backups:` in `settings.yml`.

### Why archives are small

Minecraft never reclaims the space a chunk leaves behind when it grows past its slot in a region file. Archiving zeroes those unreachable sectors without touching the header or anything it points at, so a restored world is the same world. Measured saving: 0% on a fresh world, ~26% with some building, ~60% after months, ~75% on heavy WorldEdit churn.

## Selling a House

```
/h economy sale offer <player> <amount>
/h economy sale accept <player>
/h economy sale cancel
```

The buyer pays, the folder moves, and the house is theirs with everything in it. A house whose files are on another server is refused before any money moves.

## The House Menu

`/h` inside your own house opens `main.yml`: flags, regions, scoreboard, scripting, backups, heads, holograms, NPCs, particles, music, members, roles, whitelist, moderation and economy, in three labelled rows.

Everyone else, operators included, gets Discovery instead. Admins manage other people's houses through `/ha menu <player|house>`, which names the house rather than guessing it from where the admin is standing.
