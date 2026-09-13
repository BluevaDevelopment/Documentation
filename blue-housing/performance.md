# Performance

A house is a creative world its owner may build anything in, and that includes the things that stop a server: a redstone clock feeding a chain of repeaters, a sand duplicator, a hopper loop, a farm nobody collects.

**A lag machine and a working farm are the same blocks.** So this is a budget, not a ban: a house may do as much as the server can afford in a tick, and what it asks for past that is refused for a moment and then allowed again.

```yaml
performance:
  sweep_seconds: 15
```

## Redstone

```yaml
  redstone:
    enabled: true
    max_updates_per_tick: 800
    window_seconds: 5
    freeze_after_ticks: 20
    freeze_seconds: 10
```

Measured **per tick, per house**; a piston firing counts as one. An update past the budget is dropped by handing the old current back, because the event is not cancellable.

One expensive tick is a piston door and nobody's problem, so a **freeze only follows repetition**: `freeze_after_ticks` ticks over budget inside `window_seconds` stops that house's redstone for `freeze_seconds`. It is announced to whoever is standing in the house, because silence there reads as the build breaking.

`freeze_after_ticks: 0` never freezes and only drops what is over budget.

## Entities

```yaml
  entities:
    enabled: true
    cull_excess: true
    limits:
      items: 400
      falling_blocks: 200
      mobs: 150
      tnt: 60
      projectiles: 100
      vehicles: 80
      armor_stands: 200
      orbs: 200
      other: 200
```

Capped **per family**, not by one total. A thousand dropped items, a thousand falling sand blocks and a thousand armour stands are three different builds with the same symptom, and a single number cannot tell a decorated house from a duplicator in a basement. `0` means no limit for that family.

The count is kept by the spawns and put straight by a sweep every `sweep_seconds`, because asking the world for its entity list on every spawn is O(n) exactly when n is the problem. The sweep also removes what is already over a cap (`cull_excess`), or a house that arrived over its limit would refuse every new spawn for ever while the old entities sat there.

> A refused **falling block is written back as a block**, not deleted. Minecraft has already replaced the block with the entity by the time the spawn is seen, so cancelling on its own would quietly eat somebody's build.

## Scope

All of it is keyed by the world's uid, resolved when the house loads, so the hot path is one map lookup and never a string comparison. A world with no entry is not a house and costs nothing.

Everything here is memory only and dropped on unload: the budget is about what the server is being asked to do this second, not about what a house has ever done.
