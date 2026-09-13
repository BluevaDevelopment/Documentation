# Members & Roles

Who may enter a house, and what they may do once inside.

## Access

| | Who |
|---|---|
| **Owner** | Whoever the house folder belongs to. Never an argument; it is resolved from the data layout |
| **Trusted** | `/h trust <player>`. Full build access, and by default access to the scripts |
| **Added** | `/h add <player>`. Allowed in, limited by the `added` role |
| **Whitelisted** | Allowed in, no extra rights |
| **Visitor** | Everybody else |

```
/h trust <player>        /h untrust <player>
/h add <player>          /h remove <player>
/h members               # the member list; click a head to change their role
```

### Whitelist

```
/h whitelist on|off
/h whitelist add <player>
/h whitelist remove <player>
/h whitelist list
/h whitelist status
```

**The whitelist is what decides whether a house is listed in Discovery.** There is no separate "public" switch: whitelist off means anybody may come, and the house appears in the directory.

An untouched house is private: a missing setting reads as enabled.

---

## Roles

A role is house data, not code: it lives in `house.json` and is edited from `/h role` or the Roles menu.

```
/h role set <player> <role>
/h role remove <player>
/h role list
/h role create <name>          # [a-z0-9_], up to 24 characters
/h role delete <name>
/h role info <name>
/h role override <role> <flag> <allow|deny|owner_online|inherit>
/h role priority <role> <n>
/h role chatprefix <role> <text>
/h role chatsuffix <role> <text>
/h role tabprefix <role> <text>
/h role tabsuffix <role> <text>
```

Four roles exist in every house whether or not anything is stored: `owner`, `trusted`, `added`, `visitor`. Their built-in rules are editable like any other.

### Overrides

A role overrides a **flag**, with one of four answers:

| Answer | Meaning |
|--------|---------|
| `allow` | Yes, whatever the house says |
| `deny` | No, whatever the house says |
| `owner_online` | Yes while the house owner is online |
| `inherit` | Fall through to the house flag |

`inherit` is stored rather than removed, because clearing a built-in default has to be able to say so.

Overridable flags are the ones a player action runs into:

```
break  place  interact  pickup  chest-access  door-interact  lever-interact  pvp  chat
```

Not `use` (no listener enforces it) and not the house-wide flags, which cannot mean one thing for one visitor and another for the next.

### Prefixes

`chatprefix`, `chatsuffix`, `tabprefix` and `tabsuffix` show the role in chat and in the tab list. `priority` decides which role wins when a player holds more than one.

---

## Regions

A drawn box inside the house that overrides flags for the area.

```
/h region wand                       # get the selection wand
/h region pos1 / pos2                # or mark where you stand
/h region create <id>
/h region redefine <id>
/h region delete <id>
/h region list
/h region info <id>
/h region here
/h region priority <id> <n>
/h region flag <id> <flag> <value>
/h region role <id> <role> <flag> <allow|deny|owner_online|inherit>
/h region check
```

The wand marks **the block you are standing in**, so you fly to the corner and click rather than ray-tracing at a distance. Swapping hands clears the selection, and that is the only thing that does: creating or redefining a region keeps it, so the box stays on screen while you check it landed where you meant, and a second region can be cut from the same selection.

The selection and both corners are drawn to the player who made them, so three builders in a house do not see three boxes.

---

## Precedence

The most specific answer wins:

```
region + role  →  region  →  role  →  house flag
```

This order matters. Asking the role first made regions dead weight, because the built-in roles override nearly every flag, so a drawn "no building here" never got a word in.

`regions.<id>.role_flags.<role>.<flag>` is the layer that makes *"the build team may break blocks in the arena, and nobody else may"* expressible.

### When an override looks ignored

Two cases, both invisible, both reported as bugs:

- An override written for `trusted` says nothing to the **owner**, whose role is `owner`
- **Admin mode bypasses the chain by design**

`/h region check` prints the answer and the layer that decided it, and warns when admin mode is on.
