# G7 Partners — Installation

## Requirements
- `ox_lib`
- `oxmysql`
- A framework: QBCore, Qbox or ESX (auto-detected)
- Optional: `ox_target` or `qb-target` (commands + `[E]` prompts are used if neither is running)
- Optional: `screenshot-basic` + a Fivemanage key or Discord webhook (for the photo booth)

## Step 1 — Add the resource
Drop `g7_partners` into your resources folder.

## Step 2 — Add your logo
Put your `g7.png` logo in `web/img/` (the logo slot is hidden if missing).

## Step 3 — Install the items
Add the items for your inventory from `install/items/`:
- **g7_inventory / ox_inventory** → `ox_inventory.lua` (uses `client = { export = 'g7_partners.useItem' }`)
- **qb-inventory / ps / lj** → `qb-core.lua` (into `qb-core/shared/items.lua`)
- **qs-inventory** → `qs-inventory.lua`
- **Default ESX inventory** → run `esx_items.sql`

## Step 4 — Install the images
Copy the 13 item images from `install/images/` into your inventory image folder.

## Step 5 — server.cfg
```cfg
ensure ox_lib
ensure oxmysql
# your framework + inventory + target here
ensure screenshot-basic  # optional, for the photo booth
ensure g7_partners
```

## Step 6 — Database
Nothing to do. Tables are created automatically, and updating from 1.0 adds new columns. (`install/g7_partners.sql` is available if you prefer to run it manually.)

## Step 7 — Jobs (optional)
- Create a `weddingplanner` job.
- Make sure your judge / lawyer jobs match `Config.Court.Jobs`.

## Step 8 — Configure (`config.lua`)
- **Locations:** move `Config.Marriage.Venues`, `Config.GiftShop.Ped.coords`, `Config.Court.Ped.coords` and `Config.PhotoBooth.Coords` to fit your map (the yacht venue needs the yacht IPL loaded)
- **Photo booth upload:** `Config.PhotoBooth.Upload` — use an **upload-only** key, since it is sent to players while they use the booth. Lower `MaxSizeKB` if your host's limit is smaller.
- **Date spots:** `Config.Dates.Spots`
- **Units:** `Config.Units = 'imperial'` (or `'metric'`)
- **Profile images:** allowed hosts in `Config.Profiles.ImageHosts`
- **Custom inventory:** set `Config.Inventory = 'custom'` and fill in `bridge/server/custom_inventory.lua` and `bridge/client/custom_inventory.lua`

## Step 9 — Test
1. Restart. The console should show something like `[g7_partners] framework: qb | inventory: g7 | active couples: 12`.
2. Press **F7** or run `/partners` to open the Couple Hub.
3. As admin, run `/partnersadmin start [id] [otherId]` to make two players a couple instantly for testing.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.