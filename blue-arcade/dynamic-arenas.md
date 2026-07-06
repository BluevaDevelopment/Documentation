# Why Blue Arcade Is Moving to Dynamic Arenas

Blue Arcade is moving toward **dynamic arenas** as the recommended and default arena system. Static arenas still work for backward compatibility, but they are considered a legacy system and will be deprecated in a future release.

This change is not only about performance. It also makes Blue Arcade easier to maintain, easier to debug, and ready for features that are not practical with static arenas.

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

The replay system planned for 3.4 branch is designed around isolated match worlds. Dynamic arenas make it easier to capture, reproduce, and inspect a match without the noise of a persistent world that may have been modified before or after the game.

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

Blue Arcade includes a migration command that converts a legacy static arena into dynamic templates.

First run:

```text
/baa migrate <arena_id>
```

Blue Arcade will validate the arena and print a temporary confirmation command. Then run the confirmation within the time limit:

```text
/baa migrate <arena_id> confirm
```

The migration process moves the old arena data folder into the Blue Arcade backup folder and creates new dynamic templates for the waiting lobby and configured games. The arena is left disabled after migration so you can test it before enabling it again.

## Recommendation

If you are creating a new arena, use the dynamic type.

If your server still has static arenas, start migrating them gradually. Static arenas will continue to work for now, but future Blue Arcade development is focused on dynamic arenas.
