# Decorations

Holograms, NPCs, particle emitters and the head shop.

> Houses unload. Everything on this page is stored in `house.json` and rebuilt when the house loads. Nothing here survives in the world by itself.

## Holograms

```
/h hologram create <text>
/h hologram list
/h hologram move <id>
/h hologram addline <id> <text>
/h hologram setline <id> <n> <text>
/h hologram removeline <id> <n>
/h hologram remove <id>
```

Drawn with packets, so a hologram does not exist for a player until it is sent to them. Lines are anchored at the **bottom**, so adding one grows upwards from where you placed it.

Limit: the highest `bluehousing.house.hologram.limit.<n>` the player holds, falling back to `decorations.holograms.max_per_house`.

## NPCs

```
/h npc create <name>
/h npc list
/h npc move <id>
/h npc skin <id> <player|url>
/h npc hologram <id> <text>
/h npc action <id>
/h npc message <id> <text>
/h npc command <id> <command>
/h npc remove <id>
```

Equipment, poses, glow, entity type and paths are edited from the NPC menu. On Bedrock the nine screens collapse into a single form, except equipment (which needs items from an inventory) and paths (which need points picked in the world).

Limit: `bluehousing.house.npc.limit.<n>`, falling back to `decorations.npcs.max_per_house`.

### The Carpenter

Every new house is seeded with one NPC. It is ordinary decoration data, editable and deletable like any other, and seeding is skipped for a house that already has NPCs.

Where it stands comes from `world.default_npc` in the template (`/ha template npc <id>`); without one it goes two blocks in front of the spawn.

### If decorations look wrong

They are packets, and a client that is still rebuilding its world can drop one for good. Every arrival (join, world change, respawn) sends them after `decorations.show_delay_ticks` (5) and again after `decorations.show_retry_ticks` (20). Raise both on a server where players arrive into heavy chunk loading.

## Particles

```
/h particle create <effect>
/h particle list
/h particle types
/h particle move <id>
/h particle range <id> <blocks>
/h particle toggle <id>
/h particle remove <id>
```

46 named effects, placed on a block. Particles go out **per player**, filtered by each emitter's range, and one task draws every emitter of every house, so a house with nobody inside is skipped whole.

Limit: `bluehousing.house.particle.limit.<n>`.

## Decorative Heads

```
/h heads                     # the category menu
/h heads menu
/h heads library             # heads you own
/h heads categories
/h heads search <text>
/h heads buy <id>
/h heads claim <id>
/h heads list
/ha heads reload             # re-fetch the catalogue
```

Thousands of heads, loaded from remote JSON endpoints: no heads ship in the JAR and no extra plugin is needed. `/h heads` opens the **categories** first, because landing on whichever category sorts first showed a wall of letters to somebody looking for furniture.

### Caching

Lookup order is memory → disk → network → **disk again at any age**, because an old list of heads beats an empty shop.

`decorations.heads.disk_cache_hours` defaults to `0`, meaning it never expires. A catalogue of decorative heads is not news, and the source asks API users to keep requests down. Ask for new heads deliberately with `/ha heads reload`.

### Placement protection

A broken head drops an ordinary player head with nothing on it that says where it came from, so without a check the heads could be broken out of somebody's build, carried home and placed there, which makes the shop optional.

With `decorations.heads.placement_protection` on, a head **this server's shop has sold** may only be placed by somebody who bought it. Inside a house the owner's purchases count too, so a trusted builder can arrange the heads that house paid for; carrying them home still fails.

A head from `/give`, from another plugin, or a player's own skin is not the shop's business and is placed without a word. `bluehousing.house.heads.place.any` and admin mode place anything.
