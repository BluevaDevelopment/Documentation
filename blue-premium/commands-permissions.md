# Commands & Permissions

Complete command and permission reference for Blue Premium.

The proxy commands are available on BungeeCord and Velocity. The backend command
is only available on Spigot, Paper or compatible backend servers where the
backend JAR is installed.

**Notation:**

- `[required]` - Required argument
- `(optional)` - Optional argument
- `<a|b>` - Choose one value
- Commands shown with `/bluepremium` can also be executed through the root aliases configured on the proxy.

## Root Commands

Main command: `/bluepremium`

Default root aliases: `/bp`, `/jpremium`, `/jbungee`

| Command | Permission | Description |
|---------|------------|-------------|
| `/bluepremium` | `bluepremium.admin` or legacy admin permission | Shows Blue Premium information. |
| `/bluepremium help` | `bluepremium.admin` or legacy admin permission | Shows the command help page. |
| `/bluepremium reload` | `bluepremium.admin`, `jpremium.command.reload` or `jpremium.command.*` | Reloads configuration and message files. |
| `/bluepremium version` | `bluepremium.admin` or legacy admin permission | Shows the installed plugin version. |
| `/bluepremium migrate` | `bluepremium.admin` or legacy admin permission | Imports compatible JPremium data and settings. |
| `/bluepremium antibot` | `bluepremium.admin` or legacy admin permission | Shows anti-bot guard status. |
| `/bluepremium antibot reset` | `bluepremium.admin` or legacy admin permission | Resets anti-bot guard counters. |
| `/bluepremium stats` | `bluepremium.admin` or legacy admin permission | Shows authentication and storage statistics. |

## Admin Inspection Commands

These commands are meant for support, moderation and migration verification.

| Command | Permission | Description |
|---------|------------|-------------|
| `/bluepremium check [nickname]` | `bluepremium.admin` or legacy admin permission | Shows profile, account mode and Mojang status for a player. |
| `/bluepremium audit` | `bluepremium.admin` or legacy admin permission | Shows recent security audit events. |
| `/bluepremium audit [count]` | `bluepremium.admin` or legacy admin permission | Shows a specific number of recent audit events. |
| `/bluepremium audit player [name]` | `bluepremium.admin` or legacy admin permission | Shows audit events for one player. |
| `/bluepremium history [player]` | `bluepremium.admin` or legacy admin permission | Shows a player's IP login history. |
| `/bluepremium history ip [address]` | `bluepremium.admin` or legacy admin permission | Shows all accounts that used an IP address. |
| `/bluepremium ban ip [address] (reason)` | `bluepremium.admin` or legacy admin permission | Bans an IP address from authentication. |
| `/bluepremium ban nick [nickname] (reason)` | `bluepremium.admin` or legacy admin permission | Bans a nickname from authentication. |
| `/bluepremium ban unban ip [address]` | `bluepremium.admin` or legacy admin permission | Removes an IP ban. |
| `/bluepremium ban unban nick [nickname]` | `bluepremium.admin` or legacy admin permission | Removes a nickname ban. |
| `/bluepremium ban list` | `bluepremium.admin` or legacy admin permission | Lists active IP and nickname bans. |

## Player Authentication Commands

Player commands do not require permission nodes by default. They are controlled
by the authentication state, feature toggles and server restrictions configured
in `configuration.yml`.

| Command | Aliases | Description |
|---------|---------|-------------|
| `/login [password]` | `/l` | Logs in as a registered cracked player. |
| `/register [password] (confirm)` | none | Registers a new cracked account. The confirm argument depends on the password confirmation setting. |
| `/logout` | none | Logs out and clears the current authenticated state. |
| `/changepassword [old] [new] (confirm)` | none | Changes the current password. The confirm argument depends on the password confirmation setting. |
| `/createpassword [password] (confirm)` | none | Creates the first password for accounts that do not have one yet. |
| `/unregister [password]` | none | Deletes the player's own account profile. |
| `/premiumlogin` | none | Manually retries premium login for the current player. |
| `/premium [password]` | none | Switches a cracked profile to premium mode. |
| `/cracked [password]` | none | Switches a premium profile back to cracked mode. |
| `/confirm` | none | Confirms pending authentication actions when required by the current flow. |
| `/startsession` | none | Starts a manual session so the player can skip login temporarily. |
| `/stopsession` | `/destroysession` | Stops the active manual session. |

## Recovery And Email Commands

These commands require the mail section to be configured when recovery emails
need to be sent.

| Command | Aliases | Description |
|---------|---------|-------------|
| `/changemail [password] [email]` | `/changeemailaddress` | Sets or changes the recovery email address. |
| `/requestpasswordrecovery [email]` | `/recoverypassword` | Sends a password recovery request to the stored email address. |
| `/confirmpasswordrecovery [code] [new] (confirm)` | none | Applies a pending password recovery code. The confirm argument depends on the password confirmation setting. |

## Two-Factor Commands

Blue Premium supports TOTP two-factor authentication and recovery codes.

| Command | Aliases | Description |
|---------|---------|-------------|
| `/totp enable` | `/requestsecondfactor` | Starts TOTP setup and shows the secret/QR data. |
| `/totp confirm [code]` | `/activatesecondfactor [code]` | Confirms and enables TOTP. |
| `/totp verify [code]` | none | Verifies the second factor during login. |
| `/totp disable [code]` | `/deactivatesecondfactor [code]` | Disables TOTP for the current profile. |
| `/totp recoverycodes` | none | Shows or regenerates recovery codes, depending on the current state. |
| `/totp recover [code]` | none | Uses a recovery code when the authenticator app is not available. |

Some TOTP operations can also accept the player's password before the code when
the current configuration requires password confirmation.

## Admin Force Commands

These commands are intentionally registered as root-level commands because they
mirror the JPremium administration workflow. They require `bluepremium.admin` or
an accepted legacy JPremium admin permission.

| Command | Aliases | Description |
|---------|---------|-------------|
| `/forcelogin [player]` | none | Marks a player as logged in. |
| `/forceregister [player] [password]` | none | Registers a player account. |
| `/forceunregister [player]` | none | Removes a player account. |
| `/forcechangepassword [player] [password]` | none | Changes a player's password. |
| `/forcecreatepassword [player] [password]` | none | Creates a missing password for a player. |
| `/forcechangemail [player] [email]` | `/forcechangeemailaddress` | Changes a player's recovery email. |
| `/forcepremium [player]` | none | Forces premium account mode. |
| `/forcecracked [player]` | none | Forces cracked account mode. |
| `/forcerequestpasswordrecovery [player]` | `/forcerecoverypassword` | Creates a password recovery request for a player. |
| `/forceconfirmpasswordrecovery [player] [password]` | none | Applies a pending recovery for a player. |
| `/forcestartsession [player]` | none | Starts a manual session for a player. |
| `/forcedestroysession [player]` | none | Destroys a player's active session. |
| `/forcerequestsecondfactor [player]` | none | Creates a TOTP setup request for a player. |
| `/forceactivatesecondfactor [player]` | none | Activates TOTP after a pending setup request. |
| `/forcedeactivatesecondfactor [player]` | none | Disables TOTP for a player. |
| `/forcepurgeuserprofile [player]` | `/forcepurge` | Purges a profile and related authentication data. |
| `/forceviewuserprofile [player]` | `/forceview` | Views one profile. |
| `/forceviewuserprofiles [address]` | none | Views profiles associated with an IP address. |
| `/forcemergepremiumuserprofile [current] [previous]` | none | Merges premium/cracked profile data after a name or UUID transition. |

## Backend Commands

Main backend command: `/bluepremiumbackend`

Default alias: `/bpb`

Backend commands require `bluepremium.admin`.

| Command | Description |
|---------|-------------|
| `/bluepremiumbackend help` | Shows backend command help. |
| `/bluepremiumbackend reload` | Reloads backend configuration and messages. |
| `/bluepremiumbackend version` | Shows backend plugin version. |
| `/bluepremiumbackend setspawn` | Saves the current location as the authentication spawn. |
| `/bluepremiumbackend status` | Shows backend connection and protection status. |
| `/bluepremiumbackend migrate` | Imports local JPremium backend settings. |

## Permission Nodes

Blue Premium uses one modern administrator permission and accepts selected
JPremium nodes to make migrations easier.

| Permission | Applies to | Description |
|------------|------------|-------------|
| `bluepremium.admin` | Proxy and backend | Grants access to all Blue Premium administrative commands and update notifications. |
| `jpremium.admin` | Proxy | Accepted for JPremium-compatible proxy administration during migration. |
| `jpremium.command.*` | Proxy | Accepted for JPremium-compatible proxy administration during migration. |
| `jpremium.command.reload` | Proxy | Accepted by `/bluepremium reload`. |
| `jpremium.command.<LegacyCommand>` | Proxy force commands | Accepted by matching `/force*` commands for old permission setups. |
| `jpremium.command.forceRecoveryPassword` | Proxy | Accepted as a compatibility alias for `/forcerequestpasswordrecovery`. |

For new networks, grant `bluepremium.admin` to staff groups that should manage
authentication. Keep the legacy `jpremium.*` permissions only while replacing
old JPremium permission groups.

## Configurable Aliases

Most proxy command aliases can be adjusted in the proxy `configuration.yml`
under `command_aliases`. Use this when a network already has another plugin
claiming a common command such as `/premium`, `/login` or `/register`.

After changing aliases, run:

```text
/bluepremium reload
```

If the proxy platform does not fully refresh command registrations at runtime,
restart the proxy after the reload.
