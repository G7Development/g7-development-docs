# G7 Admin Pockets — Installation

**G7 Admin Pockets** lets admins give players more carry weight in-game. Everything is saved to the database and can be revoked at any time. Follow the steps below to get it running in a few minutes.

---

## Requirements

| Resource | Required | Notes |
|---|---|---|
| [oxmysql](https://github.com/overextended/oxmysql) | ✅ Yes | Database |
| Framework | ✅ Yes | QBCore, Qbox or ESX (auto-detected) |
| Inventory | ✅ Yes | g7_inventory (default), ox_inventory or qs-inventory |
| [ox_lib](https://github.com/overextended/ox_lib) | Optional | Only needed if `Config.Notify = 'ox_lib'` (the default) |

---

## Step 1: Add the resource

1. Download `g7_adminpockets` after purchasing it on [Tebex](https://g7-development.tebex.io/).
2. Unzip it and place the `g7_adminpockets` folder in your `resources` folder.

> ⚠️ Do not rename the folder. It must stay `g7_adminpockets`.

---

## Step 2: server.cfg

Start the script **after** oxmysql, your framework and your inventory:

```cfg
ensure oxmysql
ensure ox_lib
ensure qbx_core          # or qb-core / es_extended
ensure g7_inventory      # or ox_inventory / qs-inventory

ensure g7_adminpockets
```

---

## Step 3: Permissions

Add these lines to your `server.cfg` so your staff can open the panel:

```cfg
# Can open /adminpocket and give / revoke pocket space
add_ace group.admin g7_adminpockets.admin allow

# Can also manage the in-game admin whitelist
add_ace group.god g7_adminpockets.owner allow
```

A player can open the panel if **any** of these are true:

- They have the `g7_adminpockets.admin` ACE.
- Their framework group is listed in `Config.AdminGroups` (`god`, `admin`, `superadmin` by default).
- An owner added them in the **Admins** tab of the panel.

---

## Step 4: Database

Nothing to do. The tables are created automatically the first time the script starts.

If you prefer to run it yourself, import `sql/install.sql` into your database. It creates:

| Table | Stores |
|---|---|
| `g7_adminpockets_grants` | Every pocket grant (active and revoked) |
| `g7_adminpockets_admins` | The in-game admin whitelist |
| `g7_adminpockets_logs` | The audit log |

---

## Step 5: Choose your inventory

Open `config.lua` and set your inventory. **g7_inventory is the default**, so if you use it you can skip this step.

```lua
Config.Inventory = 'g7_inventory' -- 'g7_inventory' | 'ox_inventory' | 'qs-inventory' | 'auto'
```

Then set the base carry weight (in kg) to match your inventory's normal player weight:

```lua
Config.BaseWeightKg = {
    ['g7_inventory'] = 30.0,
    ['ox_inventory'] = nil,   -- nil = read from the inventory:weight convar
    ['qs-inventory'] = 120.0,
}
```

> 💡 Bonuses are added **on top of** this number, so make sure it matches your inventory or players will end up with the wrong total.

---

## Step 6: Add the pocket items

Pocket items raise a player's carry weight just by holding them. Add these items to your inventory, then copy the item images into your inventory's image folder.

**g7_inventory / ox_inventory** — `data/items.lua`

```lua
['small_pouch'] = {
    label = 'Small Pouch',
    weight = 250,
    stack = false,
    close = true,
    description = 'A small pouch. +10kg carry weight while held.',
},
['medium_bag'] = {
    label = 'Medium Bag',
    weight = 1000,
    stack = false,
    close = true,
    description = 'A medium bag. +25kg carry weight while held.',
},
['large_backpack'] = {
    label = 'Large Backpack',
    weight = 2000,
    stack = false,
    close = true,
    description = 'A large backpack. +50kg carry weight while held.',
},
```

**qs-inventory** — `shared/items.lua`

```lua
['small_pouch'] = {
    name = 'small_pouch', label = 'Small Pouch', weight = 250, type = 'item',
    image = 'small_pouch.png', unique = true, useable = false, shouldClose = true,
    description = 'A small pouch. +10kg carry weight while held.',
},
['medium_bag'] = {
    name = 'medium_bag', label = 'Medium Bag', weight = 1000, type = 'item',
    image = 'medium_bag.png', unique = true, useable = false, shouldClose = true,
    description = 'A medium bag. +25kg carry weight while held.',
},
['large_backpack'] = {
    name = 'large_backpack', label = 'Large Backpack', weight = 2000, type = 'item',
    image = 'large_backpack.png', unique = true, useable = false, shouldClose = true,
    description = 'A large backpack. +50kg carry weight while held.',
},
```

| Inventory | Image folder |
|---|---|
| g7_inventory | `g7_inventory/web/images/` |
| ox_inventory | `ox_inventory/web/images/` |
| qs-inventory | `qs-inventory/html/images/` |

> 💡 Don't want pocket items? Set `Config.PocketItems.enabled = false` and skip this step.

---

## Step 7: Notifications (optional)

Pick how players are notified when they get or lose pocket space:

```lua
Config.Notify = 'ox_lib' -- 'ox_lib' | 'nui' | 'qb' | 'esx' | 'custom'
```

| Option | Uses |
|---|---|
| `ox_lib` | ox_lib notifications (default) |
| `nui` | Built-in G7 toasts that match the panel |
| `qb` | QBCore notify |
| `esx` | ESX notify |
| `custom` | Fires `Config.CustomNotifyEvent` so you can hook your own |

---

## Step 8: Start and test

1. Restart your server (or run `ensure g7_adminpockets`).
2. Check your server console for these three lines:

```
[g7_adminpockets] Framework detected: qbx
[g7_adminpockets] Inventory linked: g7_inventory
[g7_adminpockets] Ready — by G7 Development
```

3. Join the server and type **`/adminpocket`**. The panel should open.

✅ **You're done!**

---

## Commands

| Command | Who | What it does |
|---|---|---|
| `/adminpocket` | Admins | Opens the admin panel |
| `/givepocket [id] [kg] [hours] [reason]` | Admins / console | Quick grant (leave hours empty or 0 for permanent) |
| `/revokepocket [grantId]` | Admins / console | Removes one grant |
| `/mypocket` | Everyone | Shows your own carry weight breakdown |

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `No supported framework found` in console | Make sure your framework starts **before** `g7_adminpockets` in `server.cfg`. |
| `Inventory "..." is not started` in console | Check `Config.Inventory` matches your inventory and that it starts first. |
| `/adminpocket` says no permission | Add the ACE line from Step 3, or check your group is in `Config.AdminGroups`. |
| Total weight looks wrong | Set `Config.BaseWeightKg` to your inventory's real default player weight. |
| Pocket items don't add weight | Make sure the item names in your inventory match `Config.PocketItems.tiers` exactly. |
| Panel has no logo | Check `html/g7.png` exists and `Config.Brand.logo` is set to `g7.png`. |

Still stuck? Open a ticket in our [Discord](https://discord.gg/P6XQ4N6M95) and we'll help you out.
