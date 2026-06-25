# Set up a mini-game

**Note:** You must first have created an arena before configuring any minigame. See [how to create an arena](https://blueva.net/docs/blue-arcade-2/create-an-arena).

---

## Adding a Minigame to Your Arena

Before configuring a minigame, you need to add it to your arena:

```
/baa game [arena_id] add [minigame]
```

**Example:**
```
/baa game 1 add spleef
```

**Available minigames:**
- `race`
- `spleef`
- `snow_ball_fight`
- `all_against_all`
- `one_in_the_chamber`
- `traffic_light`
- `mine_field`
- `exploding_sheep`
- `tnt_tag`
- `red_alert`
- `knock_back`
- `fast_zone`
- `run_from_the_beast`
- `block_party`
- `tnt_run`

After adding, the minigame will be registered but **DISABLED** until you complete its configuration.

---

## Common Setup Steps (All Minigames)

Every minigame requires these three basic configurations:

### 1. Set Game Boundaries

The boundaries define the playable area. Players who leave these bounds will die or be teleported back.

**Get the Magic Stick:**
```
/baa stick
```

**Select the area:**
- **Left-click** a block to set position 1 (minimum corner)
- **Right-click** a block to set position 2 (maximum corner)

**Save the boundaries:**
```
/baa game [arena_id] [minigame] bounds set
```

**Example:**
```
/baa game 1 spleef bounds set
```

**Tips:**
- Select opposite corners of your map (diagonal selection)
- Make sure both positions are in the same world
- The selection will show you the total number of blocks

---

### 2. Add Spawn Points

Spawn points are where players appear when the minigame starts.

**Requirements:**
- You must add **at least as many spawns as your arena's maximum players**
- Example: If max players is 12, add at least 12 spawns

**Add a spawn at your current location:**
```
/baa game [arena_id] [minigame] spawn add
```

**Example:**
```
/baa game 1 spleef spawn add
```

**Important:**
- Your current position AND viewing direction will be saved
- Players will spawn facing the direction you were facing
- Repeat this command for each spawn point needed

**Managing spawns:**
```
/baa game [arena_id] [minigame] spawn list          # View all spawns
/baa game [arena_id] [minigame] spawn remove [#]    # Remove a spawn
/baa game [arena_id] [minigame] spawn teleport [#]  # Teleport to a spawn
```

---

### 3. Set Game Duration

Configure how long the minigame will last before ending:

```
/baa game [arena_id] [minigame] time [minutes]
```

**Example:**
```
/baa game 1 spleef time 5
```

**Requirements:**
- Must be between 1-60 minutes
- Default is 5 minutes if not set

---

### 4. Set Death Block (Optional)

Configure which block kills players when they touch it:

```
/baa game [arena_id] [minigame] deathblock [material]
```

**Example:**
```
/baa game 1 spleef deathblock LAVA
```

**Common options:**
- `LAVA` - Players die when stepping on lava
- `WATER` - Players die when stepping on water
- `AIR` - Players die when falling (useful for void maps)
- `BARRIER` - Players die when touching barrier blocks

---

## Minigame-Specific Configuration

### Race

**Requirements:** Common steps + finish line

#### Set Finish Line

```
/baa stick
```
- Left-click one corner of the finish line area
- Right-click the opposite corner

```
/baa game [arena_id] race finishline set
```

**Example:**
```
/baa game 1 race finishline set
```

Players who enter this area will be classified and finish the race.

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Finish line set
- [ ] Enable minigame: `/baa game [id] race enable`

---

### Spleef

**Requirements:** Common steps + floor area

#### Set Floor Area

```
/baa stick
```
- Left-click one corner of the floor
- Right-click the opposite corner

```
/baa game [arena_id] spleef floor set
```

**Example:**
```
/baa game 1 spleef floor set
```

**Important:**
- Floor must be made entirely of snow blocks
- Must be a single layer
- Area below the floor must be empty (void or deep pit)

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Floor set (snow blocks)
- [ ] Enable minigame: `/baa game [id] spleef enable`

---

### Snowball Fight

**Requirements:** Common steps only

This minigame only needs bounds, spawns, and time. No additional configuration required.

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [ ] Enable minigame: `/baa game [id] snow_ball_fight enable`

---

### All Against All

**Requirements:** Common steps only

This minigame only needs bounds, spawns, and time. No additional configuration required.

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [ ] Enable minigame: `/baa game [id] all_against_all enable`

---

### One In The Chamber

**Requirements:** Common steps + arrow removal setting

#### Configure Arrow Removal

```
/baa game [arena_id] one_in_the_chamber removearrows <true|false>
```

**Example:**
```
/baa game 1 one_in_the_chamber removearrows true
```

**Options:**
- `true`: Arrows disappear on hit (classic mode, more challenging)
- `false`: Arrows can be picked up (easier mode)

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Arrow removal configured
- [ ] Enable minigame: `/baa game [id] one_in_the_chamber enable`

---

### Traffic Light

**Requirements:** Common steps + finish line

#### Set Finish Line

```
/baa stick
```
- Left-click one corner of the finish line area
- Right-click the opposite corner

```
/baa game [arena_id] traffic_light finishline set
```

**Example:**
```
/baa game 1 traffic_light finishline set
```

Players who enter this area will be classified and finish the race.

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Finish line set
- [ ] Enable minigame: `/baa game [id] traffic_light enable`

---

### Mine Field

**Requirements:** Common steps + finish line + floor area

#### Set Finish Line

```
/baa stick
```
- Left-click one corner of the finish line area
- Right-click the opposite corner

```
/baa game [arena_id] mine_field finishline set
```

**Example:**
```
/baa game 1 mine_field finishline set
```

#### Set Floor Area

```
/baa stick
```
- Left-click one corner of the floor
- Right-click the opposite corner

```
/baa game [arena_id] mine_field floor set
```

**Example:**
```
/baa game 1 mine_field floor set
```

**Important:**
- Floor must be a single layer of blocks
- Mines will be randomly placed on this floor during gameplay

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Finish line set
- [x] Floor set (single layer)
- [ ] Enable minigame: `/baa game [id] mine_field enable`

---

### Exploding Sheep

**Requirements:** Common steps + explosion level

#### Set Explosion Level

```
/baa game [arena_id] exploding_sheep explosionlevel [level]
```

**Example:**
```
/baa game 1 exploding_sheep explosionlevel 2.0
```

**Requirements:**
- Level must be between 0.5-10.0
- Higher values = bigger explosions
- Recommended: 1.5-3.0 for balanced gameplay

**Map requirements:**
- Map should have shallow block depth (2-3 blocks recommended)
- Area below the map must be empty so players can fall

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Explosion level set
- [ ] Enable minigame: `/baa game [id] exploding_sheep enable`

---

### TNT Tag

**Requirements:** Common steps only

This minigame only needs bounds, spawns, and time. No additional configuration required.

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [ ] Enable minigame: `/baa game [id] tnt_tag enable`

---

### Red Alert

**Requirements:** Common steps + floor area + difficulty settings

#### Set Floor Area

```
/baa stick
```
- Left-click one corner of the floor
- Right-click the opposite corner

```
/baa game [arena_id] red_alert floor set
```

**Example:**
```
/baa game 1 red_alert floor set
```

**Important:**
- Floor must be made of white wool blocks
- The floor will change colors during gameplay

#### Configure Particles

```
/baa game [arena_id] red_alert particles <true|false>
```

**Example:**
```
/baa game 1 red_alert particles true
```

**Options:**
- `true`: Show particle effects when floor changes
- `false`: No particles (better performance)

#### Set Color Change Chance

```
/baa game [arena_id] red_alert colorchance [percentage]
```

**Example:**
```
/baa game 1 red_alert colorchance 2.0
```

**Requirements:**
- Must be between 0.1-10.0
- Lower = changes less often (easier)
- Higher = changes more often (harder)
- Recommended: 1.0-3.0

#### Set Difficulty Increase

```
/baa game [arena_id] red_alert increasechance [amount]
```

**Example:**
```
/baa game 1 red_alert increasechance 1
```

**Requirements:**
- Must be between 0-5
- 0 = no increase (static difficulty)
- Higher = gets harder faster
- Recommended: 1-2

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Floor set (white wool)
- [x] Particles configured
- [x] Color change chance set
- [x] Difficulty increase set
- [ ] Enable minigame: `/baa game [id] red_alert enable`

---

### Knock Back

**Requirements:** Common steps only

This minigame only needs bounds, spawns, and time. No additional configuration required.

**Map requirements:**
- Map should have a flat surface with holes that players can fall into
- Area below must be empty (void or deep pit)

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [ ] Enable minigame: `/baa game [id] knock_back enable`

---

### Fast Zone

**Requirements:** Common steps + finish line

#### Set Finish Line

```
/baa stick
```
- Left-click one corner of the finish line area
- Right-click the opposite corner

```
/baa game [arena_id] fast_zone finishline set
```

**Example:**
```
/baa game 1 fast_zone finishline set
```

Players who enter this area will be classified and finish the race.

**Map requirements:**
- Parkour or obstacle course with speed effects
- Clear path from start to finish

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Finish line set
- [ ] Enable minigame: `/baa game [id] fast_zone enable`

---

### Run From The Beast

**Requirements:** Common steps + beast spawn + beast zone

#### Set Beast Spawn Point

Stand where you want the beast to spawn and execute:

```
/baa game [arena_id] run_from_the_beast beastspawn
```

**Example:**
```
/baa game 1 run_from_the_beast beastspawn
```

**Important:**
- Your current position AND viewing direction will be saved
- The beast will spawn facing this direction

#### Set Beast Zone

This is the area where the beast is initially trapped:

```
/baa stick
```
- Left-click one corner of the beast zone
- Right-click the opposite corner

```
/baa game [arena_id] run_from_the_beast beastzone set
```

**Example:**
```
/baa game 1 run_from_the_beast beastzone set
```

**Map requirements:**
- Parkour or obstacle course leading to the beast zone
- Beast zone should have blocks that can be broken by players
- Place chests with weapons/armor along the parkour for players to collect
- Players must break the beast zone blocks to release the beast

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Beast spawn point set
- [x] Beast zone set
- [ ] Enable minigame: `/baa game [id] run_from_the_beast enable`

---

### Block Party

**Requirements:** Common steps + floor area + patterns + timing settings

#### Set Floor Area

```
/baa stick
```
- Left-click one corner of the floor
- Right-click the opposite corner

```
/baa game [arena_id] block_party floor set
```

**Example:**
```
/baa game 1 block_party floor set
```

**Important:**
- Floor should be a flat square platform
- Area below the floor must be empty so players can fall

#### Add Floor Patterns

You need **at least 5 different patterns** for Block Party to work properly.

**Method 1: Build on the floor and capture**
1. Build your desired pattern directly on the floor you set earlier
2. Use the Magic Stick to select the area (if different from the full floor)
3. Add the pattern:
```
/baa game [arena_id] block_party pattern add
```

**Method 2: Auto-capture the entire floor**
If you don't select anything with the Magic Stick, it will automatically capture the entire floor area.

**Example workflow:**
```
# Add pattern 1
(Build pattern on floor with different colored blocks)
/baa game 1 block_party pattern add

# Add pattern 2
(Build different pattern on floor)
/baa game 1 block_party pattern add

# Continue until you have at least 5 patterns

# List all patterns
/baa game 1 block_party pattern list
```

**Managing patterns:**
```
/baa game [arena_id] block_party pattern list         # List all patterns
/baa game [arena_id] block_party pattern view [#]     # Preview a pattern
/baa game [arena_id] block_party pattern remove [#]   # Remove a pattern
```

#### Configure Music Time

How long does the music play before searching for a new block.

```
/baa game [arena_id] block_party musictime [seconds]
```

**Example:**
```
/baa game 1 block_party musictime 3
```

**Requirements:**
- Must be between 1-30 seconds
- Recommended: 1-3 seconds

#### Configure Search Time

How long players have to find and stand on the correct block:

```
/baa game [arena_id] block_party searchtime [seconds]
```

**Example:**
```
/baa game 1 block_party searchtime 7
```

**Requirements:**
- Must be between 3-20 seconds
- Recommended: 5-10 seconds

#### Configure Decrease Time

How much faster each round gets (time reduction per round):

```
/baa game [arena_id] block_party decreasetime [seconds]
```

**Example:**
```
/baa game 1 block_party decreasetime 0.5
```

**Requirements:**
- Must be between 0.1-3.0 seconds
- Recommended: 0.5-1.5 seconds

**Configuration checklist:**
- [x] Bounds set
- [x] Spawns added
- [x] Game duration set
- [x] Floor set
- [x] At least 5 patterns added
- [x] Music time set
- [x] Search time set
- [x] Decrease time set
- [ ] Enable minigame: `/baa game [id] block_party enable`

---

### TNT Run

**Requirements:** Common steps + floor layers + timing settings + double jump configuration

TNT Run is a survival minigame where blocks disappear beneath players' feet as they run. Players must keep moving to avoid falling through the floor!

#### Set Floor Delay

Configure how quickly blocks disappear after players step on them:

```
/baa game [arena_id] tnt_run floor delay [ticks]
```

**Example:**
```
/baa game 1 tnt_run floor delay 10
```

**Requirements:**
- Must be between 1-100 ticks
- 20 ticks = 1 second
- Recommended: 5-15 ticks (faster = harder)
- Default: 10 ticks

**Tips:**
- Lower values (5-8 ticks) = very challenging
- Medium values (10-12 ticks) = balanced
- Higher values (15-20 ticks) = easier for beginners

#### Add Floor Layers

TNT Run requires **at least 1 floor layer**. You can add multiple layers that stack vertically, creating a multi-level experience.

**Method: Select and add a floor layer**

```
/baa stick
```
- Left-click one corner of the floor layer
- Right-click the opposite corner

```
/baa game [arena_id] tnt_run floor add
```

**Example workflow:**
```
# Add first floor layer (top level)
(Select the top floor with Magic Stick)
/baa game 1 tnt_run floor add

# Add second floor layer (middle level)
(Select the middle floor with Magic Stick)
/baa game 1 tnt_run floor add

# Add third floor layer (bottom level)
(Select the bottom floor with Magic Stick)
/baa game 1 tnt_run floor add

# List all floor layers
/baa game 1 tnt_run floor list
```

**Important floor layer guidelines:**
- Each floor should be a single layer of solid blocks
- Floors should be stacked vertically with space between them
- Use different materials for visual variety (colored terracotta, concrete, etc.)
- Leave bedrock or barrier blocks at the very bottom to prevent falling into the void
- Recommended: 2-3 floor layers for balanced gameplay

**Managing floor layers:**
```
/baa game [arena_id] tnt_run floor list            # List all floors
/baa game [arena_id] tnt_run floor remove [#]      # Remove a specific floor
```

#### Configure Auto-Removal for Floor Layers (Optional)

You can configure individual floor layers to automatically disappear after a certain time, increasing difficulty:

```
/baa game [arena_id] tnt_run floor removal [floor_number] [seconds]
```

**Example:**
```
# Remove floor 1 (top layer) after 60 seconds
/baa game 1 tnt_run floor removal 1 60

# Remove floor 2 (middle layer) after 120 seconds
/baa game 1 tnt_run floor removal 2 120

# Disable auto-removal for floor 3 (bottom layer stays)
/baa game 1 tnt_run floor removal 3 0
```

**Requirements:**
- Must be between 0-600 seconds
- 0 = disabled (floor never auto-removes)
- Players will receive countdown notifications before removal

**Strategy tips:**
- Remove upper floors first to force players down
- Keep the bottom floor permanent (0 seconds)
- Use notifications (10s, 30s, 60s before removal) to warn players

#### Configure Double Jump (Optional)

TNT Run includes an optional double jump feature that allows players to jump in mid-air.

**Set number of double jumps:**

```
/baa game [arena_id] tnt_run doublejump set [amount]
```

**Example:**
```
/baa game 1 tnt_run doublejump set 3
```

**Requirements:**
- Must be between 0-10
- 0 = double jump disabled (default)
- Recommended: 2-5 for balanced gameplay

**Set vertical boost (jump height):**

```
/baa game [arena_id] tnt_run doublejump setboost vertical [value]
```

**Example:**
```
/baa game 1 tnt_run doublejump setboost vertical 0.8
```

**Requirements:**
- Must be between 0.1-2.0
- Default: 0.8
- Lower = shorter jump
- Higher = higher jump

**Set horizontal boost (forward momentum):**

```
/baa game [arena_id] tnt_run doublejump setboost horizontal [value]
```

**Example:**
```
/baa game 1 tnt_run doublejump setboost horizontal 0.8
```

**Requirements:**
- Must be between 0.1-2.0
- Default: 0.8
- Lower = less forward movement
- Higher = more forward movement

**Double jump mechanics:**
- Players must press jump while in the air
- Visual/sound feedback when used
- Remaining jumps shown in action bar
- Jumps reset when respawning as spectator

**Map requirements:**
- Multi-level platform with 2-3 floor layers
- Each floor should be solid blocks (any material except air, barrier, bedrock)
- Space between floors for players to fall through
- Bedrock or barrier at the very bottom to prevent falling into void
- Open area with room to move around

**Configuration checklist:**
- [ ] Bounds set
- [ ] Spawns added
- [ ] Game duration set
- [ ] At least 1 floor layer added
- [ ] Floor delay configured
- [ ] (Optional) Auto-removal times set for floors
- [ ] (Optional) Double jump configured
- [ ] Enable minigame: `/baa game [id] tnt_run enable`

---

## Enabling the Minigame

After completing all required configuration, enable the minigame:

```
/baa game [arena_id] [minigame] enable
```

**Example:**
```
/baa game 1 spleef enable
```

If you're missing any required configuration, you'll get an error message telling you what's incomplete.

---

## Quick Reference by Minigame

| Minigame | Common Steps | Additional Requirements |
|----------|--------------|------------------------|
| Race | ✓ | Finish line |
| Spleef | ✓ | Floor (snow blocks) |
| Snowball Fight | ✓ | None |
| All Against All | ✓ | None |
| One In The Chamber | ✓ | Arrow removal setting |
| Traffic Light | ✓ | Finish line |
| Mine Field | ✓ | Finish line + Floor |
| Exploding Sheep | ✓ | Explosion level |
| TNT Tag | ✓ | None |
| Red Alert | ✓ | Floor (white wool) + Particles + Difficulty |
| Knock Back | ✓ | None |
| Fast Zone | ✓ | Finish line |
| Run From The Beast | ✓ | Beast spawn + Beast zone |
| Block Party | ✓ | Floor + Patterns (5+) + Timing |

---

## Troubleshooting

**"Cannot enable game. Complete its configuration first"**
- Check the configuration checklist for your specific minigame above
- Ensure all required steps are completed
- Use `/baa game [id] list` to see the minigame status

**"Add at least X spawns"**
- You need spawns equal to your arena's max players
- Use `/baa game [id] [minigame] spawn add` more times

**"Set the bounds before adding spawns"**
- Configure boundaries first using the Magic Stick
- Then add spawn points

**"Pattern does not exist"**
- Block Party requires at least 5 patterns
- Use `/baa game [id] block_party pattern list` to check

**"Both positions must be in the same world"**
- Your Magic Stick selections are in different worlds
- Use `/baa clearselection` and try again in the same world

**"Floor must be made of snow blocks" (Spleef)**
- Replace your floor with snow blocks
- Use `/fill` command or WorldEdit for large areas

**"Floor must be made of white wool" (Red Alert)**
- Replace your floor with white wool blocks
- The plugin will change the colors during gameplay

---

## Next Steps

After configuring your minigames:

1. **Enable your arena:**
   ```
   /baa enable [arena_id]
   ```

2. **Test the minigames** by joining the arena

3. **Configure more minigames** for variety (recommended: 5-8 for party mode)

4. **Set up signs** for players to join easily