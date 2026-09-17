# Lua API

Complete function reference for house scripts. For how scripting fits together, the
editor, projects and limits, see [Scripting](scripting.md).

Everything below is available to every house script; nothing has to be imported.

```lua
housing.on("house_enter", function(event)
    event.player:message("<green>Welcome to my house!")
end)
```

## Projects & files

A house does not have a single script: it has **one or more projects**. Each project is a
folder `scripts/<slug>/` inside the house data folder, with a `project.json`
(`{"name", "enabled", "position"}`) and any number of flat `.lua` files (no
subdirectories). When the project loads (world load, plugin start, or sync from the web
editor), **only `main.lua` runs**. Any other file runs when `main.lua`, or a file it
loads, asks for it with `require("name")`, and only once: a second `require` gets back
what the first one returned. A file nothing loads never runs; the editor marks it with a
warning and offers to add the `require` to `main.lua`.

- **Isolation:** every project gets its own Lua environment: own globals, event
  handlers, scheduled tasks, regions, warps, persistent variables and module cache.
  The only channel between projects of the same house is `housing.emit` (see
  "Permissions & the Default project"): a custom event reaches every project that
  handles it. Everything else stays isolated; the only other thing they share is the
  Minecraft world itself (and the cancellation of events, see below).
- **Ordering:** projects run in `(position, slug)` order. Events are dispatched to every
  enabled project of the house in that order.
- **Event cancellation:** cancellable events share one cancellation flag across all
  projects of the house, so if any project calls `event.cancel()`, the event is cancelled
  after every project has run its handlers.
- **`/h run` commands** live in one namespace per house. If two projects register the
  same command name, the project with the lowest position wins (a warning is logged when
  the house loads).
- **Persistent variables** (`housing.var`, `player:var`) are scoped to the project: two
  projects can use the same variable name without colliding.
- **`require(name)`** loads another file **from the same project** and returns what the
  module returns (or `true` if it returns nothing):

  ```lua
  -- utils.lua
  local M = {}
  function M.greet(player) player:message("<gray>hello!") end
  return M

  -- main.lua
  local utils = require("utils")   -- resolves utils.lua in this project
  ```

  Module names must be plain file names: no `.`, `/` or `\` (a trailing `.lua` is
  optional and ignored). The result is cached per project, so the file runs only once;
  circular requires raise `circular require: <name>`.

## Basics

- `housing.on(eventName, handler)`: register an event handler. You can register several
  handlers for the same event.
- `housing.emit(eventName [, data])`: fire a custom event **to every project of the
  house** (in project order), including your own. This is the one communication channel
  between the projects of a house (see "Permissions & the Default project"). Handlers
  receive `data` as their event table (`nil` when omitted); every project gets the same
  table reference. Event names from the engine catalog (the "Events" table) are
  reserved and cannot be emitted. Emits nested deeper than 20 levels raise an error
  (event loop guard), and one root fire invokes at most 1000 handlers across all
  projects (event storm guard).
- `housing.command(name, handler)`: register a house command, run as `/h run <name>`.
  The handler receives an event table with `player` and `args` (the words after the
  name).
- `housing.log(message)` / `print(...)`: write a line to the house Console in the web
  editor (see "Console & debugging").
- Every event handler receives a single `event` table. Common fields:
  - `event.player`: the player handle (see below).
  - `event.house`: the house id (nanoid).
- Cancellable events provide `event.cancel()`: call it inside the handler to cancel the
  Bukkit event (e.g. prevent a block from being broken).
- Errors inside a handler are reported in the Console and never crash the server or
  the other handlers.

## Console & debugging

Every house has a **Console** in the web editor (the Console tab of the editor page).
It shows everything your scripts print and every error they raise, live, while you
code. None of this appears in the Minecraft server console: the server console only
shows engine lifecycle messages (project loaded, sync applied), which are meant for
the server admin, not for you.

- `print(...)` and `housing.log(message)` write an `info` line to the Console, tagged
  with the project it came from.
- Lua errors write an `error` line with their context, for example
  `Error in handler for 'house_enter': main.lua:3 attempt to index a nil value` or
  `Failed to load utils.lua: utils.lua:1: unexpected symbol`.
- The Console is best effort: entries are batched in the background (flush every 2
  seconds or every 25 entries). A house can send at most 200 entries per minute;
  beyond that, entries are dropped and a single `warn` line tells you how many.

## Events

Events only fire for things happening **inside the house world**.

There is no "player joined the server" event, and there was one: `player_join`
and `player_quit` fired only for a player who joined or quit *while already
standing inside a loaded house*. A house is unloaded whenever nobody is in it,
so the ordinary login lands in the lobby and those handlers never ran, and they
read as a server event and behaved like a rarer, less reliable `house_enter`.
Use `house_enter` and `house_leave`, which cover walking in and out **and**
logging in and out inside the house. Registering the old names logs a warning
and never fires.

| Event | Extra fields on `event` | Cancellable | Notes |
|---|---|---|---|
| `house_enter` | - | no | Player enters the house world, by walking in **or by logging in inside it**. |
| `house_leave` | - | no | Player leaves the house world, by walking out **or by logging out inside it**. |
| `player_move` | - | no | Player moves at least one block (rotations ignored). |
| `player_jump` | - | no | Player left the ground upwards without flying. |
| `region_enter` | `region` (id) | no | Player enters a region, either one registered with `housing.region(...)` or one drawn with `/h region`. |
| `region_leave` | `region` (id) | no | Player leaves a region. |
| `player_chat` | `message` | no | Chat messages **cannot be cancelled** in v1. |
| `block_break` | `block` | yes | |
| `block_place` | `block` | yes | |
| `interact` | `click` (`"left"`/`"right"`), `target` (`"air"`/`"block"`), `item?` (`{material, amount, tag?}`), `block?` | yes | A click with either button, on air or on a block (main hand only). Pressure plates (`PHYSICAL`) and off-hand clicks are not covered: for players without build permission they stay protected by the classic rule. Filter on `click` if you only care about one button, since left clicks also fire on every arm swing. |
| `interact_entity` | `entity` | yes | Right click on an entity. |
| `player_damage` | `amount`, `cause` | yes | A player takes damage (any source). |
| `player_attack` | `victim`, `amount` | yes | A player damages something. `victim` is a player handle if it is a player, otherwise an entity table. |
| `player_death` | `victim`, `killer?` | no | Dispatched to the victim's house. |
| `player_kill` | `victim`, `killer` | no | Dispatched to the **killer's** house. |
| `player_respawn` | - | no | |
| `player_sneak` | `sneaking` (boolean) | no | Fires on sneak and unsneak. |
| `item_consume` | `item` | no | Eating / drinking. |
| `item_drop` | `item` | yes | |
| `item_pickup` | `item` | yes | |
| `item_select` | `slot` (0-8) | yes | Hotbar slot change. |
| `inventory_click` | `slot`, `item?`, `inventory_type` | yes | Any inventory click by the player. |
| `inventory_close` | `inventory_type` | no | The player closed an inventory. |
| `entity_death` | `entity` (handle), `killer?` | no | A non-player entity died in the house. |
| `block_damage` | `block` | yes | The player started breaking a block; it is still intact. |
| `projectile_hit` | `projectile = {type}`, `block?`, `entity?` | no | |
| `player_fish` | `state` (`FISHING`, `CAUGHT_FISH`, ...) | no | |
| `player_teleport` | `from`, `to` | yes | Dispatched to both the origin and the destination house (if different); either can cancel it. |
| `npc_click` | `npc` (id), `click` (`"left"`/`"right"`) | no | Click on a house NPC (see "House systems → NPCs"). |
| `player_swap_hands` | `item?` | yes | The **F** key. Cancelling keeps the hands as they were, which is what makes it usable as a bare keypress for an ability. |
| `player_toggle_flight` | `flying` (boolean) | yes | Double-space. Cancel it and use it as a dash or double jump. |
| `player_command` | `message`, `command` (lower case, no slash), `args` (the rest as one string) | yes | A command typed **inside the house**, before it runs. |
| `sign_change` | `block`, `lines` (array of 4) | yes | The player finished writing a sign. |
| `bucket_fill` / `bucket_empty` | `block`, `bucket` (material) | yes | Filling or emptying a bucket. |
| `vehicle_enter` / `vehicle_exit` | `vehicle` (`{type, name?, uuid}`) | yes | Boats and minecarts. |

Field shapes:

- `block` = `{x, y, z, material}`, e.g. `event.block.material == "STONE"`.
- `item` = `{material, amount}`.
- `entity` = `{type, name?, uuid}`.
- `from` / `to` = `{x, y, z, world}`.

## Permissions & the Default project

Without scripts, BlueHousing protects every house with a built-in rule: the owner and
trusted players can always break blocks, place blocks and interact; added players can
do so while the owner is online; visitors cannot. This rule needs no scripts and still
applies to every house without them.

Scripts can take over this decision, **per event type**:

- When **no project** of the house registers a handler for `block_break`, `block_place`
  or `interact`, the built-in protection applies unchanged.
- When **at least one project** handles one of these events, the built-in protection
  steps aside **for that event type only** and scripts decide. Handlers run before the
  protection would apply: if none of them calls `event.cancel()`, the action is allowed.
  Handling `block_break` does not change the protection of `block_place` or `interact`.
- Protection **fails closed**: if a handler of one of these three events errors at
  runtime (a Lua error or an exhausted instruction budget), the action is cancelled,
  the error is logged to the Console, and a warn explains the cancellation.

### Roles

Every player has a **role** in each house: `owner`, `trusted`, `added`, `visitor`, or
one the owner creates (lowercase `[a-z0-9_]`, up to 24 chars). **Roles are house data,
not code.** They are edited in the Roles menu (`/h role`, or Members → Roles) and
stored in the house, so what a role may do is visible in game without opening a
script.

A role can **override** a house flag for the people who hold it. Each of these flags
is `allow`, `deny`, `while the owner is online`, or inherited; inheriting means the
house (and any region) decides, exactly as it does for everybody else:

`break`, `place`, `interact`, `pickup`, `chest-access`, `door-interact`,
`lever-interact`, `pvp`, `chat`.

The four built-in roles start with the rules the plugin has always applied: owner and
trusted may do anything, `added` builds while the owner is online, a visitor may look
and not touch, and those defaults can now be changed like any other role's.

A **region** can override the same flags inside its own walls, for everybody or for one
role in particular. The most specific answer wins:

1. what a region says about **this role**, here (`/h region role <id> <role> <flag> …`)
2. what that region says for **everybody** (`/h region flag <id> <flag> …`)
3. what the **role** says anywhere in the house (`/h role override <role> <flag> …`)
4. the **house** flag

Which is what makes "the build team may break blocks in the arena, and nobody else may"
a thing you can say.

Scripts **read** roles; they no longer define them:

| Function | Description |
|---|---|
| `housing.role_can(role_name, flag)` | `true`/`false` for what a role of this house may do. An override answers directly; a role that inherits gets the house's own flag. Fails closed: an unknown role, a name that is not a flag, and a flag no role can override are all `false`. `'while the owner is online'` resolves against the owner being online right now. The role name lookup is case-insensitive. |

> **Changed:** the old role-registration binding is gone. Roles used to be registered from
> Lua and the plugin rewrote a generated `roles.lua` behind the commands and menus,
> which meant a house's permissions could not be read, or fixed, without a code
> editor. Anything a script defined that way is no longer a role; recreate it in the
> Roles menu.

Owners manage roles in game:

| Command | Description |
|---|---|
| `/h role set <player> <role> [temp]` | Assign a role. With `temp` it lasts until the player quits or the house world unloads. |
| `/h role remove <player>` | Remove every assignment of that player. |
| `/h role list` | Show the assignments of the house, grouped by role, temporary ones marked `(temp)`. |
| `/h members` | Chat list of everyone with a role in the house. |
| `/h trust <player>` / `/h untrust <player>` | Shortcuts for `/h role set <player> trusted` and `/h role remove <player>`. |
| `/h add <player>` / `/h remove <player>` | Shortcuts for `/h role set <player> added temp` and `/h role remove <player>`. |

The same assignments are available in the GUI. The house menu (Java) has an **Assign
Role** entry: pick a player, then a role; left click assigns it permanently, right
click for this session only. The Bedrock permissions form offers the same flow with a
player dropdown, a role dropdown and a temporary toggle. In both, **Remove Player**
lists every player with a role in the house, temporary ones marked `(temp)`, and
removes all their assignments at once.

Two player methods expose roles to scripts:

| Method | Description |
|---|---|
| `player:region()` | Id of the highest-priority house region the player is standing in, or `nil`. |
| `player:regions()` | Array of every house region covering the player, highest priority first. |
| `player:in_region(id)` | Whether the player is inside that house region. |
| `player:role()` | The player's role name in this house: `"owner"`, a role assigned with `/h role` (custom names included), or `"visitor"`. No owner-online rule applied. |
| `player:can_build()` | `true` when the player may build under the **classic** rule (owner and trusted always, added while the owner is online). Custom roles count as visitors here: the classic rule does not know what they mean. |

`housing.owner_online()` reports whether the house owner is currently online, for
rules like "this role builds while the owner is around".

### The Default project

Every house ships with a project called **Default**, editable by the owner. It starts
**empty**: who may build and interact is decided by the roles, so nothing seeded here
can lock an owner out of their own house. This is the template:

```lua
-- This project runs in your house. It starts empty on purpose: who may build
-- and interact here is decided by the Roles menu (/h role, or Members ->
-- Roles), not by code, so nothing you write here can lock you out of your own
-- house by accident.
--
-- What roles are for: every player in the house holds one - owner, trusted,
-- added, visitor, or one you create - and a role can override a flag for the
-- people who hold it. "The build team may break blocks in a house where
-- nobody else can" is a role, not a script.
--
-- What scripts are for: everything that reacts. A few to start with:
--
--   housing.on('house_enter', function(event)
--     event.player:send('<gold>Welcome to my house!')
--   end)
--
--   housing.on('block_break', function(event)
--     -- Returning without cancelling lets the break through; call
--     -- event.cancel() to stop it. Handling this event puts your script in
--     -- charge of breaking, ahead of the roles and the flags.
--     if event.block.material == 'DIAMOND_ORE' then event.cancel() end
--   end)
--
--   -- What a role may do, if a script needs to know:
--   -- housing.role_can(event.player:role(), 'break')
--
-- The full reference is in the editor's Reference tab.
```

A script still *can* take building over by handling `block_break`, `block_place` or
`interact` puts it ahead of the roles and the flags for that event, as described in
[Building rules](#building-rules), which is what a minigame wants and a permission
system does not.

### Entities

`housing.spawn()` returns a handle, and `housing.entities()` lists the ones
already there. A handle is safe to keep: every method re-checks that the entity
is still alive and still in the house, so calling one on something that has
since died simply does nothing.

| Method | |
|---|---|
| `entity:uuid()` / `entity:type()` | Identity. |
| `entity:name()` / `entity:setName(text)` | Custom name; `nil` clears it. |
| `entity:x()` / `entity:y()` / `entity:z()` | Position. |
| `entity:isAlive()` | Whether the handle still points at something. |
| `entity:health()` / `entity:setHealth(amount)` | Living entities only; `nil` for the rest. |
| `entity:teleport(x, y, z)` | Moves it inside the house world. |
| `entity:push(x, y, z)` | Adds to its velocity, capped at ±5 per axis. |
| `entity:remove()` | Despawns it, with no death drop. |

```lua
housing.on('house_enter', function(event)
    for _, entity in ipairs(housing.entities('ZOMBIE')) do
        entity:remove()
    end
    local guard = housing.spawn('IRON_GOLEM', 0, 65, 0, '<gold>Guard')
    guard:setHealth(50)
end)
```

### Giving a role its own items

`player:setSlot()` writes to one exact inventory slot, and its `tag` option
stamps a marker on the item that travels with it. Item events expose that
marker as `event.item.tag`, which is how a script tells *its* item apart from
an ordinary one of the same material; comparing display names breaks as soon
as colours or translations are involved.

The default project ships a working example in its own file, `menu_item.lua`:
owners and trusted players get a star in the last hotbar slot that opens `/h`,
it answers to either mouse button on air or on a block, and `locked = true`
keeps it in its slot.

```lua
local MENU_SLOT  = 8
local MENU_TAG   = 'house_menu'
local MENU_ROLES = { owner = true, trusted = true }

housing.on('house_enter', function(event)
  if not MENU_ROLES[event.player:role()] then return end
  event.player:setSlot(MENU_SLOT, 'NETHER_STAR', {
    name = '<light_purple><bold>House Menu',
    lore = { '<gray>Right click to open /h' },
    tag    = MENU_TAG,
    locked = true,
  })
end)

housing.on('interact', function(event)
  if event.item and event.item.tag == MENU_TAG then
    event.cancel()
    event.player:openMenu('main')
  end
end)
```

Change `MENU_ROLES` to hand it to your own roles, or delete `menu_item.lua`
to remove the item entirely: the plugin never puts it there by itself, and it
only seeds the file when it creates the Default project, so a deleted one stays
deleted.

### Roles from commands and menus

`/h role create|delete|override|chatprefix|chatsuffix|tabprefix|tabsuffix|priority`
and the **Roles** menu write to the house itself, not to a script. Adding a role is
not code:

```
/h role create vip
/h role override vip break allow
/h role chatprefix vip <gold>[VIP]
/h role set Steve vip
```

`/h role info vip` prints every overridable flag and what the role says about it,
including the ones it inherits.

> **Migrating:** roles used to be role-registration calls in a generated
> `roles.lua`. That file is no longer read; the calls in it now raise an error like
> any other removed binding. Delete it (or the calls) and recreate the roles in the
> Roles menu; the four built-in ones are already there.

A script can still take building over for a whole event type, which is what a
minigame needs and what roles are deliberately not:

```lua
-- In the minigame project, while a match is running:
local playing = false

housing.on('match_start', function() playing = true end)
housing.on('match_end', function() playing = false end)

housing.on('block_break', function(event)
  -- Handling the event puts this script ahead of the roles and the flags.
  if not playing and not housing.role_can(event.player:role(), 'break') then
    event.cancel()
  end
end)
```

`housing.emit` reaches every project of the house (guarded against runaway loops), so
one project can tell the others that a match started.

## Player handle

`event.player` (and every player handle) supports these methods. Call them with a colon:
`event.player:message("hi")`. Actions silently do nothing if the player is offline;
getters always work.

### Messaging

| Method | Description |
|---|---|
| `player:name()` / `player:uuid()` | Name / UUID (snapshotted). |
| `player:message(text)` | Chat message. Supports [MiniMessage](https://docs.advntr.dev/minimessage/format.html) (`<red>`, `<bold>`, ...). |
| `player:title(title, subtitle [, fadeIn, stay, fadeOut])` | Title + subtitle, times in ticks (default 10/70/20). |
| `player:actionbar(text)` | Action bar message (MiniMessage). |

### Effects & movement

| Method | Description |
|---|---|
| `player:sound(name [, volume, pitch])` | Play a sound (default volume 1, pitch 1). Names like `ENTITY_PLAYER_LEVELUP`. |
| `player:particle(name, x, y, z [, count])` | Spawn particles (default count 10). Names like `HAPPY_VILLAGER`. |
| `player:teleport(x, y, z [, yaw, pitch])` | Teleport inside the house world. Keeps current yaw/pitch if omitted. |
| `player:warp(id)` | Teleport to a warp registered with `housing.warp(...)`. Errors if the warp does not exist. |

### Inventory

| Method | Description |
|---|---|
| `player:give(material, amount)` | Give items (`"DIAMOND"`, `"minecraft:diamond"` and `"diamond"` all work). |
| `player:removeItem(material, amount)` | Remove up to `amount` items of that material. |
| `player:clearInventory()` | |
| `player:closeInventory()` | |
| `player:yaw()` / `player:pitch()` | Where the player is looking, in degrees. |
| `player:push(x, y, z)` | Adds to the player's velocity, for jump pads and knockback. Each axis is capped at ±5. |
| `player:velocity()` | `{x, y, z}` of the current velocity. |
| `player:setHealth(amount)` | Sets health, clamped to the player's maximum. |
| `player:maxHealth()` | The player's maximum health. |
| `player:setFood(amount)` | Sets the food bar (0-20). |
| `player:level([amount])` | Reads the experience level, or sets it when given one. |
| `player:isOnGround()` / `player:isFlying()` / `player:isSprinting()` | Movement state. |
| `player:allowFlight(enabled)` | Grants or removes flight (also stops flying when disabled). |
| `player:walkSpeed([speed])` / `player:flySpeed([speed])` | Reads or sets the speed (-1..1). |
| `player:heldItem()` | The item in the main hand, or `nil`. See the item shape below. |
| `player:armor(piece [, material, options])` | Reads a piece with one argument, replaces it with two. `piece` is `helmet`, `chestplate`, `leggings`, `boots` or `offhand`; pass `nil` as the material to clear it. |
| `player:inventory()` | Every occupied slot as `{ [slot] = item }`. |
| `player:ask(title, callback [, placeholder])` | Asks the player for text (anvil, chat as a fallback) and calls `callback(answer)`. The answer is `nil` when they cancel. |
| `player:setSlot(slot, material [, options])` | Puts an item in an exact inventory slot (`0-8` hotbar, `9-35` bag). `options` is the item option table below. A locked item cannot be moved, dragged, swapped or dropped by the player, which is what keeps a per-role item in its slot instead of being duplicated. Pass `nil` as the material to empty the slot. |
| `player:getSlot(slot)` | The item in that slot, or `nil` when it is empty. |
| `player:hasItem(material [, amount])` | Returns `true` if the player has at least `amount` (default 1). |

#### Item options

`give`, `setSlot` and `armor` all take the same option table:

| Option | Meaning |
|---|---|
| `amount` | Stack size, 1-64. |
| `name` / `lore` | Display name and lore lines (colour codes allowed). |
| `enchantments` | `{ sharpness = 5, unbreaking = 3 }`. Levels are **not** capped at their vanilla maximum. An unknown name is an error, not a silent no-op. |
| `glow` | Enchanted shimmer with no enchantment shown on the tooltip. |
| `model_data` | Custom model data, for resource packs. |
| `unbreakable` | The item never loses durability. |
| `damage` | Durability already used, on a tool or armour piece. |
| `skull` | A player name or a base64 texture, on a `PLAYER_HEAD`. |
| `tag` | A script-chosen marker, so a handler can tell its own item from an ordinary one of the same material. |
| `locked` | The player cannot move, drag, swap or drop it. This is what keeps a per-role item in its slot instead of being duplicated. |

Reading an item back (`heldItem`, `getSlot`, `inventory`) gives
`{material, amount, name?, tag?, locked?, enchantments?, model_data?, unbreakable?, damage?}`;
the optional fields are only present when the item actually has them.

### Menus

Java and Bedrock do not show the same thing. A Java player opens a chest: a grid
of slots, an item in each one, and a click that can be left, right or shifted. A
Bedrock player gets a native form: a list of buttons, or a page of dropdowns,
sliders and text fields: no slots, no grid, no click types.

So there are three entry points, and you pick one by how much of that difference
you care about.

| Method | Description |
|---|---|
| `player:menu(spec)` | **Both platforms.** Describe a list of buttons once; Java renders a chest, Bedrock a form. Use this unless you need something the other platform cannot show. |
| `player:chestMenu(spec)` | **Java's chest, in full.** Explicit rows, a slot per icon, a filler, and the click type reported back. A Bedrock player still sees it, through Geyser's own chest translation. |
| `player:form(spec)` | **Bedrock's form, in full.** Simple, modal or custom, with dropdowns, sliders, toggles and text fields. Returns `false` for a Java player, who cannot be shown one. |
| `player:isBedrock()` | `true` for a Bedrock player, so a script can branch between the two above. |
| `player:openMenu(id)` | Opens one of the plugin's own house menus, e.g. `"main"` for the `/h` menu. Errors when no menu has that id. |

#### `player:menu(spec)`: the same menu everywhere

```lua
player:menu({
    title = '&8Shop',
    content = 'Pick something',   -- Bedrock only: the text above the buttons
    buttons = {
        { text = '&aBuy',  icon = 'EMERALD',  lore = { 'Costs 10' },
          on_click = function(event) event.player:sendMessage('bought') end },
        { text = '&cSell', icon = 'GOLD_INGOT',
          on_click = function(event) event.player:sendMessage('sold') end },
    },
})
```

Buttons are laid out for you, centred row by row, up to 45 of them. Give a button
a `slot` to place it yourself (Java only, Bedrock ignores it) or an `image` URL
(Bedrock only, Java ignores it). Each `on_click` receives `{index, text, player}`,
where `index` counts from one. Every chest click is cancelled before the callback
runs, so a menu can never be looted.

#### `player:chestMenu(spec)`: the grid itself

```lua
player:chestMenu({
    title = '&8Warps',
    rows = 3,
    fill = 'GRAY_STAINED_GLASS_PANE',
    items = {
        [11] = { material = 'GRASS_BLOCK', name = '&aSpawn',
                 on_click = function(event) event.player:teleport(0, 65, 0) end },
        [13] = { material = 'PLAYER_HEAD', name = '&bOwner', skull = 'Notch' },
        [15] = { material = 'NETHER_STAR', name = '&dArena', glow = true, amount = 1 },
    },
    on_close = function(event) event.player:sendMessage('closed') end,
})
```

`rows` is optional; without it the chest grows to fit the lowest slot. `items`
may be keyed by slot as above, or be a plain list where each entry carries its
own `slot`; a `slot` field always wins. `fill` takes a material name or a full
icon table, and never gets a callback. An icon accepts `material`, `name`,
`lore`, `amount`, `glow` and `skull` (a player name or a base64 texture, on a
`PLAYER_HEAD`). `on_click` receives `{slot, click, shift, player}`, where `click`
is the Bukkit click type in lower case (`left`, `right`, `shift_left`, ...).

Slots outside the chest, two icons in one slot, or `rows` too small for an icon
are refused with an error rather than dropping the icon silently.

#### `player:form(spec)`: the Bedrock form

```lua
if player:isBedrock() then
    player:form({
        type = 'custom',
        title = 'House setup',
        components = {
            { type = 'label',       text = 'Configure your house' },
            { type = 'input',       key = 'name',  text = 'Name', placeholder = 'My house' },
            { type = 'toggle',      key = 'pvp',   text = 'PvP', default = true },
            { type = 'dropdown',    key = 'time',  text = 'Time', options = { 'day', 'night' } },
            { type = 'slider',      key = 'size',  text = 'Border', min = 50, max = 300, step = 50 },
        },
        on_submit = function(event)
            player:sendMessage('name = ' .. tostring(event.name))
            player:sendMessage('pvp = ' .. tostring(event.pvp))
        end,
        on_close = function(event) end,
    })
end
```

`type` is `simple` (default), `modal` or `custom`.

- **simple**: `buttons`, each a string or `{ text, image }`. `on_submit` receives
  `{index, text, player}`.
- **modal**: exactly two `buttons`, a yes/no question. `on_submit` receives
  `{index, confirmed, text, player}`.
- **custom**: `components` in order: `label`, `input`, `toggle`, `dropdown`,
  `slider`, `step_slider`. `on_submit` receives one field per component, named by
  its `key` (its position when it has none); a `label` reports nothing. A
  `dropdown` and a `step_slider` report the chosen **option**, not its index.
  Component options: `text`, `key`, `placeholder`, `default` (text or boolean),
  `options`, `default_index` (counting from one), `min`, `max`, `step`,
  `default_value`.

`on_close` runs when the player dismisses the form without answering.

### Music

| Method | Description |
|---|---|
| `player:playSong([id])` | Starts the house song for this listener, or a named one. |
| `player:stopSong()` | Stops whatever they are hearing. |
| `player:isSongPlaying()` | |

Music is streamed **per listener**, so a script can score one player's moment
without the rest of the house hearing it:

```lua
housing.on('region_enter', function(event)
    if event.region == 'boss' then event.player:playSong('battle') end
end)
```

A song started with `player:playSong(id)` plays once and stops; only the
house's own song loops, and only if the owner asked it to. Songs live in `plugins/BlueHousing/data/songs/`, and a starter pack is fetched on
first run, and the owner can add their own or import more with
`/ha music import <url>`. `housing.songs()` is what is actually installed, so
check it rather than assuming a name exists.

### Particles

Three levels, same as the menus:

| Method | Description |
|---|---|
| `player:particle(name, x, y, z [, count \| options])` | Raw particles, **sent to this player only**. |
| `player:playEffect(effect, x, y, z)` | One frame of a named effect, for this player only. |

`options` is `{count, dx, dy, dz, speed, color = {r, g, b}, size}`. Giving a
`color` switches to a `DUST` particle, the only kind that takes one, which is
how you build an effect of your own instead of using the catalogue.

```lua
-- A ring of your own, drawn only for the player who triggered it.
housing.on('region_enter', function(event)
    for i = 0, 23 do
        local angle = (i / 24) * math.pi * 2
        event.player:particle('DUST',
            event.player:x() + math.cos(angle) * 2,
            event.player:y() + 0.1,
            event.player:z() + math.sin(angle) * 2,
            { count = 1, color = { 255, 0, 128 }, size = 1.2 })
    end
end)
```

Because every particle goes out as a packet to named players, an effect can be
private: a marker only its owner sees, a trail only the player running it sees.
`housing.playEffect` and `housing.particle` are the house-wide versions.

Permanent emitters placed on a block are the third level: `housing.particleEmitter`
in the table above, and `/h particle` in game. They persist, redraw on a timer,
and only draw for players close enough to see them.

### Cooldowns

The only reliable way to say "not again yet": `housing.time()` is the world's
day/night cycle, not a clock, and `os` is not in the sandbox.

| Method | Description |
|---|---|
| `player:cooldown(key, seconds)` | Returns `true` when it was ready, **and starts it**. Returns `false` when it is not. |
| `player:cooldownLeft(key)` | Seconds left, `0` when ready. |
| `player:clearCooldown(key)` | Makes it ready again. |

Reading and starting in one call is deliberate: a script that checked first and
started after would let two events in the same tick both through.

```lua
housing.on('interact', function(event)
    if not event.player:cooldown('dash', 3) then
        event.player:actionbar('Dash ready in ' .. math.ceil(event.player:cooldownLeft('dash')) .. 's')
        return
    end
    event.player:push(0, 1, 0)
end)
```

Cooldowns are per player, per project, per house, and they survive a restart -
a "once a day" reward that forgets everything on reboot is worse than none.
`housing.cooldown(key, seconds)`, `housing.cooldownLeft(key)` and
`housing.clearCooldown(key)` are the same thing shared by the whole house.

### Economy

| Method | Description |
|---|---|
| `player:balance()` | Their balance, or `nil` when the server has no economy. |
| `player:charge(amount [, reason])` | Takes the money. `false` when they could not pay; nothing is taken in that case. |
| `player:pay(amount [, reason])` | Gives them money. `false` when the server does not allow it (see below). |

Taking money is always allowed: it can only remove what the player already had.
**Giving** it is gated, because a script is written by a house owner and an
unguarded deposit lets anyone with a house mint currency for the whole server.
`scripting.economy.allow_deposit` in `settings.yml` picks one of three shapes:

- `owner` (the default): the money comes out of the **house owner's** balance.
  A prize pool the owner funds themselves; nothing is created.
- `true`: free deposit. Only where house owners are trusted staff.
- `false`: `pay()` always fails; scripts can charge but never give.

Ask with `housing.economy()`, which returns `"off"`, `"read"`, `"owner"` or
`"free"`, so a shop can say so instead of silently failing on every purchase.
`housing.formatMoney(amount)` renders it with the server's currency settings.

```lua
if event.player:charge(50, 'warp to arena') then
    event.player:warp('arena')
else
    event.player:message('You need ' .. housing.formatMoney(50))
end
```

### Boss bar

| Method | Description |
|---|---|
| `player:bossBar(spec)` | Shows or updates a bar for this player. |
| `player:removeBossBar([id])` | Removes it. |

`spec` is `{ id, text, progress, color, style }`. `id` defaults to `"default"`;
calling again with the same id **updates** that bar instead of stacking a second
one. `progress` is `0..1`, `color` is one of `PINK`, `BLUE`, `RED`, `GREEN`,
`YELLOW`, `PURPLE`, `WHITE`, and `style` one of `SOLID`, `SEGMENTED_6`,
`SEGMENTED_10`, `SEGMENTED_12`, `SEGMENTED_20`.

`housing.bossBar(spec)` and `housing.removeBossBar([id])` do the same for
everyone in the house at once: a countdown, a boss health bar, a round timer.

```lua
local left = 60
housing.every(20, function()
    left = left - 1
    housing.bossBar({ id = 'timer', text = 'Time: ' .. left, progress = left / 60, color = 'RED' })
    if left <= 0 then housing.removeBossBar('timer') end
end)
```

A boss bar lives in the client, so nothing about the world unloading takes it
off the screen: the plugin removes every bar of a project when that project
stops. A project may hold 8 bars at once.

### State

| Method | Description |
|---|---|
| `player:gamemode()` | Returns `"SURVIVAL"`, `"CREATIVE"`, ... |
| `player:gamemode(name)` | Set gamemode (`SURVIVAL` / `CREATIVE` / `ADVENTURE` / `SPECTATOR`). |
| `player:heal()` | Restore full health. |
| `player:feed()` | Full hunger bar. |
| `player:damage(amount)` | Deal damage (hearts × 2). |
| `player:kill()` | |
| `player:setFire(ticks)` | Set on fire for `ticks` ticks (20 ticks = 1 second). |
| `player:effect(name, durationTicks, amplifier)` | Potion effect. Accepts `SPEED`, `speed` or `minecraft:speed`. Amplifier 0 = level I. |
| `player:clearEffect(name)` | |
| `player:kick([reason])` | Disconnect the player from the server. Reason supports MiniMessage (default: a generic "kicked from this house" message). |
| `player:runCommand(cmd)` | Run a command **as the player** (without `/`). Disabled by default: the server owner must set `scripting.player_commands: true` in settings.yml, otherwise this raises an error. |

### Getters

| Getter | Description |
|---|---|
| `player:health()` | Current health, 0-20. |
| `player:food()` | Current food level, 0-20. |
| `player:x()` / `player:y()` / `player:z()` | Position inside the house world. |
| `player:isSneaking()` | Whether the player is sneaking. |
| `player:world()` | The world name they are in. |
| `player:role()` | Their role in this house. |
| `player:can_build()` | Whether their role may build here (see "Permissions & the Default project"). |

### Player variables

Persistent per player, per project **and per house** (they survive reloads and server
restarts):

| Method | Description |
|---|---|
| `player:var(key [, default])` | Reads one of this player's variables. `default` is returned when it has never been set. |
| `player:setVar(key, value)` | Writes it. Values may be a string, a number or a boolean. |

```lua
local deaths = event.player:var("deaths", 0)   -- read with default
event.player:setVar("deaths", deaths + 1)       -- write (string, number or boolean)
```

The same values are readable for players who are **not online**, which is what
makes a record table possible:

| Function | Description |
|---|---|
| `housing.playerVar(target, key [, default])` | Reads any player's variable. `target` is a uuid, a name, or a player handle. |
| `housing.setPlayerVar(target, key, value)` | Writes it. |
| `housing.top(key [, limit])` | The players with the highest numeric value for `key`, best first: `{ {uuid, name, value, rank}, ... }`. `limit` defaults to 10, maximum 100. |

```lua
for _, row in ipairs(housing.top('best_time', 5)) do
    housing.broadcast(row.rank .. '. ' .. row.name .. ' - ' .. row.value)
end
```

Everything the project ever stored is already in memory, so a leaderboard costs
no disk access, but only players this project has written a variable for ever
appear in it.

## `housing` functions

| Function | Description |
|---|---|
| `housing.broadcast(text)` | MiniMessage to everyone in the house. |
| `housing.players()` | Array of player handles currently in the house. |
| `housing.playerCount()` | Number of players in the house. |
| `housing.setBlock(x, y, z, material)` | |
| `housing.lightning(x, y, z)` | Visual lightning strike (no damage). |
| `housing.dropItem(x, y, z, material, amount)` | Drop an item in the world. |
| `housing.random(min, max)` | Random integer, both ends inclusive. |
| `housing.time()` | Time of the house world (0-24000). |
| `housing.now()` | Milliseconds since the epoch, the only wall clock a script has. |
| `housing.fill(x1, y1, z1, x2, y2, z2, material [, replacing])` | Sets every block of a cuboid and returns how many changed. With `replacing`, only blocks of that material are touched. Capped at 200 000 blocks per call. |
| `housing.fillRegion(name, material [, replacing])` | The same over a named region, one drawn with `/h region` or declared with `housing.region(...)`. |
| `housing.spawn(type, x, y, z [, name])` | Spawns an entity and returns a handle. Players are refused (use `housing.npc()`), and a house is capped at 200 entities. Spawned entities are runtime state: they are gone when the house unloads, so spawn them again on `house_enter` if you want them back. |
| `housing.entities([type])` | Array of handles for the non-player entities in the house, optionally filtered by type. |
| `housing.getBlock(x, y, z)` | Material name of that block. |
| `housing.border()` | The house's world border: `{center_x, center_z, size, min_x, min_z, max_x, max_z}`. Read from the border itself, so it is right whatever the `world-border` flag says (including `auto`, which also decides where it sits). |
| `housing.inBorder(x, z)` | Whether that column is inside the border. Anything a script builds outside it is unreachable. |
| `housing.setTime(ticks)` | Sets the time of the house world (0-23999, wraps). |
| `housing.weather()` | `"clear"`, `"rain"` or `"thunder"`. |
| `housing.setWeather(state)` | One of `"clear"`, `"rain"`, `"thunder"`. |
| `housing.sound(x, y, z, name [, volume, pitch])` | Plays a sound at a point, for everyone nearby. |
| `housing.particle(name, x, y, z [, count])` | Spawns particles at a point, for everyone in the house. |
| `housing.particleEffects()` | The ids of the 46 named effects (`halo`, `volcano`, `spiral`, `lovewell`, …). |
| `housing.playEffect(effect, x, y, z [, range])` | Draws **one frame** of a named effect, for the players within `range`. Nothing is stored, so this is how you animate something that moves. |
| `housing.particleEmitter(id, effect, x, y, z [, range])` | Places a **permanent** emitter: it persists in the house, survives an unload and appears in `/h particle list`. Calling again with the same id moves or re-points it. |
| `housing.removeParticleEmitter(id)` | Returns `true` if it existed. |
| `housing.songs()` | Ids of the note-block songs the server has installed. |
| `housing.song()` | The song this house plays, or `nil`. |
| `housing.setSong(id [, options])` | Sets it. `options` = `{volume, speed, loop, auto_play}`. Pass `nil` as the id to turn music off. Applies to everyone inside immediately. |
| `housing.particleEmitters()` | Ids of the emitters **this scripting API created**; what the owner placed by hand is not listed. |
| `housing.region(id)` | Returns `{x1, y1, z1, x2, y2, z2}` or `nil`. Looks in the script-declared regions first, then in the ones the owner drew with `/h region`. |
| `housing.regions()` | Array of the region ids drawn with `/h region`. |
| `housing.region_at(x, y, z)` | Id of the highest-priority house region covering that block, or `nil`. |
| `housing.region_flag(id, flag)` | The raw value a house region overrides for a flag, or `nil` when it does not override it. |
| `housing.holograms()` / `housing.npcs()` | Ids of the holograms/NPCs **this scripting API created**. What the owner placed by hand is deliberately not listed: a script has no business renaming or deleting it. |
| `housing.npcSkin(id, playerName)` | Gives a script-created NPC somebody's skin, exactly as `/h npc skin` does. Pass `nil` for the default skin. |
| `housing.warp(id)` | Returns `{x, y, z, yaw, pitch}` or `nil`. |
| `housing.houseId()` | This house's id. |
| `housing.owner_online()` | Whether the house owner is currently online. |
| `housing.role_can(role_name, flag)` | What a role of this house may do: its override, or the house flag when it inherits. Fails closed on unknown roles and on anything roles do not decide. See [Roles](#roles). |
| `housing.var(key [, default])` / `housing.setVar(key, value)` | Persistent **project** variables (shared by all players of the house, scoped to this project). |
| `housing.delay(ticks, fn)` / `housing.every(ticks, fn)` | Run `fn` once after `ticks` ticks / repeatedly. Both return a task id. |
| `housing.cancel(taskId)` | Stop a delayed/repeating task. |
| `housing.cooldown(key, seconds)` / `housing.cooldownLeft(key)` / `housing.clearCooldown(key)` | House-wide cooldowns. See [Cooldowns](#cooldowns). |
| `housing.economy()` | `"off"`, `"read"`, `"owner"` or `"free"`, saying what a script may do with money here. See [Economy](#economy). |
| `housing.formatMoney(amount)` | Renders an amount with the server's currency settings. |
| `housing.playerVar(target, key [, default])` / `housing.setPlayerVar(target, key, value)` / `housing.top(key [, limit])` | Stored player data, including for offline players. See [Player variables](#player-variables). |
| `housing.bossBar(spec)` / `housing.removeBossBar([id])` | A boss bar for everyone in the house. See [Boss bar](#boss-bar). |

## House systems

These bindings talk to BlueHousing's own systems. Everything is scoped to the house the
project belongs to: a script can never touch another house's flags, scoreboard,
decorations or bans.

### Flags

Same flags as `/h flag` (names are case-insensitive: `pvp`, `god-mode`, `time`,
`spawn`, `scoreboard`, ...). Unknown names raise an error.

| Function | Description |
|---|---|
| `housing.flag(name)` | Current value: `boolean`, `number` or `string` depending on the flag type (gamemode/difficulty come back as `"CREATIVE"`, `"NORMAL"`, ...). `nil` if the flag has no value. |
| `housing.setFlag(name, value)` | Set the flag. The value is coerced to the flag type: booleans for on/off flags, numbers for integer flags, strings for text flags, `"SURVIVAL"`/`"CREATIVE"`/... for `gamemode`, `"PEACEFUL"`/`"EASY"`/... for `difficulty`, a number or `"auto"` for `world-border`. Changes apply immediately to the world and to the players inside it. Invalid values (and values the server rejects, e.g. a world-border above the house's unlocked limit) raise an error. |
| `housing.clearFlag(name)` | Remove the custom value and fall back to the default. |

```lua
housing.setFlag("pvp", false)
housing.setFlag("time", "day")
if housing.flag("god-mode") then
    housing.log("god mode is on")
end
```

### Scoreboard

The per-house scoreboard content. Visibility is controlled by the `scoreboard` flag
(`housing.setFlag("scoreboard", true)`). Lines support MiniMessage and the usual
placeholders.

| Function | Description |
|---|---|
| `housing.scoreboardTitle()` | Current custom title (`nil` = the server default is shown). |
| `housing.scoreboardTitle(text)` | Set the title (MiniMessage, max 128 characters) and refresh every player in the house. |
| `housing.scoreboardLines()` | Current custom lines as an array (empty = the server defaults are shown). |
| `housing.scoreboardLines(lines)` | Set the lines (max 15 lines, 128 characters each) and refresh. |

```lua
housing.scoreboardTitle("<gold><bold>My house")
housing.scoreboardLines({ "<gray>Players: <white>{bluehousing_world_players}", "", "<yellow>blueva.net" })
```

### Holograms

| Function | Description |
|---|---|
| `housing.hologram(id, x, y, z, lines)` | Creates a hologram, or moves and relabels it if that id already exists. `lines` is an array of MiniMessage strings (max 15 lines, 128 characters each). |
| `housing.removeHologram(id)` | Deletes it. Does nothing if there is no such hologram. |

`housing.hologram(id, x, y, z, lines)` creates the hologram if it does not exist, or
updates its position and lines if it does (`lines` is an array of MiniMessage strings,
max 15 lines / 128 characters each). `housing.removeHologram(id)` deletes it (no-op if
missing). Ids must match `[a-zA-Z0-9_-]{1,32}`.

Hologram lines and NPC names go through the server's text filter (`filters` in
`settings.yml`) like any other text a visitor reads, so a refused line raises an
error rather than being written.

Decorations created by scripts are stored with the internal prefix `lua_` (so they show
up as `lua_welcome` in `/h hologram list`) and can never overwrite or remove manually
created holograms. Creating beyond the server limit
(`decorations.max_holograms_per_world`) raises an error.

```lua
housing.hologram("welcome", 0.5, 65.0, 0.5, { "<rainbow>Welcome!", "<gray>Have fun" })
-- later, somewhere else:
housing.removeHologram("welcome")
```

### NPCs

| Function | Description |
|---|---|
| `housing.npc(id, x, y, z [, name])` | Creates an NPC, or moves and renames it if that id already exists. |
| `housing.removeNpc(id)` | Deletes it. Does nothing if there is no such NPC. |

`housing.npc(id, x, y, z [, name])` creates the NPC if it does not exist (default skin,
scale 1.0, the name (or the id) shown as its hologram label) or updates its position
and name if it does. `housing.removeNpc(id)` deletes it (no-op if missing). Same id
rules and `lua_` prefix as holograms. Creating beyond the server limit
(`decorations.max_npcs_per_world`) raises an error, as does creating NPCs on a server
without BlueFoundation.

Clicks on any house NPC fire the `npc_click` event **before** the NPC's configured
actions run. `event.npc` is the id (script-created NPCs report the id without the
`lua_` prefix) and `event.click` is `"left"` or `"right"`.

```lua
housing.npc("shop", 10.5, 64.0, -3.5, "<green>Shop")

housing.on("npc_click", function(event)
    if event.npc == "shop" and event.click == "right" then
        event.player:message("<yellow>The shop is closed today!")
    end
end)
```

### Moderation

Everything here writes what the commands and menus write, so a script, `/h ban`
and the Moderation menu are always talking about the same list. Every function
takes a player handle, a UUID string **or a name**, including for players who
have never been online in this session.

| Function | Description |
|---|---|
| `housing.ban(playerOrUuid [, reason])` | Permanently ban a player from the house (max 200 characters of reason, default `"No reason specified"`). If the player is online and inside the house they are ejected to the main spawn with a notification, exactly like `/h ban`. Banning the house owner raises an error. |
| `housing.unban(playerOrUuid)` | Remove the ban. Returns `true` if a ban existed. |
| `housing.isBanned(playerOrUuid)` | Returns `true` if the player is banned. |
| `housing.tempban(target, seconds [, reason])` | A ban that lifts itself, written exactly as `/h tempban` writes it. Ejects them the same way. |
| `housing.mute(target [, seconds, reason])` | Mutes them in the house. Without a duration the mute is permanent. |
| `housing.unmute(target)` | Returns `true` if they were muted. |
| `housing.isMuted(target)` | |

Kicking is on the player handle: `player:kick([reason])`.

### Roles and access

| Function | Description |
|---|---|
| `housing.role(target)` | The role that player holds here (`owner`, `trusted`, `added`, `visitor`, or a role your scripts registered). |
| `housing.setRole(target, role [, temporary])` | Assigns it. A **temporary** role lasts until they leave the house; a persistent one is what `/h role` and `/h trust` write. The owner's role cannot be changed. |
| `housing.removeRole(target)` | Returns `true` if they had one. |
| `housing.members()` | Everyone with a stored role in this house, online or not: `{ {uuid, name, role}, ... }`. |
| `housing.whitelist()` | Whether the house is closed to visitors. |
| `housing.setWhitelist(enabled)` | Opens or closes it. |
| `housing.whitelistAdd(name)` / `housing.whitelistRemove(name)` | By name, as `/h whitelist` takes it. |
| `housing.whitelisted()` | The names on the list. |

`housing.setRole` assigns a role; what that role may do is defined in the Roles
menu, not from a script, and `housing.role_can` reads it; see [Roles](#roles).

```lua
-- A visitor who finishes the parkour gets a role for as long as they stay.
housing.on("region_enter", function(event)
    if event.region == "finish" then
        housing.setRole(event.player, "champion", true)
    end
end)
```

```lua
housing.on("player_chat", function(event)
    if event.message:lower():find("badword") then
        housing.ban(event.player, "Watch your language")
    end
end)
```

## Regions and warps

Regions are axis-aligned boxes (inclusive bounds). Warps are named teleport points.
Both are scoped to the project that registers them. Register them at the top of
`main.lua`:

### Regions drawn in game (`/h region`)

Besides the coordinates you hardcode in a script, the house owner can draw
regions with `/h region wand` (left click = corner 1, right click = corner 2)
or `/h region pos1|pos2`, then `/h region create <id>`. Those regions are
persistent, appear in the **Regions** menu, and are visible to Lua:

```lua
housing.on("region_enter", function(event)
    if event.region == "arena" then
        event.player:message("<red>Watch out, PvP is on here!")
    end
end)

housing.on("player_move", function(event)
    -- Which regions is this player standing in, highest priority first?
    for _, id in ipairs(event.player:regions()) do
        housing.log(id)
    end
end)

if event.player:in_region("shop") then ... end
```

They also override flags inside their bounds, which is usually simpler than
checking coordinates by hand: `/h region flag arena pvp true` turns PvP on
only inside `arena`, whatever the house-wide `pvp` flag says. A house with no
regions behaves exactly as before: every flag applies to the whole house.

```lua
housing.region("spawn", 0, 60, 0, 10, 70, 10)
housing.warp("checkpoint1", 25.5, 64.0, 10.5, 90, 0)

housing.on("region_enter", function(event)
    housing.log(event.player:name() .. " entered " .. event.region)
end)
```

A script can also draw a **real** house region, one that persists, shows up in
the Regions menu and overrides flags, rather than the script-local kind
`housing.region(...)` declares:

| Function | Description |
|---|---|
| `housing.createRegion(id, x1, y1, z1, x2, y2, z2 [, priority])` | Creates or moves it. Ids are `[a-z0-9_-]`, up to 24 characters. Redrawing an existing region **keeps the flags** its owner already set on it; the script only moved the walls. |
| `housing.deleteRegion(id)` | Returns `true` if it existed. |
| `housing.setRegionFlag(id, flag, value)` | Overrides a flag inside it; pass `nil` to clear the override. Only region-capable flags are accepted, and asking for a house-wide one is an error rather than a silent no-op. |

```lua
housing.createRegion("arena", 0, 60, 0, 40, 80, 40, 10)
housing.setRegionFlag("arena", "pvp", true)
```

## Custom commands: `/h run`

```lua
housing.command("reset", function(event)
    event.player:warp("start")
    event.player:message("<yellow>Run reset!")
end)
```

Players execute it in-game with `/h run reset`. Extra words after the command name are
passed as `event.args` (`event.args[1]`, `event.args[2]`, ...).

Command names are shared by all projects of the house: if two projects register the same
name, the project with the lowest `position` wins (the other registration is ignored and
logged).

## The Lua dialect

Scripts run on **Lua 5.5**. If you have written Lua for older Minecraft plugins,
three things changed and only one of them is loud:

- **Division always produces a float.** `10/2` is `5.0`, not `5`, so
  `"Score: " .. total/2` prints `5.0`. Use `//` for integer division: `10//2` is `5`.
  Nothing errors here; it just reads wrong, so it is worth knowing before you
  build a scoreboard out of it.
- **Integers are a real type.** `math.type(1)` is `"integer"`, `math.type(1.0)` is
  `"float"`, and `math.maxinteger` exists.
- **Bitwise operators are built in**: `&`, `|`, `~`, `<<`, `>>`. The old `bit32`
  library is gone, and so are `math.pow` (use `^`), `math.ldexp` and `math.frexp`.

`warn(...)` works and goes to your house Console, same as `print`.

## Sandbox & limits

- Available standard libraries: `string`, `table`, `math`, `utf8`, plus base
  functions (`tostring`, `tonumber`, `pairs`, `ipairs`, `select`, ...). `print` writes
  to the house Console in the web editor (see "Console & debugging"). `require(name)`
  loads another file of the same project (see "Projects & files").
- **Not available:** `io`, `os`, `debug`, `coroutine`, `package`, `load`, Java access.
- Each handler execution is limited to **1,000,000 VM instructions** (configurable via
  `scripting.max_instructions_per_event`). Infinite loops are killed automatically and
  logged, and the server never hangs.
- `housing.emit` chains nested deeper than 20 levels raise an error (event loop guard),
  and one root fire invokes at most 1000 handlers in total across all projects (event
  storm guard; a warn is logged when the budget is hit).
- Scripts run on the main server thread: never busy-wait, use `housing.delay` /
  `housing.every` instead.

## Full example: parkour minigame with checkpoints

```lua
-- Setup (runs once when the script loads)
housing.warp("start", 0.5, 64.0, 0.5, 90, 0)
housing.warp("cp1", 20.5, 66.0, 5.5, 90, 0)
housing.warp("cp2", 40.5, 70.0, -3.5, 180, 0)

housing.region("checkpoint1", 18, 65, 3, 22, 68, 7)
housing.region("checkpoint2", 38, 69, -5, 42, 73, -1)
housing.region("finish", 55, 72, 0, 59, 76, 4)
housing.region("void", -50, 0, -50, 100, 60, 100)  -- fell off the parkour

local CHECKPOINT_WARP = { checkpoint1 = "cp1", checkpoint2 = "cp2" }
local NEXT_CHECKPOINT = { checkpoint1 = 1, checkpoint2 = 2 }

housing.on("house_enter", function(event)
    event.player:warp("start")
    event.player:gamemode("ADVENTURE")
    event.player:message("<gold>Welcome to the parkour! Reach the top.")
end)

housing.on("region_enter", function(event)
    local player = event.player
    local region = event.region

    if region == "void" then
        local cp = player:var("checkpoint", 0)
        player:warp(cp == 0 and "start" or ("cp" .. cp))
        player:message("<red>You fell! Back to your checkpoint.")
        return
    end

    if region == "finish" then
        player:title("<green>FINISH!", "<yellow>You completed the parkour!")
        player:sound("UI_TOAST_CHALLENGE_COMPLETE", 1, 1)
        housing.broadcast("<gold>" .. player:name() .. " finished the parkour!")
        return
    end

    local level = NEXT_CHECKPOINT[region]
    if level ~= nil and player:var("checkpoint", 0) < level then
        player:setVar("checkpoint", level)
        player:title("<green>Checkpoint!", "", 5, 40, 10)
        player:sound("ENTITY_PLAYER_LEVELUP", 1, 1.5)
    end
end)

housing.command("reset", function(event)
    event.player:setVar("checkpoint", 0)
    event.player:warp("start")
    event.player:message("<yellow>Progress reset.")
end)
```
