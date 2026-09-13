# Configuration

Everything server-side lives in `settings.yml`. Everything a player reads lives in `language.yml`. Every menu is a file under `menus/`.

This page covers the parts worth changing. Storage, network and performance have their own pages.

---

## Top of the file

```yaml
metrics: true             # bStats. No custom charts; nothing about your players
update_checker:
  enabled: true
  notify_admins: true     # tells bluehousing.admin.updates holders on join
debug: false              # verbose [DEBUG] logging
```

## Houses

```yaml
house:
  use_gui: true           # commands that can open a menu, do
  visitor_history: 100    # how many visitors a house remembers
  isolation:
    chat: true
    tab: true
```

**Isolation** keeps a house's chat and tab list inside it. Several private worlds are open at once, so without it somebody chatting in their house is heard by strangers in another, and the tab list is the whole server.

The grouping is "the house you are standing in, or nowhere", so everybody outside the houses is one group and the lobby keeps working as a lobby. Chat **trims the recipient list** rather than cancelling and re-broadcasting, so other plugins' formatting and logging still work. Turning either off mid-session gives everybody back.

## Loading and unloading

```yaml
world_unloading:
  enabled: true
  empty_grace_seconds: 60

world_loading:
  enabled: true
  default_priority: 100
  always_simulate: true
  simulation_ticks: 60
  seconds_per_player: 3
```

A house unloads once it has been **empty** for the grace period, not when a particular player quits. The grace matters because unloading copies the world to disk.

`world_loading` is the queue shown while a house is being copied in. `bluehousing.house.queue.priority.<n>` moves a player up it.

## Inventories and locations

```yaml
world_separation:
  inventory:
    enabled: true
  location:
    enabled: true
```

Each house keeps its own inventory; `location` remembers where a player was so `/h leave` can put them back.

## Cleanup and trash

```yaml
world_cleanup:
  enabled: false
  inactivity_days: 30
  check_interval_hours: 24
  warnings:
    enabled: true
    warn_at_days: [7, 3, 1]

trash:
  enabled: true
  grace_period: 7d
```

Cleanup deletes houses nobody has visited for `inactivity_days`, warning the owner first. It is **off by default**.

With trash on, a deleted house is moved to `data/trash/` and kept for the grace period.

## Backups

```yaml
backups:
  enabled: true
  backup_interval: 2d
  max_backups: 10
  delete_old_backups: true
  compression: true
  backup_on_shutdown: true
  skip_if_no_changes: true
```

## Menus

```yaml
menus:
  description_words_per_line: 5
  sounds:
    enabled: true
    success: ui.button.click
    denied: block.note_block.bass
```

Minecraft does not wrap lore, so a description is broken into lines of `description_words_per_line` **words**, counted in words because a character count cuts through the middle of one. `0` leaves it on one line.

A click that did something answers with `success`; one no handler claimed answers with `denied`. Silence reads as lag; a refusal should read as a refusal.

### Editing a menu

Menu files are copied to `plugins/BlueHousing/menus/` on first start and **never overwritten**, so a menu you repaint stays yours. A menu whose *layout* changed in an update is restored from the JAR, because an old copy would draw into slots that no longer exist.

To reset one, delete the file and restart.

## Roles

```yaml
roles:
  tablist_prefixes: true
```

## Scripting

```yaml
scripting:
  enabled: true
  max_instructions_per_event: 1000000
  max_string_length: 1048576
  player_commands: false
  economy:
    allow_deposit: owner        # owner | true | false
```

See [Scripting](scripting.md).

## Web editor

```yaml
webeditor:
  editor_url: https://housing.blueva.net
  history_limit: 20
  editor_roles:
    - trusted
```

## World extraction guards

```yaml
worlds:
  extraction:
    max_entries: 200000
    max_expanded_size_mb: 20480
```

Ceilings applied when an archive is unpacked: a template or backup that claims to be larger than this is refused rather than filling the disk.

## Language

Every player-facing string is in `language.yml`, including menu titles and lore. Nothing is hardcoded.

Messages use [MiniMessage](https://docs.advntr.dev/minimessage/format.html) (`<red>`, `<gradient:#a:#b>`) and legacy `&` codes, and `{bluehousing_prefix}` is the shared prefix every message carries.

Console messages keep the `[BlueHousing]` prefix on purpose: a server log is not chat, and the plugin's name at the start of the line is what makes it greppable.
