# G7 Baddies VIP (`g7_vipfemale`) — Installation

## Requirements
- `oxmysql`
- OneSync
- **No ox_lib needed**
- Framework: Qbox, QBCore, ESX or standalone (auto-detected)
- Inventory: g7_inventory, ox_inventory, qs-inventory, lj-inventory, qb-inventory or ESX inventory (auto-detected)

## Step 1 — Add the resource
Drop `g7_vipfemale` into your resources folder.

## Step 2 — server.cfg
Add these **after** oxmysql, your framework and your inventory:
```cfg
ensure g7_vipfemale
add_ace group.admin vipfemale.admin allow
```

## Step 3 — Install the items
Add the items for your inventory from `install/`:

| Inventory | File |
|---|---|
| g7_inventory, ox_inventory (Qbox) | `install/items_ox.lua` |
| qb-inventory, lj-inventory, ps-inventory | `install/items_qb.lua` |
| qs-inventory | `install/items_qs.lua` |
| ESX default / esx_inventoryhud | `install/items_esx.sql` |

## Step 4 — Install the images
Copy `install/images/*.png` into your inventory image folder. If images live elsewhere, set `Config.ItemImagePath` (e.g. `nui://g7_inventory/web/images/`).

## Step 5 — Add your logo
Replace `web/img/g7.png` with your logo (square PNG, 512×512 looks best).

## Step 6 — Database
Nothing to do. Tables are created automatically.

## Step 7 — Set who can open it
In `config.lua`, set `Config.Access.mode`:
- `female` — saved character gender
- `discord` — one of the Discord roles in `config_server.lua`
- `either` / `both`
- `open` — everyone

Staff can always open it (`Config.Access.staffBypass`).

## Step 8 — Server config (`config_server.lua`)
- Discord bot token and roles (if using Discord access or the Closet role lock)
- Discord webhooks
- Tebex packages (see Step 9)

## Step 9 — Tebex (optional)
1. List packages under `ServerConfig.Tebex.packages` (each has a key, label and reward).
2. In Tebex, give the package this server command (the last word must match a package key):
   ```
   vipfemale_tebex {transaction} vip_month
   ```
3. Optional chargeback command: `vipfemale_tebex_revoke {transaction}`
4. Players claim in **Rewards → Store Claim** with their `tbx-…` transaction ID.

## Step 10 — Hook up your other resources
Check `editable/client.lua` and `editable/server.lua`. Vehicle keys, fuel, clothing/appearance and dispatch are auto-detected; add your own resource there if it isn't listed.
- **Dirty money:** drug sales pay `black_money` by default. Set `Config.Drug.sell.account = 'cash'` if you don't use dirty money.
- **Closet:** auto-detects your appearance system, or set `Config.Closet.system = 'custom'`.

## Step 11 — Test
1. Restart the server.
2. Type `/vipfemale`.
3. If you can't open it, use `/vipfemale_allow [id]` or check `Config.Access.mode`.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.