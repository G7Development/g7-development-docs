# G7 Black Market — Installation

A contract, reputation and black market system with crews, heists, laundering, market events and weekly prizes. It works on its own or together with **G7 Black Market Phone**.

---

## Requirements

| Resource | Required | Notes |
|---|---|---|
| [oxmysql](https://github.com/overextended/oxmysql) | **Yes** | Database |
| A framework | **Yes** | **QBCore**, **Qbox** or **ESX** (detected automatically). Custom frameworks are supported, see [Custom framework](#custom-framework) |
| An inventory | **Yes** | **g7_inventory** (recommended), ox_inventory, qs-inventory, qb-inventory, ps-inventory, codem-inventory, core_inventory or your framework's built-in inventory |
| [ox_lib](https://github.com/overextended/ox_lib) | Recommended | Notifications, progress bars and the heist hacking minigame. Falls back to framework or native versions without it |
| ox_target / qb-target | Optional | Used for the mission givers. Without a target script, players get `[E]` prompts |
| A vehicle keys script | Optional | qbx_vehiclekeys, qb-vehiclekeys, Renewed-Vehiclekeys, wasabi_carlock or MrNewbVehicleKeys, for contract cars and purchased cars |

---

## 1. Add the resource

1. Download `g7_blackmarket` from your Keymaster / Tebex account.
2. Extract it into your `resources` folder. Keep the folder name **`g7_blackmarket`**.

---

## 2. server.cfg

Start it **after** your database, framework and inventory, and **before** `g7_blackmarket_phone` if you use the phone:

```cfg
ensure oxmysql
ensure ox_lib            # recommended
ensure qbx_core          # or qb-core / es_extended
ensure g7_inventory      # or your inventory
ensure ox_target         # optional

ensure g7_blackmarket
ensure g7_blackmarket_phone   # optional
```

---

## 3. Database

**Nothing to do.** All tables are created automatically on the first start:

`g7_blackmarket_players` · `g7_blackmarket_history` · `g7_blackmarket_crews` · `g7_blackmarket_crew_members` · `g7_blackmarket_launder` · `g7_blackmarket_serials` · `g7_blackmarket_pending`

---

## 4. Items & images

Everything you need is in the **`install/`** folder. Use the file for your inventory and **skip any items you already have**.

| Inventory | Item file | Copy images from → to |
|---|---|---|
| **g7_inventory** | `install/ox_inventory_g7_inventory_qbox.lua` → `g7_inventory/data/items.lua` | `install/images/ox/` → `g7_inventory/web/images/` |
| **ox_inventory / Qbox** | `install/ox_inventory_g7_inventory_qbox.lua` → `ox_inventory/data/items.lua` | `install/images/ox/` → `ox_inventory/web/images/` |
| **QBCore** (qb / ps / lj / codem / core) | `install/qb-core_shared_items.lua` → `qb-core/shared/items.lua` | `install/images/qb/` → your inventory's image folder |
| **qs-inventory** | `install/qs-inventory_items.lua` → `qs-inventory/shared/items.lua` | `install/images/qb/` → `qs-inventory/html/images/` |
| **ESX** (default inventory) | Run `install/esx_items.sql` on your database | `install/images/qb/` → your inventory's image folder |

> **Item names are translated automatically.** The config uses ox / g7 names (`WEAPON_PISTOL`, `ammo-9`, `armour`). On QB-style inventories the script switches them to the QB names (`weapon_pistol`, `pistol_ammo`, `armor`), so you don't need to edit the config. Add your own renames in `Config.ItemAliases`, for example `['black_money'] = 'markedbills'`.

> **Pictures always show.** Every image is also bundled in `html/images/`, so the market shows pictures even before you copy them into your inventory.

### Dirty money

| Setup | Dirty money is |
|---|---|
| g7_inventory or ox_inventory (any framework) | the `black_money` **item** |
| QBCore / Qbox with another inventory | the `black_money` **item** (change it in `Config.Accounts.dirty`) |
| ESX default inventory | the `black_money` **account** |

---

## 5. Permissions

Admin commands accept any of these:

```cfg
# ACE (any framework)
add_ace group.admin g7.admin allow
```

- QBCore `admin` / `god`
- ESX `admin` / `superadmin`

| Command | Who | What it does |
|---|---|---|
| `/bmsetrep [id] [amount]` | Admin / console | Sets a player's reputation (this also sets their rank and tier) |
| `/bmevent sale\|crackdown\|rep\|rare` | Admin / console | Starts a market event right away |
| `/bmtrace [serial]` | Police jobs from `Config.Serials.policeJobs` | Traces a weapon serial. With no serial, it traces the weapon you're holding (ox / g7 only) |
| `/bmabandon` | Players | Abandons the active contract |

---

## 6. Configure

Open `config.lua`. Settings to check first:

| Setting | Default | What it does |
|---|---|---|
| `Config.Framework` | `'auto'` | `'qbox'`, `'qb'`, `'esx'` or `'custom'` |
| `Config.Inventory` | `'auto'` | Checks **g7_inventory** first |
| `Config.Target` | `'auto'` | `'ox_target'`, `'qb-target'` or `'none'` |
| `Config.Theme.preset` | `'frost'` | UI theme: `frost` (blue), `g7` (purple), `toxic`, `ember`, `gold`, `mono` or `custom`. Uses the same engine as the other G7 scripts |
| `Config.BlacklistedJobs` | police jobs | Jobs that can't use the black market |
| `Config.Givers` | 3 givers | Mission giver peds, blips and locations |
| `Config.Ranks` | 8 ranks | Rep needed for each rank and the tier it unlocks |
| `Config.Missions` | 12 contracts | Contracts, crew contracts and heists |
| `Config.ShopItems` | weapons, ammo, tools, contraband, vehicles | Market stock, prices and tiers |
| `Config.Crews` | enabled | Crew cost, size, rep share and levels |
| `Config.Laundering` | enabled | Rates, daily limits and seizure chance |
| `Config.MarketEvents` | enabled | Random sales, crackdowns, double rep and rare shipments |
| `Config.WeeklyPrizes` | Monday 18:00 | Weekly leaderboard prizes (server time) |
| `Config.Serials` | enabled | Weapon serials and police tracing |
| `Config.Logs` | disabled | Discord webhook logging |

> **Test the coordinates on your map.** Heist sites (`Config.HeistLocations`) and mission locations use standard GTA locations. If you run MLOs that cover any of them, move those spots.

---

## 7. Test it

1. Restart the server, or run `ensure g7_blackmarket`.
2. The console should show:
   ```
   [G7] g7_blackmarket framework: qbox
   [G7] g7_blackmarket inventory: g7_inventory
   ```
3. Go to **Vince** at the Terminal docks (blip on the map) and talk to him.
4. To unlock everything, run `/bmsetrep [your id] 18000` (Kingpin, tier 5).

---

## Custom framework

Set `Config.Framework = 'custom'` and fill in the `Custom` table at the top of `bridge/server.lua`. It takes identifier, name, job, money get/add/remove and admin check functions.

---

## Exports

```lua
-- server
exports.g7_blackmarket:GetReputation(src)
exports.g7_blackmarket:GetRank(src)                       -- { index, name, tier, progress, next, nextRep }
exports.g7_blackmarket:AddReputation(src, amount, reason) -- trusted resources only
exports.g7_blackmarket:OpenRemote(src, tab)               -- trusted resources only
exports.g7_blackmarket:GetCrew(src)                       -- { id, name, tag, rep } or nil
exports.g7_blackmarket:GetActiveEvent()
exports.g7_blackmarket:GetPriceModifier(item, category)
exports.g7_blackmarket:GiveTrackedItem(src, item, qty, source)
exports.g7_blackmarket:TraceSerial(serial)                -- for MDTs
```

Trusted resources are listed in `Config.RemoteOpenResources`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Market pictures don't show | Copy the images (step 4), or check that `inventory:imagepath` points to your images |
| "Not enough money" with dirty money | Make sure the player has the `black_money` item (see Dirty money) |
| Contract car won't start | Make sure your keys script is running. `Config.MissionVehicles.giveKeys = true` gives keys on entry |
| Can't find a stash spot | Spots show as small blips. If one landed somewhere unreachable, `/bmabandon` and take the contract again |
| Crew contract says you need more members | Crew members must be online and within `Config.Crews.joinRange` (100 m) of you |
| Heist hack is just a progress bar | Install ox_lib for the skill-check minigame |
| Nobody gets paid weekly | Players need at least `Config.WeeklyPrizes.minRep` rep that week. Offline winners are paid the next time they open the menu |

Need help? Join the **[G7 Development Discord](https://discord.gg/P6XQ4N6M95)**.
