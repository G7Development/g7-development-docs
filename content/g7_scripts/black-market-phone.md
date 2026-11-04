# G7 Black Market Phone — Installation

## Requirements
- OneSync
- `oxmysql`
- A framework (Qbox, QBCore or ESX) and a supported inventory
- `screenshot-basic` (optional, for Camera and Gallery apps)
- `g7_blackmarket` (optional, for shared rep and Contracts app)

## Step 1 — Add the resource
Drop `g7_blackmarket_phone` into your resources folder.

## Step 2 — Install the items and images
Everything is in `install/`. Use the row for your inventory and skip items you already have.

| Inventory | Item file | Images (copy into) |
|---|---|---|
| **g7_inventory** | `install/ox_inventory_g7_inventory_qbox.lua` → `g7_inventory/data/items.lua` | `install/images/ox/*` → `g7_inventory/web/images/` |
| **ox_inventory / Qbox** | `install/ox_inventory_g7_inventory_qbox.lua` → `ox_inventory/data/items.lua` | `install/images/ox/*` → `ox_inventory/web/images/` |
| **QBCore** (qb / ps / lj / codem / core) | `install/qb-core_shared_items.lua` → `qb-core/shared/items.lua` | `install/images/qb/*` → your inventory image folder |
| **qs-inventory** | `install/qs-inventory_items.lua` → `qs-inventory/shared/items.lua` | `install/images/qb/*` → `qs-inventory/html/images/` |
| **ESX** (default inventory) | Run `install/esx_items.sql` | `install/images/qb/*` → your inventory image folder |

The burner phone item is `blackmarket_phone`. On ox/g7 its entry already calls `g7_blackmarket_phone.usePhone`; on QB/ESX it is registered as usable automatically.

## Step 3 — server.cfg
```cfg
ensure oxmysql
# your framework + inventory here
ensure screenshot-basic       # optional, for the camera
ensure g7_blackmarket         # optional, must start BEFORE the phone
ensure g7_blackmarket_phone

add_ace group.admin g7bmphone.admin allow
```

## Step 4 — Database
Nothing to do. Tables are created automatically on first start.

## Step 5 — Configure
- **Logs:** set `Config.Logs.service` to `discord` (with a webhook) or `fivemanage` (with an API key).
- **Drop locations:** 35 spots are preconfigured in `Config.DropLocations`. Test them on your map and move any that clash with MLOs. Add `ped = vector4(...)` to place a dealer yourself.
- **Phone look:** `Config.Phone.ui` (theme defaults to `'frost'`). Set `allowCustomize = false` to stop players changing wallpaper, frame color and size.
- **Custom phone prop (optional):** stream your prop, set `Config.Phone.prop`, then adjust `bone`, `offset` and `rotation`.
- **Admin groups:** add groups to `Config.Admin.groups` if you don't use the ACE above.
- **Item names:** config uses ox/g7 names (`WEAPON_PISTOL`, `ammo-9`, `armour`). QB-style inventories are translated automatically. Add renames in `Config.ItemAliases`.

## Step 6 — Test
1. Restart the server.
2. Give yourself a `blackmarket_phone` and use it.
3. Run `/bmphoneadmin` to check the admin app opens.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.