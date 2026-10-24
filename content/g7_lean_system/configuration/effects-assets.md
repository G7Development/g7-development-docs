# Effects, props, and animations

## Effects

`Config.Effects` controls client-side roleplay effects.

| Setting | Default | Description |
|---|---:|---|
| `Enabled` | `true` | Master switch for drink effects. |
| `Progressive` | `true` | Scales intensity using the sip count. |
| `Blackout` | `true` | Enables chance-based blackout behavior after the configured sip threshold. |
| `BlackoutStartSip` | `8` | Sip count at which blackout checks begin. |
| `BaseBlackoutChance` | `4` | Base chance percentage. |
| `MaxBlackoutChance` | `25` | Maximum chance percentage. |
| `BlackoutDuration` | `9000` | Ragdoll/blackout duration in milliseconds. |
| `RecoveryDuration` | `4000` | Recovery wait in milliseconds. |
| `ArmorEnabled` | `false` | Enables optional armor changes. |
| `ArmorByPotency` | all `0` | Armor amount by potency tier. |
| `Movement` | `true` | Applies a temporary movement-rate change. |
| `Ragdoll` | `true` | Enables ragdoll during blackout behavior. |
| `EffectDuration` | `45000` | Duration before movement/screen effects are reset. |

Effects are client-side roleplay presentation, not authoritative gameplay security. Tune them to match your server's rules and accessibility expectations.

## Props

`Config.Assets` contains `CupModel`, `BottleModel`, `SodaBottleModel`, `EmptyBottleModel`, `IceModel`, and `SyrupModels`. Defaults use base-game models so the resource does not require a custom prop pack.

If you use custom props, install assets you have permission to use and ensure the configured model names exactly match the streamed model names. Test each model in-game.

## Animations

`Config.Assets.Animations` contains:

- `PourDict` / `PourAnim`
- `DrinkDict` / `DrinkAnim`
- `MixDict` / `MixAnim`

The supplied defaults are base-game fallback animation dictionaries/clips. Custom animation assets are not included in this archive. Only add licensed assets and set dictionary/clip names that actually exist in those assets.

## Cleanup

On resource stop, the client clears NUI focus, timecycle modifiers, screen effects, camera shake, and movement-rate override. Test effect cleanup if you change the client code or introduce additional effects.
