# Network Mode

One BlueHousing, or several sharing the same houses. The rule everything follows from: **a house is loaded on exactly one server at a time, and the database says which.**

```yaml
network:
  mode: standalone      # standalone | lobby | house
```

| Mode | What the server is |
|------|--------------------|
| `standalone` | One server, its own houses on its own disk |
| `lobby` | Shows the houses and owns none. Discovery, `/h goto` and the menus work; entering a house sends the player to a house server |
| `house` | Loads and runs houses; players arrive already routed here |

`bungee` and `network` are accepted as names for `lobby`.

**A networked mode needs `storage.backend` to be a database.** Without shared state the servers would disagree about who owns what, so a mode configured but impossible is refused and logged, and the server runs standalone rather than half-networked.

## Setting it up

```yaml
network:
  mode: house
  server:
    name: house-1          # must match the proxy's own name
    address: ""            # only for paper_transfer
    max_players: 0
  house_servers: []        # on a lobby: the servers it may route to
  heartbeat_seconds: 5
  server_timeout_seconds: 30
  lock_timeout_seconds: 120
  transfer: bungee         # bungee | paper_transfer
```

| Transfer | How it moves a player |
|----------|----------------------|
| `bungee` | A plugin message the proxy reads. It carries a server **name**, so `server.name` has to match the proxy's config or nobody goes anywhere |
| `paper_transfer` | Paper's transfer packet (1.20.5+). No proxy needed, but each house server must set `server.address` to something the client can reach |

```
/ha network status
```

## World Stores

On a network the houses have to live somewhere both servers can reach.

```yaml
network:
  worlds:
    store: none          # none | local | sftp | s3
    cache: true
```

| Store | Where the world lives |
|-------|----------------------|
| `none` | `data/houses/<owner>/<id>/world/`. Standalone |
| `local` | A folder on this machine |
| `sftp` | One archive per house on an SFTP server |
| `s3` | The same, in an S3 bucket (`minio` works) |

A world is copied in to load it and copied back to unload it, twice per session and never while somebody is inside, which is what makes it possible to keep it elsewhere at all.

An upload goes under a temporary name, is checked, and is renamed into place, so a transfer that dies leaves the previous world intact rather than half of the new one.

### `cache`

Keeps the local copy after unloading, so a house coming back to this server skips the download.

Turn it off only for a house server with more houses than disk. From then on **the store is the only copy**, and every load is a download.

## How the lock works

`HouseLocks` is one row per loaded house, with the house as the primary key, so "two servers loaded it" is not a race that can be lost but a row that cannot exist twice.

The ordering is asymmetric on purpose:

```
take the lock  →  fetch the world
upload  →  verify  →  release the lock
```

A failed upload therefore **keeps** the lock, which is correct: nobody else may run a house whose newest state is still here. A house whose publish failed is retried on the heartbeat, and one whose folder has gone is given up rather than held for ever.

## Standalone servers

None of this applies with `mode: standalone`, which is the default. A single server needs no database, no store and no proxy.
