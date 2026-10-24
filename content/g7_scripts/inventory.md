# G7 Inventory — Installation

G7 Inventory is a drop-in replacement for **ox_inventory**. Scripts that use `exports.ox_inventory`, listen to `ox_inventory:*` events or check whether `ox_inventory` is started keep working without changes. Your framework is detected automatically.

**Supported frameworks:** Qbox · ESX Legacy · QBCore · ox_core · ND_Core · Standalone

---

## Requirements

| Resource | Notes |
|---|---|
| [ox_lib](https://github.com/overextended/ox_lib) | Latest release |
| [oxmysql](https://github.com/overextended/oxmysql) | Latest release |
| OneSync | Legacy or Infinity |
| [ox_target](https://github.com/overextended/ox_target) | Optional, only if you want target interactions |

---

## 1. Remove your old inventory

G7 Inventory registers itself as `ox_inventory`, so **it can't run alongside another inventory**.

- **From ox_inventory:** stop it and remove it from your server (or move it out of your resources folder). Your existing items, stashes, trunks and gloveboxes are kept. G7 uses the same database layout, so nothing needs converting.
- **From qb-inventory, ps-inventory or lj-inventory:** remove the inventory resource, then follow [Moving over from another inventory](#5-moving-over-from-another-inventory) after the first start.
- **From the default ESX inventory:** nothing to remove. Follow [Moving over from another inventory](#5-moving-over-from-another-inventory) after the first start.

> **Back up your database before installing.** Any inventory change is worth a backup first.

---

## 2. Add the resource

1. Download `g7_inventory` and extract it into your resources folder.
2. Keep the folder name **`g7_inventory`**. It's used for image paths and exports.

```
resources/
└── [inventory]/
    └── g7_inventory/
        ├── client/
        ├── data/
        ├── locales/
        ├── server/
        ├── shared/
        ├── sql/
        ├── web/
        ├── config.lua
        └── fxmanifest.lua
```

---

## 3. Update server.cfg

Start G7 Inventory **after** oxmysql, ox_lib and your framework, and **before** any script that uses the inventory.

```cfg
ensure oxmysql
ensure ox_lib
ensure qbx_core          # or es_extended / qb-core / ox_core / ND_Core
ensure g7_inventory

# everything that uses the inventory goes below
ensure ox_target
ensure [jobs]
ensure [scripts]
```

> **Remove this line if you have it.** Most servers that ran ox_inventory have:
> ```cfg
> setr inventory:imagepath "nui://ox_inventory/web/images"
> ```
> G7 redirects it automatically, but it's cleaner to delete it or point it at G7:
> ```cfg
> setr inventory:imagepath "nui://g7_inventory/web/images"
> ```

---

## 4. Start the server

On first start, G7 creates the tables and columns it needs:

| Table / column | Used for |
|---|---|
| `inventory` column on your framework's player table | Player inventories |
| `ox_inventory` | Stashes |
| `glovebox` / `trunk` columns on your framework's vehicle table | Vehicle storage |
| `g7_progression` | Levels, XP, skills and daily missions |
| `g7_players` | Player inventories, **standalone servers only** |

If your database user can't create tables, run `sql/install.sql` by hand instead.

When everything is working, the server console shows:

```
[g7_inventory] ready - framework: qbx, 507 items, ox_inventory API v2.48 compatible
```

Check that the framework named there is the one you use. To force a different one, see [Framework setup](#framework-setup).

---

## 5. Moving over from another inventory

Skip this step if you came from **ox_inventory**: your data already works.

Player inventories in the old qb-inventory and ESX formats are **converted automatically** the first time each player joins. The commands below convert everything at once, and they're **required** for qb-inventory stashes and ESX weapon loadouts.

Run the matching command **once**, from the **server console** (txAdmin live console or the server window, not in game), while no players are online:

| Coming from | Command | What it converts |
|---|---|---|
| qb-inventory / ps / lj | `g7convert qb` | All player inventories (`amount` / `info` format) |
| qb-inventory / ps / lj | `g7convert qbstashes` | Stashes, trunks and gloveboxes (**required**) |
| ESX (default inventory) | `g7convert esx` | Player inventories and weapon loadouts (**required** for loadouts) |

Wait for the console to print `conversion finished - restart the server`, then restart.

### Item lists

G7 reads items from `data/items.lua` and weapons from `data/weapons.lua`, both in **ox_inventory format**. You can paste an existing ox `items.lua` straight in.

QBCore and ESX servers can also keep their framework item lists. With this setting on, items from `QBCore.Shared.Items` or the ESX `items` table that aren't in `data/items.lua` are added automatically:

```lua
-- config.lua
Config.ImportFrameworkItems = true
```

> **Items that don't exist are never deleted.** If a player has an item that isn't in your item list, it stays saved and the console shows `unknown item "name" ... kept in storage`. Add the item to `data/items.lua` and it appears again for the player.

---

## Framework setup

The framework is detected in this order: **Qbox → ESX → QBCore → ox_core → ND_Core → Standalone**.

To force one, set it in `server.cfg`:

```cfg
setr inventory:framework "qbx"     # qbx | esx | qb | ox | nd | standalone
```

### Qbox
Works out of the box. Money is kept in sync with Qbox's cash account.

> Don't add `provide 'qb-inventory'` on Qbox. Qbox refuses to save players while it thinks qb-inventory is running.

### ESX Legacy
Works out of the box. G7 uses ESX's built-in ox_inventory support, so nothing needs changing in ESX. `money` and `black_money` are handled as account items.

### QBCore
Works out of the box. QBCore item functions (`Player.Functions.AddItem`, `RemoveItem`, `HasItem`…) and the `qb-inventory`, `ps-inventory` and `lj-inventory` exports are bridged to G7.

If a script refuses to work because it checks whether `qb-inventory` is **started**, open `fxmanifest.lua` and uncomment:

```lua
-- provide 'qb-inventory'
```

**QBCore only.** Never do this on Qbox.

### ox_core / ND_Core
Supported through their existing ox_inventory integration. No extra steps.

### Standalone
Players are identified by their FiveM license and stored in the `g7_players` table. Permission groups come from ACE:

```cfg
add_ace group.admin g7.group.admin allow
add_ace group.police g7.group.police allow
```

---

## Item images

Item images go in `web/images/` as **`<item name>.png`**, for example `water.png` or `WEAPON_PISTOL.png`.

- Use lowercase `.png` extensions. Linux servers treat `.PNG` and `.png` as different files.
- An item can use a different image with `client = { image = 'other_name.png' }` in its definition.
- To host images on a website instead:
  ```cfg
  setr inventory:imagepath "https://cdn.example.com/items"
  ```
  If an image can't be loaded from there, G7 falls back to its own `web/images` folder.

---

## Keybinds

| Default key | Action |
|---|---|
| `F2` | Open inventory (also opens a nearby drop, or the trunk when you're behind an unlocked vehicle) |
| `K` | Open trunk, glovebox, dumpster or stash |
| `TAB` | Show the hotbar |
| `1` – `5` | Use fast slots |
| `Z` (hold) | Quick wheel |
| `R` | Reload |

Players can rebind these in **Settings → Key Bindings → FiveM**. To change the defaults for everyone:

```cfg
setr inventory:keys ["F2", "K", "TAB"]    # inventory, secondary, hotbar
```

---

## Common server.cfg options

All ox_inventory `inventory:*` convars work. The most used ones:

```cfg
setr inventory:slots 50               # player slots
setr inventory:weight 30000           # player max weight, in grams
setr inventory:target false           # true = use ox_target for stashes, shops, benches
setr inventory:police ["police", "sheriff"]
setr inventory:screenblur true
setr inventory:weaponanims true
setr inventory:droplifetime 15        # minutes before dropped bags disappear (0 = never)
set  inventory:webhook "https://discord.com/api/webhooks/..."   # logs
set  inventory:loglevel 1             # 0 off, 1 important, 2 everything
```

### Player card photos (optional)

The player card can show a Discord or Steam profile picture. Put the keys in `server.cfg` with **`set`**, never `setr` (which would send them to every player) and never in a Lua file:

```cfg
set g7:discordToken "your-bot-token"
set g7:steamApiKey  "your-steam-web-api-key"
```

---

## Configuration

Everything else lives in **`config.lua`**: themes, features, shops, drops, backpacks, weapons on back, encumbrance, progression and more. Any feature can be switched off under `Config.Features`.

Data files in `data/` use the same format as ox_inventory:

| File | Contains |
|---|---|
| `items.lua` | Items |
| `weapons.lua` | Weapons, attachments, tints and ammo |
| `shops.lua` | Shops and vending machines |
| `stashes.lua` | Stashes and lockers |
| `crafting.lua` | Crafting benches and recipes |
| `evidence.lua` | Police evidence lockers |
| `licenses.lua` | Purchasable licences |
| `vehicles.lua` | Trunk and glovebox sizes |
| `animations.lua` | Item use animations |
| `skills.lua` / `missions.lua` | Progression skills and daily missions |

Pasted ox data files that use ox's `shared` table (for example `groups = shared.police`) work without editing.

---

## Check your installation

1. Join the server and press **F2**. Your inventory opens with your items.
2. Run `/giveitem <your id> water 5` and check the water appears.
3. Drop an item, then press **F2** next to the bag to pick it up.
4. Open a vehicle trunk with **K** while standing behind it.
5. Run `/saveinv`. The console prints `saved N inventories`.

### Admin commands

Admin commands need `group.admin`. `/trade` is for every player, and `/clearevidence` is for police bosses.

| Command | What it does |
|---|---|
| `/giveitem [id] [item] [count]` | Give an item (`/additem` works too) |
| `/removeitem [id] [item] [count]` | Remove an item |
| `/setitem [id] [item] [count]` | Set the exact amount of an item |
| `/viewinv [id]` | Look at a player's inventory |
| `/clearinv [id]` | Wipe a player's inventory |
| `/takeinv [id]` | Confiscate a player's inventory |
| `/restoreinv [id]` | Give a confiscated inventory back |
| `/saveinv` | Save all inventories now |
| `/addxp [id] [amount]` | Give progression XP |
| `/g7items` | Show how many items are loaded |
| `/trade [id]` | Ask a nearby player to trade (all players) |
| `/clearevidence [locker]` | Empty an evidence locker (police bosses) |

---

## Troubleshooting

**Images are missing or faded**
Check `server.cfg` for an `inventory:imagepath` line pointing at another resource, and remove it. Make sure each image is named exactly like the item, with a lowercase `.png` extension.

**`attempt to index a nil value (global 'shared')` on startup**
You're running a version older than 1.1.0. Update G7 Inventory.

**The console shows a different framework than yours**
Make sure your framework starts **before** `g7_inventory` in `server.cfg`, or force it with `setr inventory:framework`.

**`could not read inventory of ...` in the console**
G7 couldn't reach your database when the player joined. It retries 3 times. If that fails, the inventory stays read-only for that session so nothing gets overwritten. Check that oxmysql is connected, then ask the player to reconnect.

**A QBCore script says the inventory isn't started**
Uncomment `provide 'qb-inventory'` in `fxmanifest.lua` (QBCore only).

**Another inventory is still running**
Only one inventory can run. Remove ox_inventory, qb-inventory, ps-inventory or lj-inventory from your resources and `server.cfg`.

---

## Updating

1. Back up `config.lua` and anything you changed in `data/`.
2. Replace the whole `g7_inventory` folder, so the built UI files in `web/build` are replaced too.
3. Put your `config.lua` and `data/` files back, then compare them with the new versions for any new settings. The changelog lists them.
4. Restart the server.
