# G7 Beauty — Installation

## Requirements

- **ox_lib** and **oxmysql**
- **OneSync** turned on
- **Framework:** Qbox, QBCore or ESX (detected automatically)
- **Inventory:** g7_inventory, ox_inventory, qs-inventory, qb-inventory or the ESX default (detected automatically)
- **Target:** ox_target or qb-target (detected automatically)
- **Game build 2699 or newer.** Add this to your `server.cfg`:

  ```cfg
  set sv_enforceGameBuild 3095
  ```

## 1. Add the resource

1. Put the `g7_beauty` folder in your `resources` folder.
2. In `server.cfg`, start it **after** your framework, inventory, target, ox_lib and oxmysql:

   ```cfg
   ensure g7_beauty
   ```

## 2. Database

Nothing to do. The tables are created automatically the first time the script starts. If you'd rather create them yourself, run `sql/install.sql`.

## 3. Items

Add the items to your inventory using the matching file in the `install` folder, and copy the item images from `install/images/` into your inventory's images folder.

| Inventory | Item file | Images go in |
|---|---|---|
| g7_inventory / ox_inventory | `install/ox_items.lua` | your inventory's `web/images/` folder |
| qb-inventory (and ps / lj) | `install/qb_items.lua` | `qb-inventory/html/images/` |
| qs-inventory | `install/qs_items.lua` | `qs-inventory/html/images/` |
| ESX default | `install/esx_items.sql` | your inventory's images folder |

Press-on nails need an inventory that supports item metadata, so they turn off automatically on the ESX default inventory.

## 4. Jobs

The script uses two jobs:

- `nailtech` for nails
- `makeupartist` for makeup and the Lash Bar

Copy the job setup for your framework from `install/jobs.md` (Qbox, QBCore and ESX examples are included). If you want different job names, change them in `Config.Services` in `config.lua`.

## 5. Salon locations

Open `config.lua` and set your salon locations under `Config.Salons`: the nail stations, lash beds, vanities, kiosk and boss menu spot. The example coordinates are only placeholders, so replace them with the spots in your MLO.

To grab a position, turn on `Config.Debug`, stand where the chair or spot should be and type `/beautypos`. The position is copied to your clipboard.

## 6. Admin permission (for vanities)

To let admins manage any placed vanity, add this to `server.cfg`:

```cfg
add_ace group.admin g7beauty.admin allow
```

## 7. Image uploads (optional)

To let players upload their own images onto nails, add **one** of these to `server.cfg` (never to the config):

```cfg
set g7_beauty_fivemanage "YOUR_FIVEMANAGE_KEY"
```

Or:

```cfg
set g7_beauty_webhook "https://discord.com/api/webhooks/..."
```

FiveManage is recommended, because Discord image links expire after about a day and the image disappears from the nails.

## 8. Before going live

In `config.lua`:

- Set `Config.Debug = false`.
- Pick your theme with `Config.UI.theme`: `'light'` (Flower Wall) or `'dark'` (Pink Jungle Neon).
- Look through the shop prices (`Config.Shop`), refill settings (`Config.Refill`) and nail wear settings (`Config.Wear`).

Then restart the resource and you're done.

---

## Player commands

| Command | What it does |
|---|---|
| `/mynails` | Do your own nails (needs a Nail Kit) |
| `/mymakeup` | Do your own makeup |
| `/mylashes` | Do your own lashes (needs tweezers, glue and lashes) |
| `/nails` | Look at your nails |
| `/shownails` | Show your nails off to nearby players |
| `/nailcheck` | See if any nails are broken or worn |
| `/standup` | Stand up from a salon seat or vanity (BACKSPACE or X also work) |
| `/beautystats` | Your beauty job level, clients and earnings |

## Exports

Other scripts can show a G7 Beauty notification:

```lua
exports.g7_beauty:Notify('Your set is ready', 'success', 5000)
```

---

Store: [https://g7-development.tebex.io/](https://g7-development.tebex.io/) · Discord: [https://discord.gg/P6XQ4N6M95](https://discord.gg/P6XQ4N6M95)
