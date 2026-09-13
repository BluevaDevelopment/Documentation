# Moderation

## House Moderation

The owner's tools for their own house.

```
/h kick <player>
/h ban <player> [reason]
/h tempban <player> <duration> [reason]
/h unban <player>
/h mute <player> [duration] [reason]
/h unmute <player>
/h history
```

Durations are `10m`, `2h`, `7d` and the like. `/h history` lists every action taken in the house, capped by `moderation.history_limit` (100; `0` for no cap), together with the current bans.

Every action records **who took it**, and none of them can be pointed at the house's owner.

Only the owner, or staff in [admin mode](#admin-mode) in that house, may moderate. Holding `bluehousing.house.admin` is not enough on its own: Bukkit answers every permission with yes for an operator, so that check alone let any op moderate any house without being on duty.

## Server Moderation

```
/ha menu <player|house>       # manage somebody else's house
/ha info [house]              # what the plugin knows about it
/ha delete [house]
/ha backup create|list|restore|delete
/ha feature <house>           # pin it to the top of Discovery
/ha list                      # every house on the server
/ha goto <player> [slot]      # or a house id
```

`/ha goto` takes a player and a slot the way `/h goto` does. A house id still works, because `/ha list` prints them.

## Admin Mode

Holding `bluehousing.admin.bypass.access` or `.build` does **not** apply it. The permission grants the right to turn the mode on; arriving through `/h goto`, Discovery or `/h search` is arriving as a visitor.

```
/ha mode on|off        # for the house you are standing in
/ha goto <player>      # arms it, and arrival switches it on
```

Admin mode is per player **and per house**, lives only in memory, is dropped when you leave that house or disconnect, re-checks the permission on every call, and shows a boss bar, because the difference is otherwise invisible until something breaks.

Inside a house without it, `/ha` and the subcommands that default to "the house you are in" (`info`, `delete`, `backup`, `feature`) answer with a visitor notice. `mode`, `goto`, `menu`, `template`, `list`, `economy`, `music` and `setlobby` still work.

While it is on:

- The whitelist and the bans do not apply
- The build flags do not apply
- Moderation menus act on the house you are in

## Text Filter

Anything a player writes for other people to read is checked at the write: house name and description, scoreboard lines, hologram lines, NPC names, entry titles and moderation reasons.

```yaml
filters:
  words:
    - puta
  patterns: []          # raw regexes, for a rule set you already trust
  link_whitelist: []
```

Each configured word is compiled into a rule rather than compared as a string. One entry covers the word and the ways round it: accents, the usual substitutions (`4`=a, `3`=e, `0`=o, `1`=i, `$`=s, `7`=t), repeated letters, punctuation between letters and the plural. So `puta` catches `PUT4`, `p.u.t.a` and `puuuta` without catching `disputa`, and `hijo de puta` also catches `hijodeputa`.

Links are refused separately, and a domain is only an address when its ending is a real one: `puerta.norte` is a house name, `play,example,net` is an address written to get past a filter that only knows about dots.

**There is no permission that skips the filter.** An exception is written down in `link_whitelist`.
