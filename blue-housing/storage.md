# Storage

Where the plugin keeps its structured data: houses (flags, roles, whitelist, bans, decorations, Discovery counters), players and templates.

**Not the worlds.** A house's world is region files the server reads and writes directly while it is loaded, so they stay on disk whatever the backend.

```yaml
storage:
  backend: json          # json | sqlite | mysql
  table_prefix: bh_
```

| Backend | What it is | Use it when |
|---------|------------|-------------|
| `json` | One file per house, player and template | One server, no reason to change |
| `sqlite` | A single file in the data folder | One server, and you want Discovery to be a query rather than a scan of every house |
| `mysql` | A database server (`mariadb` is accepted as a name) | A network, or you keep your data in one place |

```yaml
  sqlite:
    file: housing.db
  mysql:
    host: 127.0.0.1
    port: 3306
    database: bluehousing
    user: root
    password: ""
    properties: ""
```

The JDBC driver is downloaded on first start, and only the one the backend names.

> `properties` is appended to the JDBC url verbatim. Anything there must match the driver actually in use: MariaDB's, unless another plugin brought Oracle's first (`sslMode` vs `useSSL`).

## Switching

```
/ha storage status
/ha storage migrate confirm
```

Migration copies the JSON documents into the database and **leaves the files where they are**, so putting `backend` back is all it takes to go back.

> `confirm` is required, and deliberately absent from the tab completion. Migration is a copy **from the files**, and the files stop being updated the moment a SQL backend is switched on, so a second run months later is every house on the server reverting to the day of the switch. The number of rows at risk is printed first.

## Reindexing

```
/ha storage reindex
```

A SQL row is the document plus indexed columns derived from it **on save**. Adding a column gives existing rows the type's default with nothing to backfill it, so on an upgraded install `description`, `icon`, `featured` and the rest read empty until somebody edits each house.

`reindex` recomputes every row's columns **without rewriting the document**, so nothing is read from disk and nothing is replaced by anything older. It is safe to run at any time, which is exactly what `migrate` is not.

## Why a column can be wrong

A column beside a document is a re-implementation of a reader, and a reader has defaults. `listed` is derived from `whitelist.enabled`, which `HouseWhitelistManager` reads as **enabled when absent**: a house that has never had `/h whitelist` run on it is private. A projection that read the same missing key as `false` published every untouched house in Discovery the moment somebody set a backend.

If a house looks wrong after switching backends, `/ha storage reindex` is the first thing to try.
