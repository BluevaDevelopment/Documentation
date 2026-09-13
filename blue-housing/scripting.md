# Scripting

Every house can run Lua. The runtime is sandboxed, per house, and reloaded whenever a file changes.

For the function list, see the [Lua API](lua-api.md).

## The Web Editor

Scripts are edited at [housing.blueva.net](https://housing.blueva.net) (or your own instance, via `webeditor.editor_url`).

```
/h code
```

That generates a **local**, one-use `xxx-xxx-xxx` code, valid for 20 minutes. Nothing is sent anywhere when you run it; the editor redeems it against your server. There are no tokens to set up: the server registers itself on first use and keeps its identity in `data/webeditor/identity.json`.

**The editor stores nothing.** It is a stateless proxy: the plugin holds every project, file, Blockly workspace and version, and the editor translates browser requests into RPC over the server's websocket. Server offline means the house is not editable, by design, so there is never a second copy to disagree.

### Who may edit

The house **owner**, plus anybody holding a role listed in `webeditor.editor_roles` (`trusted` by default). Somebody trusted with a house is trusted with its scripts; the alternative is the owner pasting Lua into chat for them.

Whoever edits, the files stay the owner's: everything is read and written under `data/houses/<owner>/`.

### Two people at once

Being trusted with a house is being trusted with its scripts, so two tabs on one file is normal rather than rare. Three things handle it, and none pretends to be more than it is:

- **Saving is optimistic.** The editor sends the version its tab opened at, and a save over somebody else's is refused with a conflict that carries the current text back, so the editor can offer "keep mine" or "take theirs" instead of losing work. Identical text is never a conflict.
- **Presence** rides on the house channel: who is here, and what file each of them is on.
- **What this is not** is character-by-character co-editing. There is no OT or CRDT; the plugin stores whole files, and two people typing in the same function still resolve it at save time. The guarantee is that nobody's work disappears silently.

### Version history

Every save that actually changes the Lua is versioned, rotated to `webeditor.history_limit` (20). Versions can be listed and read back from the editor.

## Projects

A house has one or more projects, each a folder of `.lua` files.

| Rule | Why |
|------|-----|
| The default project cannot be deleted or renamed | It is where a house starts |
| `main.lua` cannot be deleted or renamed | Same |
| The last remaining project cannot be deleted | A house with no scripts has nowhere to put one |

Projects can be disabled without deleting them. Cross-project communication is house-wide only, through `housing.emit`, with loop guards.

A new house is seeded with `main.lua` and `menu_item.lua`; the second is what gives the owner and trusted players the item that opens the house menu.

## Blockly or Lua

The editor offers both. Blocks generate Lua; the file on disk is always Lua, and a `.lua.blockly.json` sidecar remembers the workspace.

The block palette is generated from the API reference, so a function that exists has a block. The **House** and **Player** categories carry the hand-made blocks first, which have their arguments filled in, and the rest of the reference underneath.

## Custom Commands

```lua
housing.command("start", function(player)
  player:message("<green>Off you go!")
end)
```

Players in the house run it with `/h run start`. The **Run a Script Command** menu lists whatever the runtime holds right now, one lever each; a command that takes arguments still has to be typed.

## Limits

| | |
|---|---|
| **Instruction limit** | A script that runs away is stopped rather than hanging the server |
| **Fail closed** | When a script errors or hits the limit, `block_break`, `block_place` and `interact` are **denied** rather than allowed |
| **No console** | There is no binding that reaches the console. A house is a thing a player is given, its scripts are written by its owner, and the console is the one caller every permission check answers yes to |
| **Player commands** | `player:runCommand` is off by default (`scripting.player_commands`). It runs as the player, so it can only do what they could have typed |
| **Giving money** | `scripting.economy.allow_deposit` is `owner` by default: a prize comes out of the house owner's balance. `true` lets scripts mint currency; `false` refuses |

Each project gets its own console, shipped to the web editor, not to the server console.

## The Lua Dialect

Lua **5.5**. Integers are a distinct subtype, `/` always yields a float (`10/2` is `5.0`), `//` and the bitwise operators exist, `utf8` is available, and `bit32`, `math.pow`, `math.ldexp` and `math.frexp` are gone.

## Reloading

The runtime reloads after a write that changed the Lua, a file created, renamed or deleted, a project created, deleted or toggled. Renaming a project does not reload, because the name is not part of the runtime.
