# API & Events

Blue Premium exposes a native API and compatibility bridges for plugins that
were originally compiled against JPremium.

## Native API

Main package:

```text
net.blueva.premium.api
```

Core types:

| Type | Purpose |
|------|---------|
| `BluePremiumApi` | Profile lookups, force operations and resolver access |
| `UserProfile` | User account data |
| `UserState` | Current authentication/account state |
| `Resolver` | Custom premium-profile resolver contract |
| `CustomResolverProvider` | Registers a custom resolver |

Profile lookup methods include:

```java
getUserProfiles()
getUserProfileByUniqueId(UUID uniqueId)
getUserProfileByPremiumId(UUID premiumId)
getUserProfileByNickname(String nickname)
fetchUserProfileByUniqueId(UUID uniqueId)
fetchUserProfileByPremiumId(UUID premiumId)
fetchUserProfileByNickname(String nickname)
```

`get*` methods use the currently available state. `fetch*` methods may query the
storage layer depending on the platform implementation.

## Administrative API Operations

Blue Premium exposes the same practical force-operation surface used by the
admin commands:

```java
forceLogin(profile)
forceRegister(profile, password)
forceUnregister(profile)
forceChangePassword(profile, password)
forceCreatePassword(profile, password)
forcePremium(profile)
forceCracked(profile)
forceStartSession(profile)
forceDestroySession(profile)
forceChangeEmailAddress(profile, email)
forcePurgeUserProfile(profile)
forceRecoveryPassword(profile)
```

These operations should be used from trusted admin/internal plugins only. They
can modify stored authentication state.

## Events

Native event package:

```text
net.blueva.premium.api.event
```

Available user events include:

| Event | Fired when |
|-------|------------|
| `UserLoginEvent` | A user logs in |
| `UserLogoutEvent` | A user logs out |
| `UserFailedLoginEvent` | A login attempt fails |
| `UserRegisterEvent` | A profile is registered |
| `UserUnregisterEvent` | A profile is removed |
| `UserCreatePasswordEvent` | A password is created |
| `UserChangePasswordEvent` | A password changes |
| `UserChangeMailEvent` | Recovery email changes |
| `UserPasswordRecoveryEvent` | Password recovery is requested or applied |
| `UserRequestSecondFactorEvent` | TOTP setup is requested |
| `UserEnableTotpEvent` | TOTP is enabled |
| `UserDisableTotpEvent` | TOTP is disabled |
| `UserCreateSessionEvent` | A session is created |
| `UserDestroySessionEvent` | A session is destroyed |
| `UserPremiumDetectedEvent` | Premium status is detected |
| `UserCrackedEvent` | A profile switches to cracked mode |
| `UserAutoUnregisterEvent` | Inactive account cleanup removes a profile |
| `UserNicknameChangedEvent` | A premium account changed its Mojang nickname |

Platform adapters republish these through the relevant proxy event system.

## JPremium Compatibility Packages

Blue Premium includes common legacy packages so existing integrations can keep
linking after the JPremium JAR is removed.

Bungee-style documented package:

```text
com.jakub.premium.api
com.jakub.premium.api.event
com.jakub.premium.JPremium
```

JPremium source-style proxy package:

```text
com.jakub.jpremium.proxy.api
com.jakub.jpremium.proxy.api.user
com.jakub.jpremium.proxy.api.resolver
```

Velocity documented package:

```text
com.jakub.jpremium.velocity.api
com.jakub.jpremium.velocity.api.event
com.jakub.jpremium.velocity.JPremiumVelocity
```

Velocity source-style event package:

```text
com.jakub.jpremium.proxy.api.event.velocity
```

The compatibility bridge is intended for existing plugins. New plugins should
prefer the native `net.blueva.premium.api` package.

## Custom Premium Resolver

Use a custom resolver when your network must override Mojang lookups with an
external trusted source.

Native package:

```java
CustomResolverProvider.setResolver(nickname -> {
    // Return a Profile when the account is premium, or null/empty when cracked.
});
```

Resolvers must be thread-safe. They can run during login flow and should avoid
slow blocking calls unless they have their own timeout and cache.

## Integration Notes

- Do not call force operations from async contexts unless your own plugin is
  designed for it.
- Prefer UUID lookups when possible.
- Treat email addresses, IP addresses and recovery state as private data.
- Test integrations on both BungeeCord and Velocity if your plugin supports both.
