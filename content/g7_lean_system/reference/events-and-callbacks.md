# Events and callbacks

This page documents the event and callback names found in the `g7_leansystem` v1.0.0 source. Events marked internal are implementation details, not a stable third-party API. Do not trigger server action events from untrusted client code; the server validates several fields, but these are still internal resource interfaces.

## Server event

### `g7_lean:server:action` — internal

Client sends an action string and a payload table. Supported action values in the server handler:

| Action | Payload fields used |
|---|---|
| `pourSyrup` | `syrup` (key from `Config.Syrups`), `lines` (integer) |
| `pourSoda` | `soda` (key from `Config.Sodas`) |
| `mix` | `potency` (key from `Config.Potency`) |
| `addIce` | no client recipe fields; server reads drink and ice inventory state |
| `sip` | no client recipe fields; server reads drink inventory state |
| `pass` | `target` (server ID) |
| `discard` | no client recipe fields; server reads drink inventory state |
| `unpackCups` | no additional fields; handler behavior depends on current implementation |

The server enforces a per-player action cooldown and validates recipe identifiers and ingredients. The event is not intended as a public API contract and may change between versions.

## Client events

| Event | Parameters | Purpose |
|---|---|---|
| `g7_lean:client:open` | `item`, `data` | Opens the NUI for a usable item; shows active-drink controls when the item is a finished drink. |
| `g7_lean:client:empty` | none | Clears active-drink state and notifies the player the drink is finished. |
| `g7_lean:client:drinkState` | `active` (boolean) | Toggles active-drink controls. |
| `g7_lean:client:effects` | `syrupKey`, `potency`, `sipCount`, `iced` | Applies configured client-side roleplay effects after a sip. |

These events are registered by the client resource. Treat them as internal and avoid relying on them across versions unless G7 Development explicitly confirms support.

## ox_lib callback

### `g7_lean:server:summary`

Registered using `lib.callback.register`. The client awaits it to populate the interface. It returns a table containing:

- `syrups`: array of configured syrup keys, labels, item IDs, counts, and colors.
- `sodas`: array of configured soda keys, labels, item IDs, counts, and colors.
- `items`: inventory counts keyed by configured item names, plus convenience keys such as `emptyBottle`, `filledBottle`, `emptyCup`, `sodaCup`, `ice`, and `drink`.

This callback is an internal UI data source, not a documented third-party API guarantee.

## Public exports

No public Lua exports were explicitly declared in the supplied `fxmanifest.lua` or resource source. Do not expect `exports.g7_leansystem:...` methods in version 1.0.0 unless they are added in a future release.
