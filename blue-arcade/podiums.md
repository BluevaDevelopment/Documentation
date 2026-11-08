# Podiums & Tie-Breaks

Since 3.6.0, party matches end their rounds and the match itself with a **physical podium ceremony**, ties for the championship are broken with **extra minigames or a reaction challenge**, and tied players **share their place** with equal stars and rewards.

Podiums and tie-breaks work in **dynamic and virtual arenas only**. Static arenas never enter the ceremony flow.

## Physical Podiums

There are two ceremonies:

- **Between games** (`between_games`): after each minigame of a party match, before the next one starts.
- **Final** (`final`): at the end of the match, for the top three of the championship.

Winners are represented by **NPC statues skinned as the winners**, standing on a three-place stage (first place in the center). Each statue is outlined and its name colored in its place's color: **gold** for first, **white** for second, **bronze** for third. If players are tied for a place, they share the slot: every tied name is shown and the NPCs alternate every couple of seconds.

Viewers watch the ceremony from a fixed camera or, when `fixed_camera` is off, can fly around the venue (only the winners fly). Fireworks and the winners' selected store victory effects can be played, and between games the players are briefly blinded while the next game is announced so the title stands out.

## Configuration

All defaults live under `podiums` in `settings.yml`:

```yaml
podiums:
 # Ceremony between games (rounds) of a multi-game match.
 between_games:
  enabled: true
  # true: every viewer watches from the fixed camera. false: viewers can fly around the venue.
  fixed_camera: true
  fireworks: true
  # Plays each winner's selected store victory effect (needs store.enabled).
  winner_effects: true
  # Size of the podium NPCs relative to a player. Needs Minecraft 1.20.6+; older servers show them at normal size.
  npc_scale: 2.5
  # Outlines each NPC and colours its name in its place's colour (gold, white, bronze).
  silhouette: true
  # Blinds players while the next game is announced on the podium, so the title stands out.
  # The effect ends when they move to the next game.
  next_game_blindness: true
 # Ceremony at the end of the match.
 final:
  enabled: true
  fixed_camera: false
  fireworks: true
  winner_effects: true
  npc_scale: 2.5
  silhouette: true
```

| Option | Description |
|--------|-------------|
| `enabled` | Run the ceremony. Disabling it keeps chat rewards and the current map |
| `fixed_camera` | Everyone watches from the configured camera; off means free flight for viewers |
| `fireworks` | Decorative fireworks during the ceremony |
| `winner_effects` | Play each winner's selected store victory effect (requires the store) |
| `npc_scale` | Size of the podium NPCs relative to a player (needs Minecraft 1.20.6+) |
| `silhouette` | Outline each NPC and color its name by place |
| `next_game_blindness` | Brief blindness while the next game is announced (between games only) |

### Per-Arena Overrides

Each arena can override `enabled`, `fixed_camera`, `fireworks` and `winner_effects` (not `npc_scale`) for either ceremony:

```
/baa arena podium set <between_games|final> <enabled|fixed_camera|fireworks|winner_effects> <on|off|default>
```

`default` removes the override and falls back to `settings.yml`. The arena must be disabled, and static arenas are rejected.

## Custom Podium Maps

Arenas that don't have a custom podium map use the bundled default maps. To build your own:

```
/baa arena select [id]
/baa arena podium edit        # opens the three-place podium map
/baa arena podium npc 1       # stand at the first-place spot: saves feet position and facing
/baa arena podium npc 2
/baa arena podium npc 3
/baa arena podium camera      # stand/look as the ceremony camera: saves eye position and direction
/baa arena podium save        # saves the map (exit also leaves without saving changes)
```

Additional commands:

| Command | Description |
|---------|-------------|
| `/baa arena podium reset` | Reset every saved position |
| `/baa arena podium round npc <1-3>` / `round camera` | Position used only by the between-games ceremony |
| `/baa arena podium final npc <1-3>` / `final camera` | Position used only by the final ceremony |
| `/baa arena podium round reset` / `final reset` | Clear the per-ceremony overrides |

Setting a common point (`npc`, `camera`) clears that point's round/final overrides. While editing, each saved position shows a **private hologram label and a colored beacon beam**, visible only to you; no real blocks or entities are placed, so the exported map stays clean. See [Creating Arenas](arenas.md) for how the editor markers work.

## Championship Tie-Breaks

When players tie for the **top three** of a party match, the championship is decided with extra rounds instead of sharing the title:

- Only the tied players competing for a top-three slot play; the rest spectate.
- Extra rounds use short minigames from the configured pool, and award **no extra stars or rewards**.
- After the configured number of extra rounds (or when no eligible minigame is available), the tie is broken with the **reaction challenge**: a map-independent Core challenge where players release sneak and must sneak again the moment GO appears. Equal reactions share the result and play again; players who don't respond in time are ranked behind the responders.

Tie-breaks only exist for **party matches** on dynamic and virtual arenas, and they work with or without physical podiums. In virtual arenas, the tie-break minigames must also be universal modules.

### Configuration

Defaults live under `tie_break` in `settings.yml`:

```yaml
tie_break:
 # Opt in only short individual microgames that work with two players. Disabled/unconfigured games are ignored.
 games: [spleef, water_well, fast_zone]
 # After this many extra module rounds, use the map-independent Core reaction challenge.
 # The reaction challenge is also used when no eligible module is available.
 max_game_rounds: 5
```

Per-arena overrides (arena must be disabled, not static):

```
/baa arena tiebreak games <id...|default>
/baa arena tiebreak rounds <n|default>
```

Game ids are validated against the minigames configured in the arena; `default` removes the override.

## Shared Round Places

Independent of podiums and tie-breaks, tied players in a round now **share their place** using dense ranking:

- Tied players get the same place number, the same stars, and the same reward commands for that place.
- Dense ranking does not consume the following places: a tie of two winners at first place still leaves the next player in second (`1, 1, 2, 3`), and stars follow the shared rank (`3, 3, 2, 1` for a four-player round).
- On the podium stage, tied winners share one slot and their NPCs alternate.
- A player who earned a result keeps their place and rewards even if they leave before the round ends.

Universal modules can report tied groups explicitly; in elimination games, declared winners are never displaced by other survivors.
