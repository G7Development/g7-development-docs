# Configuration reference

Edit `config.lua`. Restart the resource after changing settings unless your server's workflow explicitly reloads them.

## Brand and detection

| Setting | Default | Purpose |
|---|---|---|
| `Config.Brand.Name` | `G7 Development` | Brand name used by the resource. |
| `Config.Brand.Product` | `G7 Lean System` | Product name shown in notifications. |
| `Config.Brand.Accent` | `#5b376a` | UI accent color. |
| `Config.Brand.Logo` | `assets/g7-logo.png` | UI logo path. |
| `Config.Debug` | `false` | Debug toggle; current code has limited use of this flag. |
| `Config.Framework` | `auto` | `auto`, `qb`, `qbox`, or `esx`. |
| `Config.Inventory` | `auto` | `auto`, `ox`, `qb`, `qs`, or `custom`. |

Automatic framework detection checks `qbx_core`, then `qb-core`, then `es_extended`. Automatic inventory detection checks `ox_inventory`, then `qs-inventory`, then `qb-inventory`. If multiple resources are running, set the inventory explicitly to the one players actually use.

## Security

| Setting | Default | Purpose |
|---|---:|---|
| `Config.Security.MaxDistance` | `3.0` | Configured maximum distance; review usage in the installed version before relying on it as a universal action limit. |
| `Config.Security.ActionCooldown` | `900` | Server action cooldown in milliseconds. |
| `Config.Security.PassDistance` | `2.5` | Maximum distance for passing a drink. |
| `Config.Security.MaxLinesPerDrink` | `4` | Configured maximum lines per drink; potency tiers currently use 1–4 lines. |
| `Config.Security.MaxBottleLines` | `16` | Maximum lines allowed in a measured bottle. |
| `Config.Security.MaxCupSips` | `20` | Starting sip count for a finished drink. |
| `Config.Security.LogRejectedActions` | `true` | Log rejected/invalid action attempts. |

The server applies a per-player cooldown, validates recipe identifiers and line/potency values, checks ingredients, and checks pass distance. Do not treat configuration values as a substitute for validating third-party inventory behavior.

## Progress durations

All values are milliseconds.

| Setting | Default |
|---|---:|
| `Config.Progress.Pour` | `4500` |
| `Config.Progress.Soda` | `3000` |
| `Config.Progress.Mix` | `5000` |
| `Config.Progress.Ice` | `2500` |
| `Config.Progress.Sip` | `2800` |

## Controls

| Setting | Default control | GTA control ID |
|---|---|---:|
| `Config.Controls.Sip` | E | `38` |
| `Config.Controls.Stow` | G | `47` |
| `Config.Controls.Pass` | C | `26` |
| `Config.Controls.Discard` | X | `73` |

The G/stow control currently displays a notification; it does not move the drink into another inventory location.

## Items, syrups, sodas, and potency

`Config.Items` contains the item IDs used for the empty bottle, measured bottle, empty cup, soda cup, ice, finished drink, and cup stack. Item IDs must match the names registered in the active inventory.

`Config.Syrups` defines five syrup profiles: `violet`, `royal`, `gold`, `crimson`, and `azure`. Each profile contains a display label, UI color, inventory item ID, screen effect, timecycle modifier, camera shake, and blackout multiplier.

`Config.Sodas` defines `citrus` and `orange`, each with a label, item ID, and UI color.

`Config.Potency` defines `light`, `medium`, `strong`, and `heavy`. Defaults consume 1, 2, 3, and 4 lines respectively and provide an effect intensity value. If you add or rename recipes, also review item registration and the UI assumptions in `web/app.js`.

`Config.Text` contains user-facing messages for common missing-item and invalid-action cases.
