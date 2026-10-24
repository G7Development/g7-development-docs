# G7 Drug Sales — Installation

This guide takes you from download to selling on your server in about 10 minutes. Follow the steps in order.

> **Need help?** Open a ticket in our [Discord](https://discord.gg/P6XQ4N6M95). Have your framework, inventory and any F8 / server console errors ready.

---

## Requirements

| | Supported |
|---|---|
| **Framework** | Qbox, QBCore, ESX |
| **Inventory** | g7_inventory, ox_inventory, qs-inventory, qb-inventory (also ps-inventory / lj-inventory) |
| **Target** | ox_target, qb-target, or none (built-in 3D text) |
| **Database** | oxmysql *(optional, saves rep and stats. Without it the script uses the server's built-in KVP storage)* |

There is **no ox_lib dependency**. The script detects your framework, inventory and target system automatically.

---

## 1. Download & Extract

Download **G7 Drug Sales** from your [Tebex / Cfx.re Portal](https://g7-development.tebex.io/) and extract it. You'll get two resources:

```
g7_drugsales/      ← the script
g7_trapphone/      ← the custom trap phone model (streamed prop)
```

Drop **both** folders into your server's `resources` folder, for example `resources/[g7]/`.

> ⚠️ Don't put `g7_trapphone` *inside* `g7_drugsales`. They are two separate resources and FiveM won't load one that's nested in another.

---

## 2. server.cfg

Add both resources **after** your framework, inventory, target and oxmysql:

```cfg
ensure oxmysql          # optional
ensure qb-core          # or qbx_core / es_extended
ensure ox_inventory     # or g7_inventory / qs-inventory / qb-inventory
ensure ox_target        # or qb-target (optional)

ensure g7_trapphone
ensure g7_drugsales
```

> ⚠️ Load order matters. If `g7_drugsales` starts before your framework or inventory, auto-detection can't find them.

---

## 3. Add the Items

All item definitions are in `g7_drugsales/install/items/`. Pick the file for **your inventory** and skip any items you already have.

**Items used by the script:**

| Item name | Label | Used for |
|---|---|---|
| `trapphone` | Trap Phone | Opens the trap phone (usable) |
| `black_money` | Dirty Money | Default payout item |
| `weed_og-kush` | OG Kush | Drug |
| `weed_white-widow` | White Widow | Drug |
| `weed_purple-haze` | Purple Haze | Drug |
| `cokebaggy` | Coke Baggy | Drug |
| `crack_baggy` | Crack Baggy | Drug |
| `xtcbaggy` | Ecstasy | Drug |
| `meth` | Meth | Drug |
| `oxy` | Oxy | Drug |
| `lean` | Lean | Drug |

### g7_inventory / ox_inventory / Qbox

File: `install/items/g7_inventory.lua` or `install/items/ox_inventory.lua`

For ox_inventory, paste into `ox_inventory/data/items.lua`, inside the `return { }` table:

```lua
['trapphone'] = {
    label = 'Trap Phone',
    weight = 200,
    stack = false,
    close = true,
    description = 'Burner for the plug.',
    client = { export = 'g7_drugsales.useTrapPhone' },
},
['black_money']      = { label = 'Dirty Money', weight = 0,   stack = true, close = true },
['weed_og-kush']     = { label = 'OG Kush',     weight = 50,  stack = true, close = true },
['weed_white-widow'] = { label = 'White Widow', weight = 50,  stack = true, close = true },
['weed_purple-haze'] = { label = 'Purple Haze', weight = 50,  stack = true, close = true },
['cokebaggy']        = { label = 'Coke Baggy',  weight = 50,  stack = true, close = true },
['crack_baggy']      = { label = 'Crack Baggy', weight = 50,  stack = true, close = true },
['xtcbaggy']         = { label = 'Ecstasy',     weight = 50,  stack = true, close = true },
['meth']             = { label = 'Meth',        weight = 50,  stack = true, close = true },
['oxy']              = { label = 'Oxy',         weight = 50,  stack = true, close = true },
['lean']             = { label = 'Lean',        weight = 250, stack = true, close = true },
```

### QBCore (qb-inventory / ps-inventory / lj-inventory)

File: `install/items/qb_qbox_items.lua`. Paste it into `qb-core/shared/items.lua`, inside `QBShared.Items = { }`.

```lua
['trapphone'] = { name = 'trapphone', label = 'Trap Phone', weight = 200, type = 'item', image = 'trapphone.png', unique = true, useable = true, shouldClose = true, description = 'Burner for the plug.' },
-- ...the rest of the items are in the file
```

### qs-inventory

File: `install/items/qs_inventory.lua`. Paste it into `qs-inventory/shared/items.lua`, inside `ItemList = { }`.

### ESX (default inventory)

Run `install/items/esx_items.sql` on your database.

> ℹ️ On ESX, `black_money` is an **account**, not an item. See [Step 6](#6-payout-setup).

---

## 4. Add the Item Images

All item images are in `g7_drugsales/install/images/`. They are 512×512 transparent PNGs, and each file is already named after its item.

Copy **every `.png`** into your inventory's image folder:

| Inventory | Image folder |
|---|---|
| g7_inventory | `g7_inventory/html/images/` |
| ox_inventory | `ox_inventory/web/images/` |
| qs-inventory | `qs-inventory/html/images/` |
| qb-inventory | `qb-inventory/html/images/` |
| ps-inventory / lj-inventory | `<inventory>/html/images/` |

The trap phone UI loads its drug images from the same folder, so this step covers both.

---

## 5. Database (optional)

If **oxmysql** is running, the script creates its `g7_drugsales` table automatically on first start. You don't need to run anything.

If you'd rather create it yourself, run `install/g7_drugsales.sql`.

Without oxmysql, rep and stats are saved in the server's built-in KVP storage, and no SQL is needed.

---

## 6. Payout Setup

By default, players are paid with the `black_money` **item**. Change this in `config.lua` to match your server:

```lua
Config.Payout = {
    Type    = 'item',          -- 'item' | 'account' | 'markedbills'
    Item    = 'black_money',   -- used when Type = 'item'
    Account = 'cash',          -- used when Type = 'account'
}
```

| Your setup | Use |
|---|---|
| g7_inventory / ox_inventory / qs-inventory | `Type = 'item'`, `Item = 'black_money'` |
| ESX (default inventory) | `Type = 'account'`, `Account = 'black_money'` |
| QBCore with marked bills | `Type = 'markedbills'` |
| Clean cash | `Type = 'account'`, `Account = 'cash'` |

---

## 7. Trap Phone Model

`g7_trapphone` already includes the G7 trap phone model, so there's nothing to configure. When a player opens the phone, it appears in their hand.

If the model ever fails to load, the script falls back to the normal GTA phone automatically.

**Adjusting how it sits in the hand** (admins):

1. Open the phone with `/drugphone`.
2. Run `/phoneprop x y z rx ry rz` (for example `/phoneprop 0.0 0.02 -0.01 0 0 180`).
3. Repeat until it looks right. Each run shows the line and copies it for you.
4. Paste it into `config.lua` → `Config.Phone.Prop` and restart the resource.

---

## 8. Admin Permissions

The admin commands (`/g7dscoords`, `/phoneprop`) work for anyone with your framework's admin permission. To allow them with ACE permissions instead, add this to `server.cfg`:

```cfg
add_ace group.admin g7drugsales.admin allow
```

---

## 9. Restart & Test

Restart your server, or run `ensure g7_trapphone` then `ensure g7_drugsales`. The server console should show:

```
[G7] g7_drugsales  framework: qb  inventory: ox_inventory
[G7] g7_drugsales  storage: oxmysql
```

**Quick test:**

1. Give yourself some product, e.g. `/giveitem 1 weed_og-kush 20` (QB) or `/additem 1 weed_og-kush 20` (ox).
2. Run `/selldrugs`, pick the drug, and wait for a customer to walk up.
3. Run `/drugphone` and accept an order in **Orders**.

---

## Commands

| Command | Who | What it does |
|---|---|---|
| `/drugphone` | Everyone | Opens / closes the trap phone |
| `/selldrugs` | Everyone | Starts corner selling (run it again to stop) |
| `/stopselling` | Everyone | Stops corner selling |
| `/canceldelivery` | Everyone | Cancels the active order or batch |
| `/g7dscoords` | Admin | Copies a `vec4(...)` of your position for adding drop locations |
| `/phoneprop x y z rx ry rz` | Admin | Positions the phone model in hand live |

All command names can be changed in `Config.Commands`. To give the phone a keybind, set `Config.Keybinds.Phone` (for example `'F6'`).

---

## Adding Your Own Drugs

Add an entry to `Config.Drugs`, using the **item name** as the key:

```lua
['my_drug'] = {
    label = 'My Drug',
    street   = { price = { 100, 180 }, amount = { 1, 3 } },   -- corner selling
    delivery = { price = { 140, 220 }, amount = { 4, 12 } },  -- phone orders
    batch    = { price = { 90, 150 },  amount = { 15, 45 } }, -- gang batch drops
    minLevel = 1,                                             -- rep level needed (optional)
},
```

Other resources can also register drugs from their own server code:

```lua
exports.g7_drugsales:RegisterDrug('lean_cup', {
    label = 'Lean Cup',
    street   = { price = { 350, 500 }, amount = { 1, 2 } },
    delivery = { price = { 420, 560 }, amount = { 2, 8 } },
    batch    = { price = { 300, 420 }, amount = { 8, 30 } },
})
```

---

## Overriding Auto-Detection

Auto-detection works for most servers. To force a specific system, set it in `config.lua`:

```lua
Config.Framework = 'auto'   -- 'qbx' | 'qb' | 'esx'
Config.Inventory = 'auto'   -- 'g7_inventory' | 'ox_inventory' | 'qs-inventory' | 'qb-inventory'
Config.Target    = 'auto'   -- 'ox_target' | 'qb-target' | '3dtext'
Config.Storage   = 'auto'   -- 'oxmysql' | 'kvp'
```

---

## Troubleshooting

<details>
<summary><b>Console says "framework: standalone" or the wrong inventory</b></summary>

`g7_drugsales` is starting before your framework or inventory. Move `ensure g7_drugsales` lower in `server.cfg`, or set `Config.Framework` / `Config.Inventory` by hand.
</details>

<details>
<summary><b>"You have nothing to sell"</b></summary>

The item names in your inventory must match the keys in `Config.Drugs` exactly, including dashes and underscores (e.g. `weed_og-kush`). Also check the drug's `minLevel`.
</details>

<details>
<summary><b>Drug images don't show in the phone or the sell menu</b></summary>

Copy the PNGs from `install/images/` into your inventory's image folder ([Step 4](#4-add-the-item-images)). If your inventory keeps images somewhere else, change the path in `Config.ImagePaths`.
</details>

<details>
<summary><b>No "Sell drugs" option on customers</b></summary>

Make sure your target resource starts **before** `g7_drugsales`. For customers in cars, aim at the **car**, not the driver. To skip target completely, set `Config.Target = '3dtext'`.
</details>

<details>
<summary><b>Players don't get paid</b></summary>

The payout item or account doesn't exist on your server. See [Step 6](#6-payout-setup). ESX servers using the default inventory need `Type = 'account'`.
</details>

<details>
<summary><b>The phone is a normal GTA phone, not the G7 model</b></summary>

`g7_trapphone` isn't running, or it starts after `g7_drugsales`. Make sure `ensure g7_trapphone` comes first and the console shows no stream errors.
</details>

<details>
<summary><b>No customers spawn / "Too hot around here"</b></summary>

You're inside a blacklisted zone (PD, hospital). Move away, or edit `Config.Zones.Blacklisted`. If `Config.Zones.RestrictToZones = true`, you can only sell inside a hotspot.
</details>

<details>
<summary><b>"Nobody is buying right now..."</b></summary>

There aren't enough police on duty for `Config.Police.MinCops`. Set the values to `0` to disable the requirement.
</details>

---

**G7 Development** · [Store](https://g7-development.tebex.io/) · [Discord](https://discord.gg/P6XQ4N6M95)
