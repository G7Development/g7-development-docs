# Installation

## 1. Install the resource

1. Back up your server and inventory item definitions.
2. Extract the `g7_leansystem` folder into your server's resources directory. Keep the folder name unchanged.
3. Copy the G7 PNG item images from `install/inventory/images/` into your inventory's item-image directory.
4. Merge the item definitions from the appropriate example file into your existing inventory registry. Do not replace the entire registry.
5. Configure `Config.Framework` and `Config.Inventory` in `config.lua` if automatic detection is not appropriate for your server.

## 2. Register the items

- **ox_inventory:** merge `install/inventory/items.lua` into the item definitions format used by your installed version. Copy images to `ox_inventory/web/images/` if that is your image directory.
- **QBCore/Qbox with qb-style item definitions:** merge `install/inventory/qb-items.lua` into `qb-core/shared/items.lua` or the equivalent shared item file used by your Qbox setup.
- **qs-inventory:** the included QB-style definitions may be a starting point only. Confirm your installed version's item format and API behavior.
- **Custom inventory:** follow [Custom inventory adapter](../integrations/custom-inventory.md).

Keep metadata-bearing items non-stackable: `g7_lean_bottle_filled`, `g7_lean_cup_soda`, and `g7_lean_drink`.

## 3. Start resources in order

Example for QBCore:

```cfg
ensure ox_lib
ensure qb-core
# Ensure your inventory resource starts here.
ensure g7_leansystem
```

For Qbox, use your server's normal Qbox startup entry (commonly `qbx_core`) instead of `qb-core`. For ESX, start `es_extended`. Start the inventory resource before `g7_leansystem`.

## 4. Verify startup

Restart the server or start the resource after dependencies. Check the server console for a line similar to:

```text
[G7 Lean System] Framework: qb | Inventory: ox
```

The values shown depend on your actual framework and inventory selection. If detection chooses the wrong inventory because multiple inventory resources are running, set `Config.Inventory` explicitly.

## 5. Test before production

On a test server, verify item use, item removal/addition, metadata retention, inventory-full behavior, passing drinks, and the effect cleanup. See [Troubleshooting](../support/troubleshooting.md) if a step fails.
