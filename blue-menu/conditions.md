# Conditions

Use display conditions to show or hide menu elements based on player data, placeholders, or logic. Java items, Bedrock buttons, and Bedrock components all share the same evaluator.

## Syntax options
- **Simple list:** every condition string must be true.
- **Grouped map:** use `all`, `any`, and `none` for more complex logic.

## Operators
- **Boolean:** `&&`, `||`, `!`.
- **Comparison:** `>`, `>=`, `<`, `<=`, `==`, `!=` (numeric or string-aware).
- **String:** `contains`, `startsWith`, `endsWith`, `equals`, `equalsIgnoreCase`, `matches`.
- **Math:** expressions inside a condition are evaluated before comparison (supports `+`, `-`, `*`, `/`, `%`).

Placeholders are resolved before evaluation, so PAPI or built-in placeholders can be used directly.

## Examples
```yaml
# Simple list: AND logic
- "{player_level} >= 10"
- "{player_health} > 5"

# Grouped logic
all:
  - "{player_level} >= 20"
any:
  - "{player_health} >= 15"
  - "{vault_eco_balance} >= 1000"
none:
  - "{player_name} equalsIgnoreCase 'griefer'"
```