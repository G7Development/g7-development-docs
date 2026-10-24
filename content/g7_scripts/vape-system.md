# G7 Vape System — Installation

This guide takes you from download to a working vape system on your server. It takes about 10 minutes.

## Requirements

| Requirement | Notes |
|---|---|
| **OneSync** | Must be enabled on your server. |
| **[ox_lib](https://github.com/overextended/ox_lib)** | Required. Start it before the vape system. |
| **Framework** | QBox, QBCore, ESX, or standalone. Detected automatically. |
| **Inventory** | ox_inventory, qb-inventory, ps-inventory, lj-inventory, or a custom inventory. Detected automatically. |
| **Vape props** | The `g7devvape` (vape pen) and `g7vapejuice` (juice bottle) prop resources. |
| **Target (optional)** | ox_target or qb-target for the shops. Without one, a built-in "Press E" prompt is used. |

No SQL is needed. Shops, flavour edits and the UI theme are saved in a JSON file inside the resource.

---

## Step 1: Add the resource

1. Download `g7_vapesystem` from your Keymaster / Tebex purchase.
2. Unzip it and place the `g7_vapesystem` folder in your server's `resources` folder.

> **Important:** do not rename the folder. It must stay `g7_vapesystem`.

---

## Step 2: Start order in `server.cfg`

`ox_lib` and the prop resources must start **before** the vape system:

```cfg
ensure ox_lib
ensure g7devvape        # vape pen prop
ensure g7vapejuice      # vape juice prop
ensure g7_vapesystem
```

If you use a framework, inventory or target resource, it should also start before `g7_vapesystem`.

---

## Step 3: Add the items

The script uses three items:

| Item | What it is |
|---|---|
| `g7_devvape` | The vape pen. Its tank, flavour, battery and condition are saved on the item. |
| `g7_vapejuice` | One item for **every** flavour of juice. The flavour and how much is left are saved on the item. |
| `g7_vapecharger` | The vape charger. Use it to charge a vape. |

Pick your inventory below.

### ox_inventory (QBox, ESX, or standalone)

Open `ox_inventory/data/items.lua` and paste this inside the table:

```lua
['g7_devvape'] = {
    label = 'Vape Pen',
    weight = 1,
    stack = false,
    close = true,
    consume = 0,
    description = 'Refillable pen',
    server = { export = 'g7_vapesystem.g7UseItem' },
},

['g7_vapejuice'] = {
    label = 'Vape Refill',
    weight = 1,
    stack = false,
    close = true,
    consume = 0,
    description = 'Use it to open the refill station.',
    server = { export = 'g7_vapesystem.g7UseItem' },
},

['g7_vapecharger'] = {
    label = 'Vape Charger',
    weight = 1,
    stack = true,
    close = true,
    consume = 0,
    description = 'USB-C fast charger. Use it to charge your vape.',
    server = { export = 'g7_vapesystem.g7UseItem' },
},
```

> **Note:** the vape and juice items must have `stack = false`. Each one carries its own data (tank level, battery, flavour), so they can't stack.

### qb-inventory, ps-inventory or lj-inventory (QBCore)

Open `qb-core/shared/items.lua` and paste this inside the table:

```lua
g7_devvape     = { name = 'g7_devvape',     label = 'Vape Pen',     weight = 1, type = 'item', image = 'g7_devvape.png',     unique = true,  useable = true, shouldClose = true, description = 'Refillable pen' },
g7_vapejuice   = { name = 'g7_vapejuice',   label = 'Vape Refill',  weight = 1, type = 'item', image = 'g7_vapejuice.png',   unique = true,  useable = true, shouldClose = true, description = 'Use it to open the refill station.' },
g7_vapecharger = { name = 'g7_vapecharger', label = 'Vape Charger', weight = 1, type = 'item', image = 'g7_vapecharger.png', unique = false, useable = true, shouldClose = true, description = 'USB-C fast charger. Use it to charge your vape.' },
```

<details>
<summary>Using an older QBCore with the <code>['name'] = { ... }</code> style?</summary>

```lua
['g7_devvape']     = { ['name'] = 'g7_devvape',     ['label'] = 'Vape Pen',     ['weight'] = 1, ['type'] = 'item', ['image'] = 'g7_devvape.png',     ['unique'] = true,  ['useable'] = true, ['shouldClose'] = true, ['description'] = 'Refillable pen.' },
['g7_vapejuice']   = { ['name'] = 'g7_vapejuice',   ['label'] = 'Vape Refill',  ['weight'] = 1, ['type'] = 'item', ['image'] = 'g7_vapejuice.png',   ['unique'] = true,  ['useable'] = true, ['shouldClose'] = true, ['description'] = 'Opens the refill station.' },
['g7_vapecharger'] = { ['name'] = 'g7_vapecharger', ['label'] = 'Vape Charger', ['weight'] = 1, ['type'] = 'item', ['image'] = 'g7_vapecharger.png', ['unique'] = false, ['useable'] = true, ['shouldClose'] = true, ['description'] = 'Charges your vape.' },
```

</details>

### Custom inventory

1. In `config.lua`, set:

   ```lua
   Config.Inventory = 'custom'
   Config.CustomInventory.resource = 'your_inventory'
   ```

2. Your inventory must provide these **server exports**. The names can be changed in `Config.CustomInventory.exports`. Every item is a table `{ name, count, metadata, slot }`, metadata must be saved per slot, and the vape and juice items must not stack.

   | Export | Arguments | Returns |
   |---|---|---|
   | `GetItemBySlot` | `(src, slot)` | the item, or `nil` |
   | `GetItemsByName` | `(src, name)` | a list of items |
   | `SetMetadata` | `(src, slot, metadata)` | `true` / `false` |
   | `AddItem` | `(src, name, count, metadata)` | `true` / `false` |
   | `RemoveItem` | `(src, name, count, slot)` | `true` / `false` |
   | `CanCarryItem` | `(src, name, count)` | `true` / `false` |

3. When a player uses `g7_devvape`, `g7_vapejuice` or `g7_vapecharger`, call:

   ```lua
   exports.g7_vapesystem:UseItem(source, itemName, slot)
   ```

---

## Step 4: Add the item images

Copy **every** image from `g7_vapesystem/install/images/` into your inventory's image folder:

| Inventory | Image folder |
|---|---|
| ox_inventory | `ox_inventory/web/images/` |
| qb-inventory | `qb-inventory/html/images/` |
| ps / lj-inventory | the `html/images/` folder of that inventory |

The images are:

```text
g7_devvape.png
g7_vapecharger.png
g7_vapejuice.png
g7_vapejuice_beastice.png
g7_vapejuice_coolmint.png
g7_vapejuice_mangofreeze.png
g7_vapejuice_pinklemonade.png
g7_vapejuice_purplehaze.png
g7_vapejuice_unflavoured.png
g7_vapejuice_watermelonice.png
```

> **Tip:** on ox_inventory every flavour shows its own bottle picture. Other inventories show `g7_vapejuice.png` for all flavours.

---

## Step 5: Admin permissions

Admins can open `/vapeadmin` and use the admin tools. Add this to `server.cfg`, and change `group.admin` to your own admin group if it's different:

```cfg
add_ace group.admin g7vape.admin allow
```

Framework groups `admin`, `god` and `superadmin` are also allowed automatically. You can change those groups, or add single players by license or Discord ID, in `Config.Admin` in `config.lua`:

```lua
Config.Admin = {
    ace         = 'g7vape.admin',
    groups      = { 'admin', 'god', 'superadmin' },
    identifiers = { 'license:xxxxxxxx', 'discord:xxxxxxxx' },
}
```

---

## Step 6: Full server restart

Do a **full server restart** the first time. Restarting just the resource is not enough.

> **Why:** the hold animation (`g7vapehold.ycd`) and the smoke effect (`g7smokecustom.ypt`) are streamed files, and FiveM only loads new streamed files on a full restart.

---

## Step 7: Test it

A test shop called **Vape Shop** is created at Legion Square the first time the script starts. You can edit or delete it in `/vapeadmin`.

You can also give yourself items. These commands work in game for admins and in the server console:

```text
/g7givevape <id> g7_devvape 3     a vape with 3 ml of juice in it
/g7givevape <id> beastice         a full bottle of Beast Ice
/g7givevape <id> charger          a vape charger
```

Then check:

- [ ] Use the vape: it appears in your hand and the side HUD shows the tank, flavour, battery and condition.
- [ ] Hold **E** to hit it, let go to exhale: a cloud in the flavour's colour comes out.
- [ ] Use the bottle: the refill station opens.
- [ ] Use the charger: the charge station opens.
- [ ] Walk up to the shopkeeper: the shop opens.

---

## Configuration

Everything can be changed in `config.lua`. The most common settings:

| Setting | What it changes |
|---|---|
| `Config.Framework` | `'auto'`, or force `'qbx'`, `'qb'`, `'esx'` or `'standalone'`. |
| `Config.Inventory` | `'auto'`, or force `'ox'`, `'qb'` or `'custom'`. |
| `Config.Notify` | `'ox_lib'`, `'nui'`, `'qb'`, `'esx'` or `'custom'`. |
| `Config.Devices` | Vape models: tank size, how fast they use juice and battery, price. |
| `Config.Flavors` | Flavour names, colours, pictures and smoke colours. |
| `Config.Battery` | Battery drain per hit and while the vape is held out. |
| `Config.Durability` | How fast vapes wear out and break. |
| `Config.Charger` | Charging speed, price, and whether the charger is used up. |
| `Config.Bubbles` | The speech bubble lines for each flavour, and how often they show. |
| `Config.Shops` | Shop interaction (`ox_target`, `qb-target` or prompt), default shopkeeper, buy limits. |
| `Config.Controls` | The hit and put-away keys, and the `/vape` command. |
| `Config.Commands` | The names of every admin command. |
| `Config.Theme` | The UI colours. Admins can also change these live in `/vapeadmin`. |

Shops, prices, flavour edits and the theme you change in `/vapeadmin` save to `data/store.json` and apply to everyone right away, with no restart.

---

## Troubleshooting

<details>
<summary><b>Using an item does nothing</b></summary>

The items weren't added, or were added to the wrong inventory. Redo [Step 3](#step-3-add-the-items) and restart. On ox_inventory, check that each item has `server = { export = 'g7_vapesystem.g7UseItem' }`.
</details>

<details>
<summary><b>No vape in my hand, or a stock vape shows instead of mine</b></summary>

The `g7devvape` prop resource isn't started, or it starts after `g7_vapesystem`. Fix the order in [Step 2](#step-2-start-order-in-servercfg) and do a full restart.
</details>

<details>
<summary><b>The vape is too big or too small in the hand</b></summary>

Keep the grip `scale` at `1.0` in `Config.Grips`. Any other value makes the vape trail behind the hand when you walk. Resize the model itself in Blender or CodeWalker instead. Admins can line up the position with `/vapegrip`.
</details>

<details>
<summary><b>No smoke when I exhale</b></summary>

Do a full server restart. If the F8 console says `particle asset g7smokecustom did not load`, check that `stream/g7smokecustom.ypt` is in the resource. An empty tank or a dead battery also makes no smoke.
</details>

<details>
<summary><b>Item pictures are blank</b></summary>

The images from `install/images/` weren't copied into your inventory's image folder. See [Step 4](#step-4-add-the-item-images).
</details>

<details>
<summary><b>I can't open /vapeadmin</b></summary>

Add the `add_ace` line from [Step 5](#step-5-admin-permissions), or add your license or Discord ID to `Config.Admin.identifiers`.
</details>

<details>
<summary><b>"You are too far from the counter" in the shop</b></summary>

Stand closer to the shopkeeper. The distance is set by `Config.Shops.distance`.
</details>

---

Need more help? Open a ticket in the **G7 Development** Discord.
