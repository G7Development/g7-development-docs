# G7 Black Market — Installation

## Requirements
- `oxmysql`
- A framework (Qbox, QBCore, ESX, or custom) and a supported inventory
- `ox_lib` (optional: notifications, progress bars and heist hack minigame)
- `ox_target` or `qb-target` (optional; `[E]` prompts are used otherwise)

## Step 1 — Add the resource
Drop `g7_blackmarket` into your resources folder.

## Step 2 — Install the items and images
Everything is in `install/`. Use the row for your inventory and skip items you already have.

| Inventory | Item file | Images (copy into) |
|---|---|---|
| **g7_inventory** | `install/ox_inventory_g7_inventory_qbox.lua` → `g7_inventory/data/items.lua` | `install/images/ox/*` → `g7_inventory/web/images/` |
| **ox_inventory / Qbox** | `install/ox_inventory_g7_inventory_qbox.lua` → `ox_inventory/data/items.lua` | `install/images/ox/*` → `ox_inventory/web/images/` |
| **QBCore** (qb / ps / lj / codem / core) | `install/qb-core_shared_items.lua` → `qb-core/shared/items.lua` | `install/images/qb/*` → your inventory image folder |
| **qs-inventory** | `install/qs-inventory_items.lua` → `qs-inventory/shared/items.lua` | `install/images/qb/*` → `qs-inventory/html/images/` |
| **ESX** (default inventory) | Run `install/esx_items.sql` | `install/images/qb/*` → your inventory image folder |

## Step 3 — Add your logo
Replace `html/g7.png` with your own G7 logo.

## Step 4 — server.cfg
```cfg
ensure oxmysql
# your framework + inventory here
ensure g7_blackmarket
ensure g7_blackmarket_phone  # optional, must start AFTER g7_blackmarket
```

## Step 5 — Database
Nothing to do. Tables are created automatically on first start.

## Step 6 — Configure
- **Theme:** `Config.Theme.preset` (`'frost'` default, `'g7'`, `'toxic'`, `'ember'`, `'gold'`, `'mono'` or `'custom'`)
- **Heist sites:** check `Config.HeistLocations` on your map
- **Crews:** `Config.Crews` (default: $25k, rank 2)
- **Laundering:** `Config.Laundering`
- **Market events:** `Config.MarketEvents`
- **Weekly prizes:** `Config.WeeklyPrizes` (default Monday 18:00 server time)
- **Serial tracing:** `Config.Serials.reveal` (`name`, `partial` or `none`)
- **Trusted resources:** list resources allowed to open the UI remotely in `Config.RemoteOpenResources` (add `g7_blackmarket_phone` if used)
- **Item names:** QB-style names are translated automatically; add renames in `Config.ItemAliases`
- **Custom framework:** set `Config.Framework = 'custom'` and fill in the `Custom` table in `bridge/server.lua`

## Step 7 — Test
1. Restart the server and visit a mission giver.
2. As admin: `/bmsetrep [id] [amount]` to test rank unlocks, and `/bmevent sale` to test a market event.
3. As police: buy a weapon and run `/bmtrace [serial]`.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.