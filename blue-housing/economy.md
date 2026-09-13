# Economy

Money for building, and things to spend it on. All of it is optional: with `economy.enabled: false`, and none of the rest exists.

## Providers

```yaml
economy:
  provider: internal      # internal | vault | custom
```

| Provider | Balances live in |
|----------|------------------|
| `internal` | `data/players/<uuid>.json` |
| `vault` | Whatever economy plugin Vault is bound to |
| `custom` | Somebody else's, driven by commands and a placeholder |

A custom provider needs three things:

```yaml
economy:
  custom:
    give_command: "eco give %player% %amount%"
    take_command: "eco take %player% %amount%"
    get_placeholder: "%vault_eco_balance%"
```

Currency name, symbol, symbol position and decimals are under `economy.currency`.

## Rewards

**Rewards are the only way money enters the internal economy.** Both are on by default.

### Building by hand

```yaml
economy:
  rewards:
    blocks:
      enabled: true
      amount: 0.05
      probability: 0.73
      daily_limit: 500.0
```

Each block placed in a house pays `amount`, `probability` of the time. Earning is passive, something that happens by building rather than by running a command, so `bluehousing.house.economy.earn` is granted by default like the other things a player just does.

### WorldEdit

```yaml
      worldedit:
        enabled: true
        amount: 0.005
        probability: 0.42
        max_blocks_per_operation: 10000
```

A `//sphere` is thousands of blocks in one keystroke, so it is paid at **its own, lower rate**, a tenth of the hand rate by default. Otherwise building by hand is never worth it. The action bar names the block count.

Requires WorldEdit or FastAsyncWorldEdit. On FAWE the plugin adds its counting extent to FAWE's own `allowed-plugins` list at startup; the console says which provider the hook registered on.

### Playtime

```yaml
    playtime:
      enabled: true
      interval_ticks: 1200      # once a minute
      amount: 1.0
      daily_limit: 250.0
```

Paid for being in a house.

### Why the numbers are unpredictable

A reward that pays every time is a number anybody can work out, and a number anybody can work out is worth farming. The probability is worth changing now and then.

A lost roll still **spends the position**, so putting the same block back does not re-roll it.

### Anti-abuse

A block position that has just paid does not pay again for `economy.rewards.blocks.anti_abuse.cache_seconds` (3 hours). This is a memory cache with a lifetime, not a record kept for ever: a creative house is millions of blocks. It is swept from the playtime timer and dropped when a house is reset, because after a reset those blocks are gone and putting them back is building.

A WorldEdit operation is claimed as one key for the box it filled, so repeating the same `//set` on the same selection does not pay again.

### Daily limits

`blocks.daily_limit` (500, hand and WorldEdit together) and `playtime.daily_limit` (250) are hard ceilings per player; `0` means none. The last payout is **trimmed** to land exactly on the limit rather than dropped.

They are stored in `data/players/<uuid>.json` under `economy.daily.<source>` with the date they belong to, so a record from another day reads as zero and nothing has to run at midnight. Running out is said once a day, not once a block.

### Feedback

A run of payouts is numbered: `{combo}` and `{total}` both move on every payout, and a run ends after `economy.rewards.combo_seconds` (3) without one, or when the reward changes kind, since a `//set` after a handful of hand-placed blocks is a different thing being reported.

## Purchases

### Extra house slots

```yaml
    extra_house_slots:
      base_slots: 1
      max_purchasable: 5
      price: 1000.0
      price_increase: 500.0
```

Each slot costs `price + (price_increase × slots already bought)`.

### A bigger border

```yaml
    world_border:
      step: 64
      max_size: 1024
      price_per_step: 500.0
```

```
/h economy buy slot
/h economy buy border
```

## Player to Player

| Command | Description |
|---------|-------------|
| `/h economy balance [player]` | Check a balance |
| `/h economy pay <player> <amount>` | Pay somebody, picked from a list; only the amount is typed |
| `/h economy donate <amount>` | Donate to the house you are visiting |
| `/h economy sale offer <player> <amount>` | Offer this house |
| `/h economy sale accept <player>` | Accept an offer |
| `/h economy sale cancel` | |

Offers expire after `economy.sales.offer_timeout_seconds` (120).

Donate is deliberately absent from the Economy **menu**: that menu opens from the house menu, which only the owner reaches, so it was an offer to pay yourself. The command stays for the visitor it was written for.

## Admin

```
/ha economy balance <player>
/ha economy set <player> <amount>
/ha economy add <player> <amount>
/ha economy remove <player> <amount>
```

Permission: `bluehousing.admin.economy`.

## Placeholders

| Placeholder | Description |
|-------------|-------------|
| `%bluehousing_balance%` | Formatted with the currency |
| `%bluehousing_balance_raw%` | The number |
| `%bluehousing_rewards_left_building%` | What is left of today's building limit |
| `%bluehousing_rewards_left_playtime%` | Same, for playtime |
| `%bluehousing_rewards_earned_building%` | Earned today |
| `%bluehousing_rewards_earned_playtime%` | Earned today |

`%bluehousing_rewards_left_*%` shows `economy.rewards.no_limit` (∞) when that source has no limit, so a scoreboard can show it instead of the player finding out when the money quietly stops.
