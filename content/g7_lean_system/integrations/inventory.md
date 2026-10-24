# Inventory integrations

The inventory bridge lives in `server/bridge.lua`. Set `Config.Inventory` in `config.lua` to the inventory players actually use.

## ox_inventory

Set:

```lua
Config.Inventory = 'ox'
```

Merge the entries from `install/inventory/items.lua` into your existing item definitions using the format expected by your installed ox_inventory version. Do not replace the entire item registry. Copy `install/inventory/images/*.png` into the configured image directory (commonly `ox_inventory/web/images/`).

The bridge uses `Search`, `RemoveItem`, `AddItem`, and `RegisterUsableItem` exports. Verify those APIs against your installed ox_inventory version.

## qb-inventory / Qbox

For QBCore, use `Config.Framework = 'qb'`; for Qbox, use `Config.Framework = 'qbox'`. Set `Config.Inventory = 'qb'` when using the QB-style inventory bridge.

Merge `install/inventory/qb-items.lua` into the appropriate shared item table. Do not replace your entire item file. Copy images to the directory used by your inventory.

The current bridge uses QBCore-style `GetPlayer`, `GetItemByName`, `RemoveItem`, `AddItem`, and `CreateUseableItem` methods. Confirm your Qbox/inventory version exposes compatible methods.

## qs-inventory

Set `Config.Inventory = 'qs'`. The bridge attempts QBCore-compatible player item methods first, then uses `qs-inventory` exports for count/removal/addition as applicable. QS APIs vary by release and configuration; this integration must be verified against the exact installed version. If the API contract differs, use the custom adapter rather than assuming compatibility.

## ESX

Set `Config.Framework = 'esx'` and select the inventory integration appropriate to your server. The supplied bridge uses ESX inventory methods as a fallback for count, removal, addition, and usable-item registration. The item metadata behavior of a plain ESX inventory may not support the unique metadata-bearing recipe items in the same way as ox_inventory/QB-style inventories. Validate metadata persistence before using the resource in production.

## Multiple inventories installed

Automatic detection chooses the first matching inventory in this order: ox_inventory, qs-inventory, qb-inventory, then custom. If more than one is started, explicitly set `Config.Inventory` to prevent detection from selecting the wrong one.
