# Commands and Permissions

Complete reference for all Blue Worlds commands and permissions.

**Notation:**
- `[required]` - Required parameter
- `(optional)` - Optional parameter
- `<option1|option2>` - Choose one option

---

## Command Prefixes

Blue Worlds uses three main command trees:

| Command | Aliases | Purpose |
|---------|---------|---------|
| `/blueworlds` | `/blueworld`, `/bw` | Plugin info, help, and template management |
| `/privateworlds` | `/world`, `/w`, `/pw` | Player private-world management |
| `/globalworlds` | `/gw` | Administrator global-world management |

---

## Main Plugin Commands

| Command | Permission | Description |
|---------|------------|-------------|
| `/bw` | `blueworlds.info` | Display plugin information |
| `/bw help` | `blueworlds.help` | Show help |
| `/bw reload` | `blueworlds.admin.reload` | Reload configuration and menus |

## Template Commands

All template commands use the `/bw template` prefix.

| Command | Permission | Description |
|---------|------------|-------------|
| `/bw template create <name> [world] [-f]` | `blueworlds.template.create` | Create template from existing world |
| `/bw template delete <name> [-f]` | `blueworlds.template.delete` | Delete a template |
| `/bw template list` | `blueworlds.template.list` | List available templates |
| `/bw template info <name>` | `blueworlds.template.info` | View template details |
| `/bw template import <folder> <name> [-f]` | `blueworlds.template.import` | Import folder as template |
| `/bw template export <name> [folder]` | `blueworlds.template.export` | Export template to server folder |
| `/bw template copy <source> <new> [-f]` | `blueworlds.template.copy` | Copy a template |

Protected default templates: `normal`, `nether`, `end`, `void`.

---

## Private World Commands

Base `/pw` opens a GUI when `private_worlds.use_gui` is enabled.

### Creation & Navigation

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw create <template> [alias] [-f]` | `blueworlds.private.create` | Create a private world |
| `/pw delete [slot]` | `blueworlds.private.delete` | Delete your world |
| `/pw confirm` | - | Confirm deletion |
| `/pw reset <slot>` | `blueworlds.private.reset` | Reset world to its template |
| `/pw goto [slot]` | `blueworlds.private.goto` | Teleport to your world |
| `/pw goto <player> [slot]` | `blueworlds.private.goto` | Teleport to another player's world |
| `/pw list` | `blueworlds.private.list` | List worlds you own or can access |
| `/pw info [slot]` | `blueworlds.private.info` | Show world details |

### Flags

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw flag set <flag> <value>` | `blueworlds.private.flag` | Set a world flag |
| `/pw flag get <flag>` | `blueworlds.private.flag` | Get current flag value |
| `/pw flag remove <flag>` | `blueworlds.private.flag` | Reset flag to default |
| `/pw flag list [category]` | `blueworlds.private.flag` | List flags by category |
| `/pw flag info <flag>` | `blueworlds.private.flag` | Show flag details |

### Access Control

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw trust <player>` | `blueworlds.private.trust` | Give permanent access |
| `/pw add <player> [duration]` | `blueworlds.private.add` | Give temporary access |
| `/pw remove <player>` | `blueworlds.private.remove` | Remove trust/temporary access |
| `/pw members` | `blueworlds.private.members` | List world members |
| `/pw whitelist on/off/add/remove/list/status` | `blueworlds.private.whitelist` | Manage whitelist |

### Moderation

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw kick <player> [reason]` | `blueworlds.private.kick` | Kick a player from your world |
| `/pw ban <player> [reason]` | `blueworlds.private.ban` | Ban a player |
| `/pw tempban <player> <duration> [reason]` | `blueworlds.private.ban` | Temporarily ban a player |
| `/pw unban <player>` | `blueworlds.private.ban` | Unban a player |
| `/pw history [player]` | `blueworlds.private.history` | View moderation history |

### Search & Backups

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw search [tags] [page]` | `blueworlds.private.search` | Search public private worlds |
| `/pw backup create [slot]` | `blueworlds.private.backup` | Create a backup |
| `/pw backup list [slot]` | `blueworlds.private.backup` | List backups |
| `/pw backup restore [slot] <backup>` | `blueworlds.private.backup` | Restore from backup |
| `/pw backup delete [slot] <backup>` | `blueworlds.private.backup` | Delete a backup |
| `/pw backup info [slot] <backup>` | `blueworlds.private.backup` | Backup details |

### Admin

| Command | Permission | Description |
|---------|------------|-------------|
| `/pw admin info <world_id>` | `blueworlds.private.admin` | View any private world info |
| `/pw admin list [page]` | `blueworlds.private.admin` | List all private worlds |
| `/pw admin goto <world_id>` | `blueworlds.private.admin` | Teleport to any private world |
| `/pw admin delete <world_id>` | `blueworlds.private.admin` | Delete any private world |
| `/pw admin backup <world_id> <action>` | `blueworlds.private.admin` | Manage backups of any private world |

---

## Global World Commands

All global world commands use the `/gw` prefix.

| Command | Permission | Description |
|---------|------------|-------------|
| `/gw create new <world> [flags]` | `blueworlds.global.create` | Generate a new world |
| `/gw create template <world> <template>` | `blueworlds.global.create` | Create from template |
| `/gw delete <world>` | `blueworlds.global.delete` | Delete global world |
| `/gw reset <world> [template]` | `blueworlds.global.delete` | Reset world to template |
| `/gw goto <world>` | `blueworlds.global.goto` | Teleport to global world |
| `/gw list` | `blueworlds.global.list` | List global worlds |
| `/gw info <world>` | `blueworlds.global.info` | World info |
| `/gw flag set <flag> <value> -w <world>` | `blueworlds.global.flag` | Set a flag |
| `/gw flag get <flag> -w <world>` | `blueworlds.global.flag` | Get flag value |
| `/gw flag remove <flag> -w <world>` | `blueworlds.global.flag` | Remove a flag |
| `/gw flag list [category]` | `blueworlds.global.flag` | List flags |
| `/gw flag info <flag>` | `blueworlds.global.flag` | Flag details |
| `/gw import <world>` | `blueworlds.global.import` | Import existing world |
| `/gw link <world> nether/end <target>` | `blueworlds.global.link` | Link dimensions |
| `/gw link <world> --all` | `blueworlds.global.link` | Auto-link nether and end |
| `/gw link <world> info` | `blueworlds.global.link` | View links |
| `/gw link <world> unlink nether/end/--all` | `blueworlds.global.link` | Unlink dimensions |
| `/gw backup <world> create/list/restore/delete` | `blueworlds.global.backup` | Backup management |
| `/gw help` | `blueworlds.global.help` | Show help |

---

## Permissions

### Wildcards

| Permission | Default | Description |
|------------|---------|-------------|
| `blueworlds.*` | op | All plugin permissions |
| `blueworlds.private.*` | true | All private world permissions |
| `blueworlds.global.*` | op | All global world permissions |
| `blueworlds.template.*` | op | All template permissions |
| `blueworlds.admin.*` | op | Admin bypass permissions |

### Main & Template

| Permission | Default | Description |
|------------|---------|-------------|
| `blueworlds.info` | true | View plugin info |
| `blueworlds.help` | true | View help |
| `blueworlds.admin.reload` | op | Reload configuration |
| `blueworlds.template.create` | op | Create templates |
| `blueworlds.template.delete` | op | Delete templates |
| `blueworlds.template.list` | true | List templates |
| `blueworlds.template.info` | true | View template info |
| `blueworlds.template.import` | op | Import templates |
| `blueworlds.template.export` | op | Export templates |
| `blueworlds.template.copy` | op | Copy templates |

### Private World

| Permission | Default | Description |
|------------|---------|-------------|
| `blueworlds.private.create` | true | Create private worlds |
| `blueworlds.private.delete` | true | Delete own worlds |
| `blueworlds.private.reset` | true | Reset own worlds |
| `blueworlds.private.goto` | true | Teleport to worlds |
| `blueworlds.private.list` | true | List worlds |
| `blueworlds.private.info` | true | View world info |
| `blueworlds.private.flag` | true | Manage flags |
| `blueworlds.private.whitelist` | true | Manage whitelist |
| `blueworlds.private.trust` | true | Trust players |
| `blueworlds.private.add` | true | Add players temporarily |
| `blueworlds.private.remove` | true | Remove player access |
| `blueworlds.private.members` | true | View members |
| `blueworlds.private.kick` | true | Kick players |
| `blueworlds.private.ban` | true | Ban players |
| `blueworlds.private.history` | true | View history |
| `blueworlds.private.search` | true | Search worlds |
| `blueworlds.private.backup` | true | Manage backups |
| `blueworlds.private.admin` | op | Admin commands |

### Global World

| Permission | Default | Description |
|------------|---------|-------------|
| `blueworlds.global.create` | op | Create global worlds |
| `blueworlds.global.delete` | op | Delete global worlds |
| `blueworlds.global.goto` | op | Teleport to global worlds |
| `blueworlds.global.list` | true | List global worlds |
| `blueworlds.global.info` | true | View global world info |
| `blueworlds.global.flag` | op | Manage global flags |
| `blueworlds.global.import` | op | Import worlds |
| `blueworlds.global.link` | op | Link dimensions |
| `blueworlds.global.backup` | op | Manage backups |
| `blueworlds.global.help` | true | View help |

### Admin & Bypass

| Permission | Default | Description |
|------------|---------|-------------|
| `blueworlds.admin.bypass.access` | op | Bypass access restrictions |
| `blueworlds.admin.bypass.build` | op | Bypass build restrictions |

### Flag Override Permissions

| Permission | Description |
|------------|-------------|
| `blueworlds.flag.override.*` | Bypass all flags |
| `blueworlds.flag.override.break` | Bypass break restrictions |
| `blueworlds.flag.override.place` | Bypass place restrictions |
| `blueworlds.flag.override.interact` | Bypass interact restrictions |
| `blueworlds.flag.override.pvp` | Bypass PvP restrictions |
| `blueworlds.flag.override.chest-access` | Bypass chest restrictions |
| `blueworlds.flag.override.door-interact` | Bypass door restrictions |
| `blueworlds.flag.override.lever-interact` | Bypass lever restrictions |
| `blueworlds.flag.override.drop` | Bypass drop restrictions |
| `blueworlds.flag.override.pickup` | Bypass pickup restrictions |

### Permission Packs

| Pack | Default | Contents |
|------|---------|----------|
| `blueworlds.pack.user` | true | Basic player permissions, all private commands, global list/info/goto |
| `blueworlds.pack.admin` | op | `blueworlds.*` |

### Per-Flag Permissions

- `blueworlds.private.flag.<flag>` - allow setting a specific flag in private worlds
- `blueworlds.global.flag.<flag>` - allow setting a specific flag in global worlds

---

## Command Aliases

### Main Command Aliases
- `/blueworlds`
- `/blueworld`
- `/bw`

### Private World Aliases
- `/privateworlds`
- `/world`
- `/w`
- `/pw`

### Global World Aliases
- `/globalworlds`
- `/gw`
