# BlueHousing Lua API for LLMs

A single self-contained document for generating BlueHousing house scripts. Everything needed is here; no other page has to be read.

**Not linked from the documentation site. This page exists to be handed to a language model.**

---

## 1. What you are writing

A house script is Lua that runs inside the BlueHousing plugin on a Minecraft (Spigot/Paper) server. Each house is a private world with one owner. A script only ever affects **its own house**: there is no way to reach another house, the console, or the server at large.

Files live in projects: `scripts/<project>/main.lua` plus any number of flat `.lua` files, all of which run when the house loads (`main.lua` first, then the rest alphabetically). Handlers may live in any file.

Output is plain Lua. Nothing is imported: `housing` and the player handle are already there.

```lua
housing.on("house_enter", function(event)
    event.player:message("<green>Welcome!")
end)
```

## 2. Hard rules

These are the things that are wrong most often. Follow them literally.

1. **Lua 5.5.** `/` always returns a float, so `10/2` is `5.0` and not `5`. Use `//` for integer division. `bit32`, `math.pow`, `math.ldexp` and `math.frexp` do not exist. `utf8` does.
2. **Methods on a player use a colon**: `player:message(...)`, never `player.message(...)`. Functions on `housing` use a dot: `housing.broadcast(...)`.
3. **There is no `print`.** Use `housing.log(text)`, which goes to the project console in the web editor, not to the server console.
4. **No `os`, `io`, `require` of anything outside the project, no network, no file access.** `require("utils")` loads `utils.lua` **from the same project** and nothing else.
5. **No console commands.** There is no binding that runs anything as the server. `player:runCommand(cmd)` runs as *that player* and is disabled by default.
6. **Text is MiniMessage**: `<red>`, `<bold>`, `<gradient:#EC4899:#F43F5E>`. Not `§` or `&` codes.
7. **Never loop forever.** There is an instruction limit per event; a script that exceeds it is stopped, and while it is failing `block_break`, `block_place` and `interact` are **denied** for everyone in the house. Use `housing.delay` / `housing.every` instead of busy-waiting.
8. **Events fire only for things inside the house.** There is no server-wide event, no `player_join`, no `player_quit`. Use `house_enter` / `house_leave`, which also cover logging in and out inside the house.
9. **Coordinates are the house world's.** `player:teleport(x, y, z)` cannot leave it.
10. **Actions on an offline player do nothing** rather than erroring; getters still work off a snapshot. There is no `player:isOnline()`.
11. **Persistent variables are per project.** Two projects may use the same key without colliding.

## 3. Events

```lua
housing.on("<event>", function(event) ... end)
```

`event.player` is always the player it happened to. Cancellable events have `event.cancel()`.

| Event | Extra fields | Cancellable |
|---|---|---|
| `house_enter` / `house_leave` | - | no |
| `player_move` | - | no |
| `player_jump` | - | no |
| `region_enter` / `region_leave` | `region` | no |
| `player_chat` | `message` | no |
| `block_break` / `block_place` | `block` | yes |
| `block_damage` | `block` | yes |
| `interact` | `click` (`"left"`/`"right"`), `target` (`"air"`/`"block"`), `item?`, `block?` | yes |
| `interact_entity` | `entity` | yes |
| `player_damage` | `amount`, `cause` | yes |
| `player_attack` | `victim`, `amount` | yes |
| `player_death` | `victim`, `killer?` | no |
| `player_kill` | `victim`, `killer` | no |
| `player_respawn` | - | no |
| `player_sneak` | `sneaking` | no |
| `player_swap_hands` | `item?` | yes |
| `player_toggle_flight` | `flying` | yes |
| `item_consume` / `item_drop` / `item_pickup` | `item` | drop/pickup only |
| `item_select` | `slot` (0-8) | yes |
| `inventory_click` | `slot`, `item?`, `inventory_type` | yes |
| `inventory_close` | `inventory_type` | no |
| `entity_death` | `entity`, `killer?` | no |
| `projectile_hit` | `projectile`, `block?`, `entity?` | no |
| `player_fish` | `state` | no |
| `player_teleport` | `from`, `to` | yes |
| `player_command` | `message`, `command`, `args` | yes |
| `sign_change` | `block`, `lines` (4) | yes |
| `bucket_fill` / `bucket_empty` | `block`, `bucket` | yes |
| `vehicle_enter` / `vehicle_exit` | `vehicle` | yes |
| `npc_click` | `npc` (id), `click` | no |

Field shapes:

```
block  = {x, y, z, material}        item   = {material, amount}
entity = {type, name?, uuid}        from/to = {x, y, z, world}
```

Custom events between projects of the same house:

```lua
housing.emit("my_event", { any = "table" })
housing.on("my_event", function(event) ... end)
```

## 4. Scheduling

```lua
local id = housing.delay(20, function() ... end)      -- once, in ticks (20 = 1s)
local id = housing.every(100, function() ... end)     -- repeating
housing.cancel(id)
```

Never `while true do end`.

## 5. Custom commands

```lua
housing.command("start", function(event)
    event.player:message("<green>Starting…")
    local first = event.args[1]
end)
```

The handler receives an **event**, like every other handler, not the player.
Players run it with `/h run start`; extra words arrive as `event.args[1]`,
`event.args[2]`, and so on. Command names are shared by every project of the
house.

## 6. Complete API

Signatures are exact. Square brackets mark optional arguments.

## housing.*

| Signature | Area | Description |
|---|---|---|
| `housing.role_can(role_name, flag)` | Roles | `true`/`false` for what a role of this house may do. An override answers directly; a role that inherits gets the house's own flag. Fails closed: an unknown role, a name that is not a flag, and a flag no role can override are all `false`. `'while the owner is online'` resolves against the owner being online right now. The role name lookup is case-insensitive. |
| `housing.playerVar(target, key [, default])` | Player variables | Reads any player's variable. `target` is a uuid, a name, or a player handle. |
| `housing.setPlayerVar(target, key, value)` | Player variables | Writes it. |
| `housing.top(key [, limit])` | Player variables | The players with the highest numeric value for `key`, best first: `{ {uuid, name, value, rank}, ... }`. `limit` defaults to 10, maximum 100. |
| `housing.broadcast(text)` | housing functions | MiniMessage to everyone in the house. |
| `housing.players()` | housing functions | Array of player handles currently in the house. |
| `housing.playerCount()` | housing functions | Number of players in the house. |
| `housing.lightning(x, y, z)` | housing functions | Visual lightning strike (no damage). |
| `housing.dropItem(x, y, z, material, amount)` | housing functions | Drop an item in the world. |
| `housing.random(min, max)` | housing functions | Random integer, both ends inclusive. |
| `housing.time()` | housing functions | Time of the house world (0-24000). |
| `housing.now()` | housing functions | Milliseconds since the epoch, the only wall clock a script has. |
| `housing.fill(x1, y1, z1, x2, y2, z2, material [, replacing])` | housing functions | Sets every block of a cuboid and returns how many changed. With `replacing`, only blocks of that material are touched. Capped at 200 000 blocks per call. |
| `housing.fillRegion(name, material [, replacing])` | housing functions | The same over a named region, one drawn with `/h region` or declared with `housing.region(...)`. |
| `housing.spawn(type, x, y, z [, name])` | housing functions | Spawns an entity and returns a handle. Players are refused (use `housing.npc()`), and a house is capped at 200 entities. Spawned entities are runtime state: they are gone when the house unloads, so spawn them again on `house_enter` if you want them back. |
| `housing.entities([type])` | housing functions | Array of handles for the non-player entities in the house, optionally filtered by type. |
| `housing.getBlock(x, y, z)` | housing functions | Material name of that block. |
| `housing.setTime(ticks)` | housing functions | Sets the time of the house world (0-23999, wraps). |
| `housing.weather()` | housing functions | `"clear"`, `"rain"` or `"thunder"`. |
| `housing.setWeather(state)` | housing functions | One of `"clear"`, `"rain"`, `"thunder"`. |
| `housing.sound(x, y, z, name [, volume, pitch])` | housing functions | Plays a sound at a point, for everyone nearby. |
| `housing.particle(name, x, y, z [, count])` | housing functions | Spawns particles at a point, for everyone in the house. |
| `housing.particleEffects()` | housing functions | The ids of the 46 named effects (`halo`, `volcano`, `spiral`, `lovewell`, …). |
| `housing.playEffect(effect, x, y, z [, range])` | housing functions | Draws **one frame** of a named effect, for the players within `range`. Nothing is stored, so this is how you animate something that moves. |
| `housing.particleEmitter(id, effect, x, y, z [, range])` | housing functions | Places a **permanent** emitter: it persists in the house, survives an unload and appears in `/h particle list`. Calling again with the same id moves or re-points it. |
| `housing.removeParticleEmitter(id)` | housing functions | Returns `true` if it existed. |
| `housing.songs()` | housing functions | Ids of the note-block songs the server has installed. |
| `housing.song()` | housing functions | The song this house plays, or `nil`. |
| `housing.setSong(id [, options])` | housing functions | Sets it. `options` = `{volume, speed, loop, auto_play}`. Pass `nil` as the id to turn music off. Applies to everyone inside immediately. |
| `housing.particleEmitters()` | housing functions | Ids of the emitters **this scripting API created**; what the owner placed by hand is not listed. |
| `housing.region(id)` | housing functions | Returns `{x1, y1, z1, x2, y2, z2}` or `nil`. Looks in the script-declared regions first, then in the ones the owner drew with `/h region`. |
| `housing.regions()` | housing functions | Array of the region ids drawn with `/h region`. |
| `housing.region_at(x, y, z)` | housing functions | Id of the highest-priority house region covering that block, or `nil`. |
| `housing.region_flag(id, flag)` | housing functions | The raw value a house region overrides for a flag, or `nil` when it does not override it. |
| `housing.holograms()` | housing functions | Ids of the holograms/NPCs **this scripting API created**. What the owner placed by hand is deliberately not listed: a script has no business renaming or deleting it. |
| `housing.npcs()` | housing functions | Ids of the holograms/NPCs **this scripting API created**. What the owner placed by hand is deliberately not listed: a script has no business renaming or deleting it. |
| `housing.npcSkin(id, playerName)` | housing functions | Gives a script-created NPC somebody's skin, exactly as `/h npc skin` does. Pass `nil` for the default skin. |
| `housing.warp(id)` | housing functions | Returns `{x, y, z, yaw, pitch}` or `nil`. |
| `housing.houseId()` | housing functions | This house's id. |
| `housing.owner_online()` | housing functions | Whether the house owner is currently online. |
| `housing.var(key [, default])` | housing functions | Persistent **project** variables (shared by all players of the house, scoped to this project). |
| `housing.setVar(key, value)` | housing functions | Persistent **project** variables (shared by all players of the house, scoped to this project). |
| `housing.delay(ticks, fn)` | housing functions | Run `fn` once after `ticks` ticks / repeatedly. Both return a task id. |
| `housing.every(ticks, fn)` | housing functions | Run `fn` once after `ticks` ticks / repeatedly. Both return a task id. |
| `housing.cancel(taskId)` | housing functions | Stop a delayed/repeating task. |
| `housing.cooldown(key, seconds)` | housing functions | House-wide cooldowns. See Cooldowns. |
| `housing.cooldownLeft(key)` | housing functions | House-wide cooldowns. See Cooldowns. |
| `housing.clearCooldown(key)` | housing functions | House-wide cooldowns. See Cooldowns. |
| `housing.economy()` | housing functions | `"off"`, `"read"`, `"owner"` or `"free"`, saying what a script may do with money here. See Economy. |
| `housing.formatMoney(amount)` | housing functions | Renders an amount with the server's currency settings. |
| `housing.bossBar(spec)` | housing functions | A boss bar for everyone in the house. See Boss bar. |
| `housing.removeBossBar([id])` | housing functions | A boss bar for everyone in the house. See Boss bar. |
| `housing.flag(name)` | Flags | Current value: `boolean`, `number` or `string` depending on the flag type (gamemode/difficulty come back as `"CREATIVE"`, `"NORMAL"`, ...). `nil` if the flag has no value. |
| `housing.setFlag(name, value)` | Flags | Set the flag. The value is coerced to the flag type: booleans for on/off flags, numbers for integer flags, strings for text flags, `"SURVIVAL"`/`"CREATIVE"`/... for `gamemode`, `"PEACEFUL"`/`"EASY"`/... for `difficulty`, a number or `"auto"` for `world-border`. Changes apply immediately to the world and to the players inside it. Invalid values (and values the server rejects, e.g. a world-border above the house's unlocked limit) raise an error. |
| `housing.clearFlag(name)` | Flags | Remove the custom value and fall back to the default. |
| `housing.scoreboardTitle()` | Scoreboard | Current custom title (`nil` = the server default is shown). |
| `housing.scoreboardLines()` | Scoreboard | Current custom lines as an array (empty = the server defaults are shown). |
| `housing.hologram(id, x, y, z, lines)` | Holograms | Creates a hologram, or moves and relabels it if that id already exists. `lines` is an array of MiniMessage strings (max 15 lines, 128 characters each). |
| `housing.removeHologram(id)` | Holograms | Deletes it. Does nothing if there is no such hologram. |
| `housing.npc(id, x, y, z [, name])` | NPCs | Creates an NPC, or moves and renames it if that id already exists. |
| `housing.removeNpc(id)` | NPCs | Deletes it. Does nothing if there is no such NPC. |
| `housing.ban(playerOrUuid [, reason])` | Moderation | Permanently ban a player from the house (max 200 characters of reason, default `"No reason specified"`). If the player is online and inside the house they are ejected to the main spawn with a notification, exactly like `/h ban`. Banning the house owner raises an error. |
| `housing.unban(playerOrUuid)` | Moderation | Remove the ban. Returns `true` if a ban existed. |
| `housing.isBanned(playerOrUuid)` | Moderation | Returns `true` if the player is banned. |
| `housing.tempban(target, seconds [, reason])` | Moderation | A ban that lifts itself, written exactly as `/h tempban` writes it. Ejects them the same way. |
| `housing.mute(target [, seconds, reason])` | Moderation | Mutes them in the house. Without a duration the mute is permanent. |
| `housing.unmute(target)` | Moderation | Returns `true` if they were muted. |
| `housing.role(target)` | Roles and access | The role that player holds here (`owner`, `trusted`, `added`, `visitor`, or a role your scripts registered). |
| `housing.setRole(target, role [, temporary])` | Roles and access | Assigns it. A **temporary** role lasts until they leave the house; a persistent one is what `/h role` and `/h trust` write. The owner's role cannot be changed. |
| `housing.removeRole(target)` | Roles and access | Returns `true` if they had one. |
| `housing.members()` | Roles and access | Everyone with a stored role in this house, online or not: `{ {uuid, name, role}, ... }`. |
| `housing.whitelist()` | Roles and access | Whether the house is closed to visitors. |
| `housing.setWhitelist(enabled)` | Roles and access | Opens or closes it. |
| `housing.whitelistAdd(name)` | Roles and access | By name, as `/h whitelist` takes it. |
| `housing.whitelistRemove(name)` | Roles and access | By name, as `/h whitelist` takes it. |
| `housing.whitelisted()` | Roles and access | The names on the list. |
| `housing.createRegion(id, x1, y1, z1, x2, y2, z2 [, priority])` | Regions drawn in game (/h region) | Creates or moves it. Ids are `[a-z0-9_-]`, up to 24 characters. Redrawing an existing region **keeps the flags** its owner already set on it; the script only moved the walls. |
| `housing.deleteRegion(id)` | Regions drawn in game (/h region) | Returns `true` if it existed. |
| `housing.setRegionFlag(id, flag, value)` | Regions drawn in game (/h region) | Overrides a flag inside it; pass `nil` to clear the override. Only region-capable flags are accepted, and asking for a house-wide one is an error rather than a silent no-op. |

## player:*

| Signature | Area | Description |
|---|---|---|
| `player:region()` | Roles | Id of the highest-priority house region the player is standing in, or `nil`. |
| `player:regions()` | Roles | Array of every house region covering the player, highest priority first. |
| `player:in_region(id)` | Roles | Whether the player is inside that house region. |
| `player:role()` | Roles | The player's role name in this house: `"owner"`, a role assigned with `/h role` (custom names included), or `"visitor"`. No owner-online rule applied. |
| `player:can_build()` | Roles | `true` when the player may build under the **classic** rule (owner and trusted always, added while the owner is online). Custom roles count as visitors here: the classic rule does not know what they mean. |
| `player:name()` | Messaging | Name / UUID (snapshotted). |
| `player:uuid()` | Messaging | Name / UUID (snapshotted). |
| `player:message(text)` | Messaging | Chat message. Supports MiniMessage (`<red>`, `<bold>`, ...). |
| `player:title(title, subtitle [, fadeIn, stay, fadeOut])` | Messaging | Title + subtitle, times in ticks (default 10/70/20). |
| `player:actionbar(text)` | Messaging | Action bar message (MiniMessage). |
| `player:sound(name [, volume, pitch])` | Effects & movement | Play a sound (default volume 1, pitch 1). Names like `ENTITY_PLAYER_LEVELUP`. |
| `player:particle(name, x, y, z [, count])` | Effects & movement | Spawn particles (default count 10). Names like `HAPPY_VILLAGER`. |
| `player:teleport(x, y, z [, yaw, pitch])` | Effects & movement | Teleport inside the house world. Keeps current yaw/pitch if omitted. |
| `player:warp(id)` | Effects & movement | Teleport to a warp registered with `housing.warp(...)`. Errors if the warp does not exist. |
| `player:give(material, amount)` | Inventory | Give items (`"DIAMOND"`, `"minecraft:diamond"` and `"diamond"` all work). |
| `player:removeItem(material, amount)` | Inventory | Remove up to `amount` items of that material. |
| `player:yaw()` | Inventory | Where the player is looking, in degrees. |
| `player:pitch()` | Inventory | Where the player is looking, in degrees. |
| `player:push(x, y, z)` | Inventory | Adds to the player's velocity, for jump pads and knockback. Each axis is capped at ±5. |
| `player:velocity()` | Inventory | `{x, y, z}` of the current velocity. |
| `player:setHealth(amount)` | Inventory | Sets health, clamped to the player's maximum. |
| `player:maxHealth()` | Inventory | The player's maximum health. |
| `player:setFood(amount)` | Inventory | Sets the food bar (0-20). |
| `player:level([amount])` | Inventory | Reads the experience level, or sets it when given one. |
| `player:isOnGround()` | Inventory | Movement state. |
| `player:isFlying()` | Inventory | Movement state. |
| `player:isSprinting()` | Inventory | Movement state. |
| `player:allowFlight(enabled)` | Inventory | Grants or removes flight (also stops flying when disabled). |
| `player:walkSpeed([speed])` | Inventory | Reads or sets the speed (-1..1). |
| `player:flySpeed([speed])` | Inventory | Reads or sets the speed (-1..1). |
| `player:heldItem()` | Inventory | The item in the main hand, or `nil`. See the item shape below. |
| `player:armor(piece [, material, options])` | Inventory | Reads a piece with one argument, replaces it with two. `piece` is `helmet`, `chestplate`, `leggings`, `boots` or `offhand`; pass `nil` as the material to clear it. |
| `player:inventory()` | Inventory | Every occupied slot as `{ [slot] = item }`. |
| `player:ask(title, callback [, placeholder])` | Inventory | Asks the player for text (anvil, chat as a fallback) and calls `callback(answer)`. The answer is `nil` when they cancel. |
| `player:setSlot(slot, material [, options])` | Inventory | Puts an item in an exact inventory slot (`0-8` hotbar, `9-35` bag). `options` is the item option table below. A locked item cannot be moved, dragged, swapped or dropped by the player, which is what keeps a per-role item in its slot instead of being duplicated. Pass `nil` as the material to empty the slot. |
| `player:getSlot(slot)` | Inventory | The item in that slot, or `nil` when it is empty. |
| `player:hasItem(material [, amount])` | Inventory | Returns `true` if the player has at least `amount` (default 1). |
| `player:menu(spec)` | Menus | **Both platforms.** Describe a list of buttons once; Java renders a chest, Bedrock a form. Use this unless you need something the other platform cannot show. |
| `player:chestMenu(spec)` | Menus | **Java's chest, in full.** Explicit rows, a slot per icon, a filler, and the click type reported back. A Bedrock player still sees it, through Geyser's own chest translation. |
| `player:form(spec)` | Menus | **Bedrock's form, in full.** Simple, modal or custom, with dropdowns, sliders, toggles and text fields. Returns `false` for a Java player, who cannot be shown one. |
| `player:isBedrock()` | Menus | `true` for a Bedrock player, so a script can branch between the two above. |
| `player:openMenu(id)` | Menus | Opens one of the plugin's own house menus, e.g. `"main"` for the `/h` menu. Errors when no menu has that id. |
| `player:playSong([id])` | Music | Starts the house song for this listener, or a named one. |
| `player:stopSong()` | Music | Stops whatever they are hearing. |
| `player:playEffect(effect, x, y, z)` | Particles | One frame of a named effect, for this player only. |
| `player:cooldown(key, seconds)` | Cooldowns | Returns `true` when it was ready, **and starts it**. Returns `false` when it is not. |
| `player:cooldownLeft(key)` | Cooldowns | Seconds left, `0` when ready. |
| `player:clearCooldown(key)` | Cooldowns | Makes it ready again. |
| `player:balance()` | Economy | Their balance, or `nil` when the server has no economy. |
| `player:charge(amount [, reason])` | Economy | Takes the money. `false` when they could not pay; nothing is taken in that case. |
| `player:pay(amount [, reason])` | Economy | Gives them money. `false` when the server does not allow it (see below). |
| `player:bossBar(spec)` | Boss bar | Shows or updates a bar for this player. |
| `player:removeBossBar([id])` | Boss bar | Removes it. |
| `player:gamemode()` | State | Returns `"SURVIVAL"`, `"CREATIVE"`, ... |
| `player:heal()` | State | Restore full health. |
| `player:feed()` | State | Full hunger bar. |
| `player:damage(amount)` | State | Deal damage (hearts × 2). |
| `player:setFire(ticks)` | State | Set on fire for `ticks` ticks (20 ticks = 1 second). |
| `player:effect(name, durationTicks, amplifier)` | State | Potion effect. Accepts `SPEED`, `speed` or `minecraft:speed`. Amplifier 0 = level I. |
| `player:kick([reason])` | State | Disconnect the player from the server. Reason supports MiniMessage (default: a generic "kicked from this house" message). |
| `player:runCommand(cmd)` | State | Run a command **as the player** (without `/`). Disabled by default: the server owner must set `scripting.player_commands: true` in settings.yml, otherwise this raises an error. |
| `player:health()` | Getters | Current health, 0-20. |
| `player:food()` | Getters | Current food level, 0-20. |
| `player:x()` | Getters | Position inside the house world. |
| `player:y()` | Getters | Position inside the house world. |
| `player:z()` | Getters | Position inside the house world. |
| `player:isSneaking()` | Getters | Whether the player is sneaking. |
| `player:world()` | Getters | The world name they are in. |
| `player:var(key [, default])` | Player variables | Reads one of this player's variables. `default` is returned when it has never been set. |
| `player:setVar(key, value)` | Player variables | Writes it. Values may be a string, a number or a boolean. |

---

## 7. Patterns

### Persistent state

```lua
-- Per house
local wins = housing.var("wins", 0)
housing.setVar("wins", wins + 1)

-- Per player, scoped to this project
local best = event.player:var("best_time", 0)
event.player:setVar("best_time", 12.4)

-- Leaderboard from a numeric player variable
for _, row in ipairs(housing.top("best_time", 10)) do
    housing.broadcast(row.rank .. ". " .. row.name .. " - " .. row.value)
end
```

### Regions

`housing.region(id, x1, y1, z1, x2, y2, z2)` declares a **script-local** region:
it fires `region_enter` / `region_leave` and disappears with the runtime.
`housing.createRegion(...)` draws a **real** one that persists, shows up in the
Regions menu and overrides flags. `region_enter` fires for both, and for regions
the owner drew with `/h region`.

```lua
housing.region("arena", 0, 60, 0, 20, 80, 20)

housing.on("region_enter", function(event)
    if event.region == "arena" then
        event.player:title("<red>Arena", "<gray>Good luck")
    end
end)
```

### Protecting the build

```lua
housing.on("block_break", function(event)
    if event.player:role() ~= "owner" then
        event.cancel()
        event.player:actionbar("<red>You may not break blocks here.")
    end
end)
```

### A menu

Three entry points. `player:menu(spec)` renders a chest on Java and a native form
on Bedrock from one description. Use it unless you need something only one
platform can show. `player:chestMenu(spec)` is Java's chest in full;
`player:form(spec)` is Bedrock's form in full and returns `false` for a Java
player. `player:openMenu(id)` is different: it opens one of the **plugin's own**
menus, such as `"main"`.

```lua
housing.on("npc_click", function(event)
    event.player:chestMenu({
        title = "<dark_gray>Shop",
        rows = 3,
        fill = "GRAY_STAINED_GLASS_PANE",
        items = {
            [13] = {
                material = "DIAMOND_SWORD",
                name = "<aqua>Sword",
                lore = { "<gray>100 coins" },
                on_click = function(click)
                    if click.player:charge(100) then
                        click.player:give("DIAMOND_SWORD", 1)
                        click.player:sound("ENTITY_PLAYER_LEVELUP")
                    else
                        click.player:message("<red>Not enough.")
                    end
                end,
            },
        },
    })
end)
```

`on_click` receives `{slot, click, shift, player}`, where `click` is the click
type in lower case (`left`, `right`, `shift_left`, ...).

### Cooldowns

```lua
if not event.player:cooldown("ability", 5) then
    event.player:actionbar("<red>Wait a moment.")
    return
end
```

### A timer that stops

```lua
local left = 10
local id
id = housing.every(20, function()
    left = left - 1
    housing.broadcast("<yellow>" .. left)
    if left <= 0 then housing.cancel(id) end
end)
```

## 8. Worked example: parkour with checkpoints

```lua
local CHECKPOINTS = {
    { id = "cp1", x = 10, y = 64, z = 5 },
    { id = "cp2", x = 30, y = 70, z = 5 },
}
local START = { x = 0, y = 64, z = 0 }

for i, cp in ipairs(CHECKPOINTS) do
    housing.region(cp.id, cp.x - 1, cp.y - 1, cp.z - 1, cp.x + 1, cp.y + 2, cp.z + 1)
end
housing.region("finish", 48, 74, 3, 52, 78, 7)

local function reset(player)
    player:setVar("cp", 0)
    player:setVar("started", 0)
    player:teleport(START.x, START.y, START.z)
end

housing.on("house_enter", function(event)
    reset(event.player)
    event.player:title("<gold>Parkour", "<gray>Reach the end")
end)

housing.on("region_enter", function(event)
    local player = event.player

    for index, cp in ipairs(CHECKPOINTS) do
        if event.region == cp.id and player:var("cp", 0) < index then
            player:setVar("cp", index)
            if index == 1 then player:setVar("started", housing.now()) end
            player:actionbar("<green>Checkpoint " .. index)
            player:sound("ENTITY_EXPERIENCE_ORB_PICKUP")
            return
        end
    end

    if event.region == "finish" and player:var("cp", 0) >= #CHECKPOINTS then
        local started = player:var("started", 0)
        local seconds = (housing.now() - started) // 1000
        local best = player:var("best", 0)
        if best == 0 or seconds < best then
            player:setVar("best", seconds)
            housing.broadcast("<gold>" .. player:name() .. " set a record: " .. seconds .. "s")
        else
            player:message("<green>Finished in " .. seconds .. "s")
        end
        reset(player)
    end
end)

housing.on("player_damage", function(event)
    if event.cause == "FALL" then
        event.cancel()
        local index = event.player:var("cp", 0)
        local cp = CHECKPOINTS[index]
        if cp then
            event.player:teleport(cp.x, cp.y, cp.z)
        else
            event.player:teleport(START.x, START.y, START.z)
        end
    end
end)

housing.command("reset", function(event)
    reset(event.player)
    event.player:message("<yellow>Back to the start.")
end)
```

## 9. Mistakes to avoid

| Wrong | Right |
|---|---|
| `housing.command("x", function(player) end)` | `function(event)`, then `event.player` and `event.args` |
| `print("hi")` | `housing.log("hi")` |
| `player.message("hi")` | `player:message("hi")` |
| `housing:broadcast("hi")` | `housing.broadcast("hi")` |
| `"§aHello"` or `"&aHello"` | `"<green>Hello"` |
| `local half = total / 2` for an index | `local half = total // 2` |
| `while true do ... end` | `housing.every(ticks, fn)` |
| `os.time()` | `housing.now()` (epoch ms). `housing.time()` is the world time, 0-24000 |
| `housing.on("player_join", ...)` | `housing.on("house_enter", ...)` |
| Running a command as the server | Not possible; use the API |

## 10. Checklist before returning a script

- Every `housing.on` name is in the table in section 3
- Every call matches a signature in section 6, with a `:` for player methods and a `.` for `housing`
- No infinite loops; delays and repeats go through `housing.delay` / `housing.every`
- All text is MiniMessage
- Integer maths uses `//`
- Nothing reaches outside the house
