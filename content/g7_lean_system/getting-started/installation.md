# G7 Lean System — Installation

## Requirements

- `ox_lib`
- One framework: QBCore, Qbox or ESX
- One inventory: ox_inventory, qb-inventory, qs-inventory, or a custom adapter
- `ox_target` or `qb-target` (optional, only needed for the target option on placed cups)
- `g7_advanced_progressbar` (optional, any supported progress bar also works)

## Steps

1. Copy the `g7_leansystem` folder into your server's `resources` folder. Keep the folder name exactly `g7_leansystem`.
2. Add the items to your inventory (see the section for your inventory below).
3. Copy every PNG from `install/inventory/images/` into your inventory's item image folder.
4. Add the resource to `server.cfg`, after its dependencies and your inventory:

   ```cfg
   ensure ox_lib
   ensure qb-core            # or qbx_core / es_extended
   ensure ox_inventory       # or qb-inventory / qs-inventory
   ensure g7_leansystem
   ```

5. Restart the server and check the console. You should see the detected framework and inventory.
6. In game, use any G7 lean item, or press `F7` / type `/leanmenu`, to open the mixing lab.

## ox_inventory

1. Merge the entries from `install/inventory/items.lua` into `ox_inventory/data/items.lua`. Do not replace the whole file.
2. Copy the PNGs to `ox_inventory/web/images/`.
3. Keep `consume = 0` and the `export = 'g7_leansystem.useItem'` line on every G7 item.
4. Keep `g7_lean_bottle_filled`, `g7_lean_cup_soda` and `g7_lean_drink` non-stackable (`stack = false`).
5. Restart `ox_inventory`, then `g7_leansystem`.

## qb-inventory / Qbox

1. Merge the entries from `install/inventory/qb-items.lua` into `qb-core/shared/items.lua` (or your Qbox shared items file). Do not replace the whole table.
2. Copy the PNGs to your inventory's image folder (for example `qb-inventory/html/images/`).
3. Keep `unique = true` on `g7_lean_bottle_filled`, `g7_lean_cup_soda` and `g7_lean_drink`.
4. Restart the server.

## qs-inventory

Start with `install/inventory/qb-items.lua` and the same PNGs. If your qs-inventory version does not support the same add / remove / use-item exports, set `Config.Inventory = 'custom'` and fill in the functions below.

## Custom inventory

In `config.lua` set `Config.Inventory = 'custom'` and fill in `Config.CustomInventory`:

```lua
Config.CustomInventory = {
    count = function(source, item) end,                  -- return the item count
    remove = function(source, item, count, metadata) end, -- return true on success
    add = function(source, item, count, metadata) end,    -- keep metadata, return true on success
    registerUse = function(item, callback) end            -- callback(source, itemData)
}
```

## Config choices

Set these in `config.lua` if auto-detection is not right for your server:

```lua
Config.Framework = 'auto'   -- auto, qb, qbox, esx
Config.Inventory = 'auto'   -- auto, ox, qb, qs, custom
Config.Locale    = 'en'     -- en, fr, es
Config.ProgressBar.Provider = 'auto' -- auto, g7, qb, qbox, esx, ox_lib, custom
Config.Interaction.Target = 'auto'   -- auto, ox_target, qb-target, custom, none
Config.Interaction.Text3D = 'auto'   -- auto, always, never
```

If you run more than one inventory resource, set `Config.Inventory` to the one players actually use.

## Shops

Shops are built in game with `/leanadmin` (no file editing). Give your admins access in `server.cfg`:

```
add_ace group.admin g7lean.admin allow
```

(Framework groups and identifiers can also be listed in `Config.Admin`.)

1. Run `/leanadmin` and press **New shop**.
2. Name it, pick cash / bank / both, then **Place shopkeeper** (aim and left click) or **Where I stand**.
3. Choose a map icon, colour and size. **Preview** shows it on your minimap.
4. Switch on the items it sells and set each price, then **Save shop**.

Shops save to `data/store.json` and appear for everyone immediately. Back up or move shops by copying that file. The three example shops in `Config.Shop.Locations` are only created the first time the script starts.

Interaction uses ox_target or qb-target when running (`Config.Shop.Target`), otherwise an `E` prompt. Money works with QBCore, Qbox and ESX out of the box. For standalone with ox_inventory, cash is the `money` item (`Config.Shop.CashItem`). For anything else, fill in `Config.CustomMoney`.
