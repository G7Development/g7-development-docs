# Items and metadata

Register every item ID below in the inventory used by your server. Item IDs are defined in `config.lua` and `shared/items.lua`.

## Item IDs

| Item ID | Default label / use |
|---|---|
| `g7_lean_syrup_violet` | Violet Reserve syrup |
| `g7_lean_syrup_royal` | Royal Purple syrup |
| `g7_lean_syrup_gold` | Golden Drop syrup |
| `g7_lean_syrup_crimson` | Crimson Blend syrup |
| `g7_lean_syrup_azure` | Azure Night syrup |
| `g7_lean_soda_citrus` | Citrus Soda |
| `g7_lean_soda_orange` | Orange Soda |
| `g7_lean_bottle_empty` | Empty Baby Bottle |
| `g7_lean_bottle_filled` | Measured Syrup Bottle |
| `g7_lean_cup_empty` | G7 Foam Cup |
| `g7_lean_cup_soda` | Soda-Filled Cup |
| `g7_lean_ice` | Ice Cubes |
| `g7_lean_drink` | G7 Mixed Drink |
| `g7_lean_cup_stack` | Stack of G7 Foam Cups |

## Items that must be unique

The following items carry per-instance state and must not stack together:

- `g7_lean_bottle_filled`
- `g7_lean_cup_soda`
- `g7_lean_drink`

The provided ox_inventory definitions use `stack = false` for these items. The QB-style definitions use `unique = true`.

## Metadata fields

The server writes a `g7lean = true` marker and recipe fields to metadata.

- **Measured bottle:** `syrup`, `lines`, `label`, `description`.
- **Soda cup:** `soda`, `label`, `description`.
- **Finished drink:** `syrup`, `soda`, `potency`, `lines`, `sips`, `maxSips`, `iced`, `label`, `description`.

Metadata is read differently depending on the inventory. ox_inventory uses slot metadata; QB/Qbox uses `info` or `metadata` on player item data. Some custom inventory builds store it differently and may require adapting `itemMeta()` in `server/main.lua`.

## Images

The package contains PNGs in `install/inventory/images/`. Copy the images into your inventory's item-image folder and keep the filenames matching the definitions. If your inventory expects a different item-image path or naming convention, update that inventory configuration accordingly.
