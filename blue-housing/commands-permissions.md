# Commands and Permissions

**Notation:**
- `<required>`: required parameter
- `[optional]`: optional parameter
- `<a|b>`: choose one

Main command: `/bluehousing` (aliases `h`, `bh`, `housing`)
Admin command: `/bluehousingadmin` (aliases `ha`, `bha`)

A house is addressed by **slot** (`2`) or by **owner and slot** (`Whiron 1`). Most commands take an optional slot so they work from anywhere.

---

## Houses

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h` | `bluehousing.house.menu` | House menu inside your own house, Discovery outside |
| `/h info` | `bluehousing.info` | Plugin information |
| `/h help` | `bluehousing.help` | Help |
| `/h create [template]` | `bluehousing.house.create` | Create a house |
| `/h list` | `bluehousing.house.list` | Your houses |
| `/h goto [player] [slot]` | `bluehousing.house.goto` | Go to a house, or open the navigation menu |
| `/h leave` | `bluehousing.house.leave` | Leave the house |
| `/h reset` | `bluehousing.house.reset` | Back to the template |
| `/h delete` / `/h confirm` | `bluehousing.house.delete` | Delete a house |
| `/h backup <create\|list\|restore\|delete>` | `bluehousing.house.backup` | Backups |

## Flags

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h flag set <flag> <value> [slot]` | `bluehousing.house.flag` + `bluehousing.house.flag.<flag>` | Set a flag |
| `/h flag get <flag>` | `bluehousing.house.flag` | Read one |
| `/h flag remove <flag>` | `bluehousing.house.flag` | Back to the default |
| `/h flag list` / `/h flag info <flag>` | `bluehousing.house.flag` | |
| `/h flag scoreboard <title\|set\|clear>` | `bluehousing.house.flag.scoreboard` | Sidebar |

## Members, Roles and Regions

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h trust <player>` / `/h untrust <player>` | `bluehousing.house.trust` | Permanent access |
| `/h add <player>` / `/h remove <player>` | `bluehousing.house.add` / `.remove` | Temporary access |
| `/h members` | `bluehousing.house.members` | The member list |
| `/h whitelist <on\|off\|add\|remove\|list\|status>` | `bluehousing.house.whitelist` | Whitelist |
| `/h role <set\|remove\|list\|create\|delete\|info\|override\|priority\|chatprefix\|chatsuffix\|tabprefix\|tabsuffix\|menu>` | `bluehousing.house.role` | Roles |
| `/h region <wand\|pos1\|pos2\|create\|delete\|redefine\|list\|info\|here\|check\|priority\|flag\|role\|menu>` | `bluehousing.house.region` | Regions |

## Moderation

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h kick <player>` | `bluehousing.house.kick` | |
| `/h ban <player> [reason]` | `bluehousing.house.ban` | |
| `/h tempban <player> <duration> [reason]` | `bluehousing.house.ban` | |
| `/h unban <player>` | `bluehousing.house.ban` | |
| `/h mute <player> [duration] [reason]` | `bluehousing.house.mute` | |
| `/h unmute <player>` | `bluehousing.house.mute` | |
| `/h history` | `bluehousing.house.history` | Moderation log |

## Discovery

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h discovery [popular\|liked\|recent\|online\|category <name>\|search <text>]` | `bluehousing.house.discovery` | The directory |
| `/h browse` | `bluehousing.house.discovery` | Browse list |
| `/h search` | `bluehousing.house.search` | Search menu |
| `/h like` | `bluehousing.house.like` | Like this house |
| `/h rate <0-5>` | `bluehousing.house.rate` | Rate it |
| `/h cookie [amount]` | `bluehousing.house.cookie` | Give a gift |

## Decorations

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h hologram <create\|remove\|list\|move\|addline\|removeline\|setline>` | `bluehousing.house.hologram` | |
| `/h npc <create\|remove\|list\|move\|skin\|hologram\|action\|message\|command>` | `bluehousing.house.npc` | |
| `/h particle <create\|remove\|move\|range\|toggle\|list\|types>` | `bluehousing.house.particle` | |
| `/h heads <menu\|library\|buy\|claim\|list\|categories\|search>` | `bluehousing.house.heads` | Head shop |

## Music

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h music <list\|info\|play\|stop\|volume\|speed\|loop\|autoplay\|mute\|reload>` | `bluehousing.house.music` | |

## Economy

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h economy balance [player]` | `bluehousing.house.economy` (`.balance.others`) | |
| `/h economy pay <player> <amount>` | `bluehousing.house.economy.pay` | |
| `/h economy donate <amount>` | `bluehousing.house.economy.donate` | |
| `/h economy buy <slot\|border>` | `bluehousing.house.economy.buy` | |
| `/h economy sale <offer\|accept\|cancel>` | `bluehousing.house.economy.sell` | |

Aliases: `/h eco`, `/h money`, `/h balance`.

## Scripting

| Command | Permission | Description |
|---------|-----------|-------------|
| `/h code` | `bluehousing.house.code` | One-use web editor link code |
| `/h run <name> [args]` | `bluehousing.house.run` | Run a command a script registered |

---

## Admin Commands

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ha` | `bluehousing.house.admin` | Admin help |
| `/ha mode <on\|off>` | `bluehousing.house.admin` | Admin mode for the house you are in |
| `/ha menu <player\|house>` | `bluehousing.house.admin` | Manage a house |
| `/ha goto <player> [slot]` | `bluehousing.house.admin` | Go there, with admin mode armed |
| `/ha info [house]` | `bluehousing.house.admin` | |
| `/ha list` | `bluehousing.house.admin` | Every house |
| `/ha delete [house]` | `bluehousing.house.admin` | |
| `/ha backup <create\|list\|restore\|delete>` | `bluehousing.house.admin` | |
| `/ha feature <house>` | `bluehousing.admin.feature` | Pin to the top of Discovery |
| `/ha setlobby` | `bluehousing.admin.setlobby` | Where `/h leave` lands |
| `/ha template <create\|delete\|list\|info\|setdesc\|setdisplay\|edit\|save\|cancel\|flag\|npc>` | `bluehousing.house.admin` | Templates |
| `/ha economy <balance\|set\|add\|remove>` | `bluehousing.admin.economy` | |
| `/ha music <import\|reload\|list>` | `bluehousing.admin.music` | |
| `/ha heads reload` | `bluehousing.house.admin` | Re-fetch the head catalogue |
| `/ha storage <status\|migrate\|reindex>` | `bluehousing.admin.storage` | |
| `/ha network status` | `bluehousing.admin.network` | |

---

## Permission Reference

### Grouping

| Node | Default | Grants |
|------|---------|--------|
| `bluehousing.*` | op | Everything |
| `bluehousing.house.*` | false | Every player command |
| `bluehousing.flags.pack.creative` | false | Every flag a creative house needs |

### Limits

The **highest** number the player holds wins.

| Node | Default | Meaning |
|------|---------|---------|
| `bluehousing.house.slots.<n>` | - | How many houses |
| `bluehousing.house.hologram.limit.<n>` | `.20` | Holograms per house |
| `bluehousing.house.npc.limit.<n>` | `.10` | NPCs per house |
| `bluehousing.house.particle.limit.<n>` | - | Emitters per house |
| `bluehousing.house.queue.priority.<n>` | - | Place in the house-loading queue |

`settings.yml` is the fallback when no limit permission is granted.

### Flags

| Node | Default | Meaning |
|------|---------|---------|
| `bluehousing.house.flag.<flag>` | false | The owner may set it |
| `bluehousing.admin.flag.<flag>` | op | Staff may set it on any house |
| `bluehousing.flag.override.<flag>` | false | This player ignores that flag |

### Admin and bypass

| Node | Default | Meaning |
|------|---------|---------|
| `bluehousing.house.admin` | op | Admin commands, and the right to **turn admin mode on** |
| `bluehousing.admin.bypass.access` | op | In admin mode, ignore whitelist and bans |
| `bluehousing.admin.bypass.build` | op | In admin mode, ignore build flags |
| `bluehousing.admin.updates` | op | Told when a newer version is released |

> The bypasses apply **only while admin mode is on** in that house. Holding the permission is not being on duty: staff can visit a house as a visitor, which is what `/h goto` does.

### Discovery and music

| Node | Default | Meaning |
|------|---------|---------|
| `bluehousing.discovery.staff` | false | Owner's houses show in a staff channel |
| `bluehousing.discovery.ranked` | false | …in a ranked-players channel |
| `bluehousing.discovery.youtube` | false | …in a content-creator channel |
| `bluehousing.music.songs` | false | Every song without buying it |
| `bluehousing.music.song.<id>` | - | One song without buying it |
| `bluehousing.house.heads.place.any` | false | Place any head, bought or not |
| `bluehousing.house.economy.earn` | **true** | Earn from rewards |
