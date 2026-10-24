# G7 Banking — Installation

This guide walks you through installing **G7 Banking** on your FiveM server. It takes about 10 minutes.

---

## Requirements

| Resource | Required | Notes |
|---|---|---|
| [oxmysql](https://github.com/overextended/oxmysql) | ✅ Yes | Database connector |
| Framework | ✅ Yes | ESX, QBCore or QBox (auto-detected). ox_core and standalone are also supported |
| Inventory | ✅ Yes | ox_inventory, qb-inventory or g7-inventory |
| ox_target / qb-target | Optional | Without either, the script uses its own built-in **[E]** prompt |
| lb-phone | Optional | If running, a **G7 Bank** app is added to it automatically |

> **OneSync** must be enabled (it is on by default for most servers).

---

## Step 1 — Add the resource

1. Download **g7_banking** from your Keymaster / Tebex account.
2. Extract it into your resources folder, for example:

```
resources/[g7]/g7_banking
```

> ⚠️ Do **not** rename the folder. It must be called `g7_banking`.

---

## Step 2 — server.cfg

Start G7 Banking **after** oxmysql, your framework and your inventory:

```cfg
ensure oxmysql
ensure qb-core          # or es_extended / qbx_core
ensure ox_inventory     # or qb-inventory / g7-inventory
ensure ox_target        # optional
ensure g7_banking
```

### Admin permissions

```cfg
# Access to /bankadmin (branches, ATMs, players, credit, market, outages)
add_ace group.admin g7banking.admin allow

# Lets in-game admins use the g7outage command (the server console can always use it)
add_ace group.admin command.g7outage allow
```

> Players in the ESX / QBCore groups `admin`, `god` or `superadmin` can also open `/bankadmin`. You can change this in `Config.Admin`.

---

## Step 3 — Database

**Nothing to do.** All tables are created automatically on first start.

If you prefer to import manually, run `sql/install.sql` in your database before starting the resource.

---

## Step 4 — Card items

Players carry a physical bank card to use ATMs. Add the items to your inventory.

### ox_inventory

Add to `ox_inventory/data/items.lua`:

```lua
['g7_card'] = { label = 'G7 Card', weight = 5, stack = false, close = true, description = 'A G7 Bank payment card' },
['g7_card_standard'] = { label = 'G7 Standard Card', weight = 5, stack = false },
['g7_card_silver']   = { label = 'G7 Silver Card',   weight = 5, stack = false },
['g7_card_gold']     = { label = 'G7 Gold Card',     weight = 5, stack = false },
['g7_card_black']    = { label = 'G7 Black Card',    weight = 5, stack = false },
```

### qb-inventory

Add to `qb-core/shared/items.lua`:

```lua
g7_card          = { name = 'g7_card',          label = 'G7 Card',          weight = 5, type = 'item', image = 'g7_card.png',          unique = true, useable = false, shouldClose = true, description = 'A G7 Bank payment card' },
g7_card_standard = { name = 'g7_card_standard', label = 'G7 Standard Card', weight = 5, type = 'item', image = 'g7_card_standard.png', unique = true, useable = false, shouldClose = true, description = 'G7 debit card' },
g7_card_silver   = { name = 'g7_card_silver',   label = 'G7 Silver Card',   weight = 5, type = 'item', image = 'g7_card_silver.png',   unique = true, useable = false, shouldClose = true, description = 'G7 debit card' },
g7_card_gold     = { name = 'g7_card_gold',     label = 'G7 Gold Card',     weight = 5, type = 'item', image = 'g7_card_gold.png',     unique = true, useable = false, shouldClose = true, description = 'G7 debit card' },
g7_card_black    = { name = 'g7_card_black',    label = 'G7 Black Card',    weight = 5, type = 'item', image = 'g7_card_black.png',    unique = true, useable = false, shouldClose = true, description = 'G7 debit card' },
```

### g7-inventory

Add the same five items (`g7_card`, `g7_card_standard`, `g7_card_silver`, `g7_card_gold`, `g7_card_black`) to your g7-inventory item list, using the matching image names from Step 5. Set them as **unique / non-stackable**.

> Only `g7_card` is used in normal play. The card's number, owner and design are stored on the item. The four tier items are a fallback and are safe to add.

---

## Step 5 — Item images

Copy **every** PNG from:

```
g7_banking/images/inventory/
```

into your inventory's image folder:

| Inventory | Image folder |
|---|---|
| ox_inventory | `ox_inventory/web/images/` |
| qb-inventory | `qb-inventory/html/images/` |
| g7-inventory | g7-inventory's item image folder |

> Copy all of them. Each credit card and tier has its own design, and ox_inventory picks the right image automatically.

---

## Step 6 — Start the server

Restart your server (or run `ensure g7_banking`). In the server console you should see:

```
[g7_banking] framework: qb  inventory: ox_inventory
[g7_banking] database ready
[g7_banking] business accounts: internal
```

On first start, **8 bank branches** are created around the map with tellers and blips. You can move, rename or delete them in-game with `/bankadmin`.

---

## Step 7 — First-time setup in game

### Branches & ATMs
Open `/bankadmin` → **Branches** to add, move or remove branches. **Place in world** lets you position the teller with your camera:

| Key | Action |
|---|---|
| Mouse | Aim the placement |
| Scroll / Arrow keys | Rotate |
| Shift | Snap to your feet |
| E | Confirm |
| Backspace | Cancel |

All GTA map ATMs work automatically. Use **ATMs** in `/bankadmin` to place extra ones.

### Tune the ATM screen (recommended)
The bank UI appears on the ATM's real screen. To line it up perfectly:

1. Stand at an ATM and type `/atmcam`.
2. Drag the sliders until the outline fits the screen.
3. Press **Copy config** and paste it into `Config.ATM.models` in `config.lua`.

Repeat once for each ATM model on your map. If you'd rather skip this, set `Config.ATM.screenMode = 'framed'`.

---

## Supported inventories

| Inventory | Supported |
|---|---|
| ox_inventory | ✅ |
| qb-inventory | ✅ |
| g7-inventory | ✅ |

---

## Commands

| Command | Who | Description |
|---|---|---|
| `/bank` | Everyone | Opens the bank (near a branch only, unless `commandAnywhere = true`) |
| `/bankphone` | Everyone | Opens the G7 phone banking app |
| `/bankadmin` | Admins | Admin panel |
| `/atmcam` | Everyone | ATM screen alignment tool (see Step 7) |
| `g7outage breach [min]` | Admins / console | Starts a data breach |
| `g7outage maint [min]` | Admins / console | Starts maintenance |
| `g7outage stop` | Admins / console | Ends any outage |

---

## Quick config

Everything lives in `config.lua`. The most common changes:

```lua
Config.BankName = 'G7 Bank'

Config.UI = {
    accent  = '#5B376A',   -- main colour, re-themes the whole UI
    accent2 = '#9D71B4',
    logo    = 'logo.png',  -- web/dist/logo.png
}

Config.Open = {
    command = 'bank',
    commandAnywhere = false,  -- true = /bank works anywhere
}

Config.PhysicalCards = {
    enabled = true,
    requireForATM = true,     -- false = ATMs work without carrying a card
    reissueFee = 250,
}
```

> **Your own logo:** replace `web/dist/logo.png` (and `logo-dark.png`, used on silver/gold/platinum cards).

---

## Troubleshooting

**The whole screen is black when the resource is running**
Clear your FiveM cache (`%localappdata%/FiveM/FiveM.app/data/cache`) and make sure you are on the latest version of G7 Banking.

**"You need your G7 card to use this ATM"**
The player has no card item. Cards are given when a player prints one at a branch teller (**Overview → Teller desk → Print card**), upgrades their debit tier, or is approved for a credit card. To let ATMs work without a card, set `Config.PhysicalCards.requireForATM = false`.

**Card items have no picture**
The PNGs from `images/inventory/` were not copied into your inventory's image folder (Step 5).

**/bank says I need to be at a branch**
This is intended. Visit a branch, or set `Config.Open.commandAnywhere = true`.

**/bankadmin says no permission**
Add the `add_ace` line from Step 2 and make sure your identifier is in `group.admin`.

**Branches don't appear**
Check the server console for `database ready`. If it's missing, oxmysql is not started before g7_banking.

**The ATM screen is misaligned**
Run `/atmcam` at that ATM (Step 7).

---

## Updating

1. Back up your `config.lua`.
2. Replace the `g7_banking` folder with the new version.
3. Put your `config.lua` back (check the changelog for any new options).
4. Restart the resource. Database changes are applied automatically.
