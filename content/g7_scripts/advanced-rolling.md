# G7 Advanced Rolling — Installation

## Requirements
- `ox_lib`
- `oxmysql`
- A framework (Qbox, QBCore or ESX) and supported inventory (g7_inventory, ox_inventory, qs-inventory, qb-inventory / ps / lj, ESX default)
- `g7_vapesystem` (optional, for live theme sync)

## Step 1 — Add the resource
Drop `g7_advancedrolling` into your resources folder.

## Step 2 — Install the items
Add the items for your inventory from `install/`:

| Inventory | File | Where |
|---|---|---|
| g7_inventory / ox_inventory | `items_ox_g7_inventory.lua` | `data/items.lua` |
| qb-inventory / ps / lj | `items_qb_core.lua` | `qb-core/shared/items.lua` |
| qs-inventory | `items_qs_inventory.lua` | `qs-inventory/shared/items.lua` |
| ESX default inventory | `items_esx.sql` | Run in your database |

If your server already has a lighter item, skip `lighter` and set `Config.Lighter.item` to your item name.

## Step 3 — Install the images
Copy `install/images/*` into your inventory image folder:
- ox / g7: `web/images`
- qb: `html/images`
- qs: `html/images`

## Step 4 — server.cfg
```cfg
ensure ox_lib
ensure oxmysql
# your framework + inventory here
ensure g7_advancedrolling
```

## Step 5 — Database
Nothing to do. The `g7_rolling_stats` table is created automatically on first start.

## Step 6 — Full restart
Do a **full server restart** the first time (not just `restart g7_advancedrolling`) so the streamed smoke particle (`g7rollsmoke.ypt`), props and animation load.

## Step 7 — Configure
- **Discord logs:** `server/sv_config.lua`
- **Rolling mode:** `Config.Rolling.mode = 'tray'` (3D tray) or `'classic'`
- **UI:** `Config.UI.style = 'g7'` or `'ox_lib'`
- **Theme:** `Config.Theme` (`g7`, `frost`, `toxic`, `ember`, `gold`, `mono` or `'custom'`); set `syncWithVape = true` to follow g7_vapesystem
- **Image path:** if images aren't in the default folder, set `Config.UI.images`, e.g. `'nui://g7_inventory/web/images/%s.png'`
- **Stress relief:** `Config.Smoking.stress = 'auto'` uses `hud:server:RelieveStress` (QB/Qbox) or `esx_status` (ESX)
- **Weaker PCs:** `Config.Tray.quality = 'low'`

## Step 8 — Test
1. Give yourself a strain (e.g. `weed_og_kush`), a `weed_grinder`, a wrap (e.g. `wrap_original`) and a `lighter`.
2. Use the weed to open the rolling tray.
3. Run `/rollstats` and `/strains` to confirm stats save.
4. Admins can use `/bluntgrip` to line up the blunt in the hand.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.