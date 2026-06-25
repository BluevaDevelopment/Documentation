# Backup Management

Blue Worlds includes automatic and manual backup systems for both private and global worlds.

## Configuration

Backup settings are in `plugins/BlueWorlds/settings.yml` under `backups:`.

```yaml
backups:
  enabled: true
  backup_interval: 2d
  max_backups: 10
  delete_old_backups: true
  compression: true
  backup_on_shutdown: true
  skip_if_no_changes: true
  global_worlds:
    enabled: true
    worlds: [world, world_nether, world_the_end]
  private_worlds:
    enabled: true
```

| Setting | Description |
|---------|-------------|
| `enabled` | Enable automatic backups |
| `backup_interval` | Time between backups |
| `max_backups` | Maximum backups per world |
| `delete_old_backups` | Remove oldest backups when limit is reached |
| `compression` | Compress backups to save disk space |
| `backup_on_shutdown` | Create backup when server stops |
| `skip_if_no_changes` | Skip backup if world has not changed |

## Private World Backups

### Create a Backup

```
/pw backup create [slot]
```

**Example:**
```
/pw backup create 1
```

### List Backups

```
/pw backup list [slot]
```

### Restore from Backup

```
/pw backup restore [slot] <backup_name>
```

**Example:**
```
/pw backup restore 1 backup_2025-01-15_14-30-00
```

### Delete a Backup

```
/pw backup delete [slot] <backup_name>
```

### View Backup Info

```
/pw backup info [slot] <backup_name>
```

## Global World Backups

### Create a Backup

```
/gw backup <world> create
```

**Example:**
```
/gw backup world create
```

### List Backups

```
/gw backup <world> list
```

### Restore from Backup

```
/gw backup <world> restore <backup_name>
```

**Example:**
```
/gw backup world restore world_2025-01-15_14-30-00
```

### Delete a Backup

```
/gw backup <world> delete <backup_name>
```

## Admin Backups

Administrators can manage backups of any private world:

```
/pw admin backup <world_id> <action>
```

**Examples:**
```
/pw admin backup bw_pvt_123 create
/pw admin backup bw_pvt_123 list
```

## Backup Storage

Backups are stored in `plugins/BlueWorlds/backups/` by default. The exact path can be configured in `settings.yml`.

## Best Practices

1. **Back up before major changes** such as resets or flag updates.
2. **Set a reasonable retention policy** to balance safety and disk usage.
3. **Enable compression** to save disk space.
4. **Test restores** occasionally to ensure backups work.
5. **Back up templates** separately if they are complex or custom-built.

## Troubleshooting

**"Backup failed"**
- Check available disk space.
- Verify the world is loaded.
- Check console for errors.

**"Backup not found"**
- Use `/pw backup list [slot]` or `/gw backup <world> list` to see available backups.
- Check spelling and case sensitivity.

**"Restore failed"**
- Make sure no players are in the world.
- Verify the backup file is not corrupted.
- Check console for errors.
