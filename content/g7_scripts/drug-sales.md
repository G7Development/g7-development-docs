# G7 Drug Sales + Trap Phone — Installation

## Requirements
- A framework: Qbox, QBCore or ESX
- A supported inventory: g7_inventory, ox_inventory, qs-inventory, qb-inventory (ps / lj)
- A target (optional): `ox_target` or `qb-target`; built-in 3D text is used otherwise
- `oxmysql` (optional; KVP is used without SQL)
- **No ox_lib needed**

## Step 1 — Add the resources
Put **both** `g7_drugsales` and `g7_trapphone` in your resources folder.

## Step 2 — Install the items and image
1. Add the items from `install/items/` for your inventory.
2. Copy the image from `install/images/` into your inventory image folder.

## Step 3 — Add your logo
Replace `html/img/g7.png` with your G7 logo.

## Step 4 — server.cfg
Start these after your framework, inventory and target. `g7_trapphone` goes first:
```cfg
ensure g7_trapphone
ensure g7_drugsales
```

## Step 5 — Database
Nothing to do. With oxmysql the table is created automatically; without it, KVP is used.

## Step 6 — Configure (`config.lua`)
- **Drugs:** add entries to `Config.Drugs` (item name as key), or register them from another resource with `exports.g7_drugsales:RegisterDrug(...)`
- **Payout:** `Config.Payout.Type` = `'item'` (default `black_money`), `'account'` or `'markedbills'`
- **Police:** minimum cops, price bonus per cop, and dispatch (ps-dispatch, cd_dispatch, qs-dispatch, built-in or custom)
- **Zones:** hotspots and blacklisted zones (PD, hospital)
- **Locations:** use `/g7dscoords` in game to copy a `vec4(...)` for new spots
- **Phone prop (optional):** stream your own model in `g7_trapphone`

## Step 7 — Test
1. Restart the server.
2. Give yourself a drug item from your config.
3. Run `/drugphone` to open the phone, then `/selldrugs` to start corner selling.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.