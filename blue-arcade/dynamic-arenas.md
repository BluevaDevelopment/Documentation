# Why Blue Arcade Is Moving to Dynamic Arenas

Blue Arcade is moving toward **dynamic arenas** as the recommended and default arena system. Since 3.5.0 new static arenas can no longer be created. Since **3.6.0** the ones you already have are **read-only**: setup commands refuse to edit or clone them until you migrate them to dynamic, although their matches keep running. Static support ends in **3.7.0**, so migrate them while there is time.

This change is not only about performance. It also makes Blue Arcade easier to maintain, easier to debug, and ready for features that are not practical with static arenas: physical podiums, championship tie-breaks and replays all require dynamic (or virtual) arenas.

## Static vs Dynamic Arenas

### Static Arenas

A static arena runs inside a persistent world. After a match, Blue Arcade has to restore the arena by placing blocks back into the same world.

This works, but it means the plugin has to keep track of changed regions, restore blocks carefully, handle chunk loading, and avoid doing too much work in a single tick.

### Dynamic Arenas

A dynamic arena runs in a fresh temporary world created from a saved template. When the match ends, the temporary world is deleted. The next match starts from a clean copy again.

Instead of repairing the same world over and over, Blue Arcade starts from a clean template every time.

## Why Static Arenas Are Being Deprecated

Maintaining two arena systems is expensive and fragile:

- Static arenas need block snapshotting, regeneration queues, chunk loading, world safety checks, and special edge-case handling.
- Dynamic arenas use a simpler lifecycle: create world, run match, delete world.
- Every new arena feature has to be designed twice when both systems are treated equally.
- Bugs are harder to reproduce because static arenas depend on the current state of a persistent world.
- Modern Blue Arcade features are built around isolated match worlds.

Static arenas do not provide many real advantages over dynamic arenas anymore. They mainly exist because older servers were created before dynamic arenas became the default.

## Performance Difference

Static regeneration time depends on how many blocks must be restored and how many blocks Blue Arcade is allowed to process per tick.

A simple estimate is:

```text
static regeneration time = (changed blocks / blocks per tick) / 20 seconds
```

For example, if an arena has **500,000 blocks** to restore and the server restores **2,000 blocks per tick**:

```text
500,000 / 2,000 = 250 ticks
250 / 20 = 12.5 seconds
```

For **5,000,000 blocks**:

```text
5,000,000 / 2,000 = 2,500 ticks
2,500 / 20 = 125 seconds
```

Dynamic arenas avoid that block-by-block repair step. The internal template copy/selection step is typically **under 1 ms**; the remaining time is normal world loading and disk work.

Using that sub-1 ms template operation as the comparison point:

- 500,000 changed blocks: `12.5s / 0.001s = 12,500x faster`
- 5,000,000 changed blocks: `125s / 0.001s = 125,000x faster`

The exact number depends on server hardware, disk speed, world size, and configuration, but the important point is the same: **dynamic arenas scale much better because Blue Arcade does not need to repair millions of blocks one by one.**

## Features That Require Dynamic Arenas

Dynamic arenas unlock features that are either impossible or much harder to support safely with static arenas.

### Arena On Demand

Arena On Demand can create extra runtime instances when more players need to join. This only works cleanly when each match can run in its own temporary world.

With static arenas, every extra instance would need its own manually prepared persistent world.

### Replay System

The [replay system](replay-system.md) is built around isolated match worlds. A replay is rebuilt inside a fresh copy of the arena template, which is only possible when the match ran in its own world instead of a persistent one that may have been modified before or after the game. Static arenas are not recorded.

### Easier Debug Sharing

Dynamic arenas can be packaged and shared more easily. If a server owner reports a bug, the relevant arena template can be sent for debugging without also sending a full persistent server world.

This makes support faster and makes bugs easier to reproduce.

### Faster Feature Iteration

When Blue Arcade only has to target one modern arena lifecycle, new features can be added faster and with fewer compatibility problems.

Dynamic arenas make it easier to improve:

- private games
- hosted events
- arena cloning
- temporary test instances
- per-match world rules
- replay/debug tooling
- future scaling systems

## Do Static Arenas Have Advantages?

Static arenas can feel familiar if your server was built around one persistent world. They also avoid creating temporary worlds during the match lifecycle.

However, for most servers, dynamic arenas are better:

- safer resets
- cleaner match isolation
- faster recovery after games
- easier debugging
- better support for future features
- better scaling with Arena On Demand

For that reason, static arenas are now considered legacy.

## Migrating a Static Arena

Blue Arcade includes a migration command that converts a legacy static arena into dynamic templates. It runs **from the console**, for safety:

```text
/baa arena migrate <arena_id> dynamic
```

Blue Arcade will validate the arena and print a temporary confirmation command. Then run the confirmation within 30 seconds:

```text
/baa arena migrate <arena_id> dynamic confirm
```

The migration process moves the old arena data folder into the Blue Arcade backup folder (`plugins/BlueArcade3/backup/`) and creates new dynamic templates for the waiting lobby and configured games. The arena is left disabled after migration so you can test it before enabling it again.

Migrated dynamic arenas can later be converted to the experimental [virtual arena](virtual-arenas.md) type with `/baa arena migrate <arena_id> virtual`, also from the console and with backups.

## Recommendation

New arenas are dynamic by default, so there is nothing to choose. Dynamic arenas remain the recommended type for production; the experimental virtual arenas are not a replacement yet.

If your server still has static arenas, migrate them now. They keep running matches until **3.7.0**, but they can no longer be edited or cloned, and after 3.7.0 they will not load.
