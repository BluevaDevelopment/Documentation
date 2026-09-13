# Music

Note Block Studio playback inside a house.

## Commands

| Command | Description |
|---------|-------------|
| `/h music` | Open the music menu |
| `/h music list` | The song library |
| `/h music info` | What this house is playing |
| `/h music play <id>` | Set the house song |
| `/h music stop` | Stop it |
| `/h music volume <0-100>` | |
| `/h music speed <n>` | |
| `/h music loop <true\|false>` | |
| `/h music autoplay <true\|false>` | Play on entry |
| `/h music mute` | Silence it **for yourself**, this session |
| `/ha music import <url> [overwrite]` | Import a pack |
| `/ha music reload` | Re-scan `data/songs/` |
| `/ha music list` | |

## The Library

Songs live in `data/songs/` as `.nbs` or `.mid` and are **shared by every house**: a file is never copied per house.

**No songs ship in the JAR.** On the first start a starter pack is downloaded from `music.auto_download.url` (the `BluevaDevelopment/NBSsongs` repository by default) and `data/songs/.pack-installed` records that it happened. Delete that file to fetch again, or point the URL at a pack of your own.

Keeping the songs in a separate repository is deliberate: it can be taken down or replaced without touching a released plugin.

## Playback

Playback is **per listener** and lives only in memory, so it stops on leaving, on quitting and when the house unloads. Looping is polled once a second; a script's one-off `player:playSong(id)` is excluded from that poll.

## The Song Shop

Songs are unlocked per player, like decorative heads.

```yaml
music:
  shop:
    free: [pigstep, cat]
    default_price: 500
    prices:
      my_song: 1200
```

| | |
|---|---|
| Free songs | `music.shop.free` |
| Everything else | `music.shop.default_price`, or a per-song override |
| Skip the shop | `bluehousing.music.songs`, or `bluehousing.music.song.<id>` |

Ownership lives in `data/players/<uuid>.json` under `music.owned.<id>`. Buying grants the **right to choose** a song; the library is shared, so nothing is copied.

The gate is enforced in the Java menu, the Bedrock form and `/h music play` alike; leaving any one open would make paying optional. There is deliberately **no operator bypass**: owning a song is a purchase, not authority, and the admin testing the shop is the one person who has to see it working. With no economy configured the gate opens rather than locking every priced song away for ever.

### In the menu

A song you own is drawn with a disc colour picked from a hash of its id, with a hidden enchantment for the glint. One you have not bought is the **same black disc** for all of them, so the colour in the list is the part of the library that is actually yours.

**My Songs** (`/h music list` → library) shows what this player may already choose. Ownership is asked of the shop rather than read from a stored list, so the menu cannot disagree with what clicking a song does.
