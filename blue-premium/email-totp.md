# Email Recovery & TOTP

Blue Premium includes password recovery by email and TOTP two-factor
authentication.

## Email Recovery

Configure SMTP on the proxy:

```yaml
mail:
  host: 'smtp.example.com'
  port: 587
  user: 'no-reply@example.com'
  password: 'smtp-password'
  name: 'Your Server'
  use_tls: true
  recovery_subject: 'Password Recovery'
  recovery_delay_minutes: 60
```

| Key | Meaning |
|-----|---------|
| `host` | SMTP server hostname |
| `port` | SMTP port, usually `587` with TLS |
| `user` | SMTP username |
| `password` | SMTP password |
| `name` | Sender display name |
| `use_tls` | Enables STARTTLS/TLS |
| `recovery_subject` | Email subject |
| `recovery_delay_minutes` | Cooldown between recovery requests |

## Player Recovery Flow

Players should first set an email:

```text
/changemail <password> <email>
```

Legacy alias:

```text
/changeemailaddress <password> <email>
```

Then they can request recovery:

```text
/requestpasswordrecovery <email>
/recoverypassword <email>
```

And confirm it:

```text
/confirmpasswordrecovery <token> <newPassword>
```

If password confirmation is enabled:

```text
/confirmpasswordrecovery <token> <newPassword> <newPassword>
```

## Recovery Template

If a migrated JPremium folder contains `recovery.html`, Blue Premium copies it
as:

```text
plugins/BluePremium/recovery-template.legacy.html
```

Review the template before using it. Keep placeholders such as `%nickname%`,
`%password%` or the migrated recovery token placeholder if your old template
used them.

## TOTP Setup

Configure the display name:

```yaml
totp:
  server_name: 'My Server'
```

Player commands:

```text
/totp enable
/totp confirm <code>
/totp verify <code>
/totp disable <code>
/totp recoverycodes
/totp recover <code>
```

JPremium-compatible commands:

```text
/requestsecondfactor
/activatesecondfactor <code>
/deactivatesecondfactor <code>
```

Some flows can require the password before the code when password confirmation
is enabled or when using legacy command behavior.

## Login With TOTP

Players can complete 2FA in one command:

```text
/login <password> <code>
```

or after password login:

```text
/totp verify <code>
```

## Admin TOTP Commands

```text
/forcerequestsecondfactor <player>
/forceactivatesecondfactor <player>
/forcedeactivatesecondfactor <player>
```

These require `bluepremium.admin` or accepted legacy JPremium admin permissions.

## JPremium TOTP Migration

Blue Premium can migrate active JPremium TOTP secrets stored in legacy profile
fields into its own TOTP table. Existing migrated users should be tested with:

```text
/login <oldPassword> <currentAuthenticatorCode>
```

before production cutover.

## Troubleshooting

**No recovery email arrives**

- Check SMTP host, port and credentials.
- Confirm `use_tls` matches your provider.
- Check proxy console logs for mail errors.
- Confirm the player has a stored recovery email.

**TOTP code is rejected**

- Confirm the phone clock is synchronized.
- Try a fresh code.
- Use a recovery code if available.
- Staff can use admin TOTP commands after verifying account ownership.
