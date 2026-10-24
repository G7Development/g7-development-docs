# G7 Illegal Weapon — Installation

**g7_illegalweapon** by G7 Development. Placeable laser printers, a hacker laptop OS, a dark web shop, ATM wire payments and courier deliveries.

Setup takes about ten minutes. No SQL import is needed.

---

## 1. Requirements

| Requirement | Notes |
|---|---|
| FiveM server | A recent artifact with **OneSync** enabled. |
| [ox_lib](https://github.com/overextended/ox_lib) | **Required.** Used for callbacks, progress bars and skill checks. |
| A framework | `es_extended` (ESX), `qb-core` (QBCore) or `qbx_core` (Qbox). Auto-detected. |
| An inventory | `ox_inventory`, `qb-inventory` (also `ps-` / `lj-inventory`) or `qs-inventory`. Auto-detected. A custom inventory can be added (see [Custom inventory](#custom-inventory)). |
| A target system *(optional)* | `ox_target`, `qb-target` or `qtarget`. Without one, the script falls back to on-screen prompts (**E** and **G**). |
| A dispatch script *(optional)* | `ps-dispatch`, `cd_dispatch`, `qs-dispatch` or `core_dispatch`. Auto-detected. |

> **Note**
> `oxmysql` is not required. Printers and progression are stored in the resource's own key-value store and survive restarts.

---

## 2. Install the resource

1. Download and extract the resource.
2. Put the folder in your `resources` directory. **The folder name must be exactly `g7_illegalweapon`.**

```
resources/
└── [g7]/
    └── g7_illegalweapon/
        ├── fxmanifest.lua
        ├── config/
        ├── client/ server/ bridge/ shared/ locales/
        ├── web/
        ├── stream/
        └── install/
```

---

## 3. Add your props

The script spawns two custom props. Both are optional, and each has a fallback model if yours is missing or fails to stream.

| Prop | Model name | Config key |
|---|---|---|
| Laser printer | `g7devlaserprinter` | `Config.Printer.model` |
| Hacker computer *(optional)* | `g7devhackerpc` | `Config.Computer.model` |

1. Put your `.ydr`, `.ytd` and **`.ytyp`** files in `g7_illegalweapon/stream/`.
2. The `.ytyp` is required. A `.ydr` on its own will not create an object in game.
3. The archetype name in the `.ytyp` must match the model name above, or you must change the config to match.
4. If the model sits too high or low on surfaces, adjust `Config.Printer.zOffset`.

> **Note**
> If the custom model cannot load, the script uses `fallbackModel` and prints a debug line (turn on `Config.Debug`).

---

## 4. Register the items

Pick the file for your inventory from `install/` and paste its contents into your inventory's item list.

| Inventory | File | Paste into |
|---|---|---|
| ox_inventory | `install/ox_inventory_items.lua` | `ox_inventory/data/items.lua` |
| ox_inventory (weapons) | `install/ox_inventory_weapons.lua` | `ox_inventory/data/weapons.lua` |
| qb-inventory / ps / lj | `install/qb_items.lua` | `qb-core/shared/items.lua` |
| qs-inventory | `install/qs_items.lua` | `qs-inventory/shared/items.lua` |

### Item images

Copy everything in `install/images/` to your inventory's image folder.

| Inventory | Image folder |
|---|---|
| ox_inventory | `ox_inventory/web/images/` |
| qb-inventory | `qb-inventory/html/images/` |
| qs-inventory | `qs-inventory/html/images/` |

### Item list

| Item | Purpose |
|---|---|
| `g7_printer` | Deployable printer. |
| `g7_hacklaptop` | Hand-held laptop. Opens the hacker OS. |
| `g7_hackerpc` | Deployable workstation. Opens the same OS. |
| `g7_filament`, `g7_filament_cf` | Standard and carbon filament spools. |
| `g7_metal_scrap` | Print material. |
| `g7_nozzle`, `g7_printer_repair` | Printer maintenance. |
| `g7_vpn_dongle` | One-use VPN token. |
| `g7_blueprint_pistol`, `_compact`, `_smg` | Required in your inventory to print that weapon. |
| `g7_part_slide`, `_barrel`, `_trigger`, `_spring`, `_magazine`, `_frame` | Assembly parts. |
| `g7_ghost_frame_pistol`, `_compact`, `_smg` | Printed frames. Use one in your inventory to start assembly. |
| `WEAPON_GHOSTPISTOL`, `WEAPON_GHOSTCOMPACT`, `WEAPON_GHOSTSMG` | Placeholder weapons. Replace with yours (next section). |

> **Note**
> Usable items need no extra setup. The script registers them through your framework. On ox_inventory they use `consume = 0`, so the script, not the inventory, removes items.

---

## 5. Add your own weapons

Weapons are defined in `config/recipes.lua`. No other file needs editing.

1. Stream your weapon as usual (weapon metas, `data_file` entries, models).
2. Register the weapon in your inventory. On ox_inventory, add it to `weapons.lua`.
3. In `config/recipes.lua`, set `assembly.result.item` to your weapon name:

```lua
assembly = {
    result = { item = 'WEAPON_YOURWEAPON', count = 1, ammo = 0 },
    ...
}
```

To add a new recipe, copy an existing block and change `id`, `label`, `blueprint`, `materials`, `output` and `assembly`. If you add a new blueprint, printed frame or part, register it in your inventory (section 4) and add an image.

> **Note**
> Use uppercase weapon names in the config (`WEAPON_X`). They are converted to lowercase automatically for qb-inventory and qs-inventory.

---

## 6. Start the resource

Add this to `server.cfg`, **after** ox_lib, your framework, your inventory and your target script:

```cfg
ensure ox_lib
ensure es_extended        # or qb-core / qbx_core
ensure ox_inventory       # or qb-inventory / qs-inventory
ensure ox_target          # optional
ensure g7_illegalweapon

# Admin commands
add_ace group.admin g7illegal.admin allow
```

Restart the server. The console should print the detected framework and inventory:

```
[g7_illegalweapon] framework: qbox
[g7_illegalweapon] inventory: ox
```

---

## 7. Configure

All settings live in `config/`:

| File | Controls |
|---|---|
| `main.lua` | Framework, inventory, target, notify and dispatch overrides. Logo, accent color, money accounts, police jobs, progression, webhook logs. |
| `printer.lua` | Printer, computer and laptop props. Placement rules, limits, filament, wear and jams, tracker, VPN, police heat, ATM. |
| `recipes.lua` | Weapon recipes and assembly. |
| `shop.lua` | Dark web products, prices, stock and order expiry. |
| `delivery.lua` | Delivery tiers, courier appearance, claim window and meet points. |

### Logo and color

In `config/main.lua`:

```lua
Config.Theme = {
    brand  = 'G7 Development',
    logo   = 'img/logo.svg',   -- file inside web/img/
    accent = '#7C5CFF',        -- one hex value; the whole UI follows it
}
```

Replace `web/img/logo.svg` with your logo, or add a new file to `web/img/` and change `logo`. Square images work best.

### Delivery locations

The meet points in `config/delivery.lua` are examples. **Check each one in game** and replace them with your own:

```lua
{ label = 'Elysian Docks', coords = vec4(1242.3, -3257.5, 5.8, 270.0) },
```

### Language

Strings live in `locales/en.lua`. Copy it to `locales/xx.lua`, rename `Locales.en` to `Locales.xx`, translate it, and set `Config.Locale = 'xx'`.

---

## 8. Test checklist

Give yourself the items from the console, then check each step:

1. Use `g7_printer`. Place it, then pick it up again.
2. Give yourself `g7_filament`, `g7_metal_scrap` and `g7_blueprint_pistol`. Open the printer, load a spool and start a print.
3. Wait for the print, then collect the frame.
4. Give yourself the parts for the recipe, use the frame, and pass the skill check.
5. Use `g7_hacklaptop`. Add items to the cart and check out.
6. Walk to an ATM, choose **Wire Transfer**, and enter the account number from the laptop.
7. Wait for the courier and take the package.

---

## 9. Admin commands

Require the `g7illegal.admin` ACE (see section 6). They also work from the server console.

| Command | Effect |
|---|---|
| `/g7props` | List every deployed printer and computer. |
| `/g7removeprop <id>` | Delete one by id. |
| `/g7setxp <serverId> <xp>` | Set a player's progression XP. |

---

## 10. Custom framework or inventory

### Custom inventory

Set `Config.Inventory = 'custom'` and fill in the `impl.custom` block in `bridge/server/inventory.lua`. Implement five functions:

```lua
Count(src, item)              -- number owned
Add(src, item, count, meta)   -- true on success
Remove(src, item, count)      -- true on success
CanCarry(src, item, count)    -- true if it fits
GetMeta(src, item)            -- metadata table of the first matching stack
```

### Custom framework

Set `Config.Framework = 'custom'` and implement the same functions in `bridge/server/framework.lua`. For item use, call this when your system uses an item:

```lua
exports['g7_illegalweapon']:UseItem(source, 'g7_printer')
```

### Custom dispatch

Set `Config.Dispatch = 'custom'` and fill in `Config.CustomDispatch` in `config/printer.lua`.

---

## 11. Troubleshooting

| Problem | Fix |
|---|---|
| Console says `inventory: custom` or `framework: custom` | The script was started before your inventory or framework. Fix the `ensure` order, or force the value in `config/main.lua`. |
| Item does nothing when used | The item is not registered, or its name is misspelled. Compare with `install/` and restart the inventory. |
| Printer is a different model | The custom prop did not load, so the fallback is in use. Check the `.ytyp` and the archetype name. |
| Printer floats or sinks | Adjust `Config.Printer.zOffset`. |
| Weapon is not received after assembly | The `result.item` name is not registered in your inventory. |
| No ATM option | The option only appears if you have an unpaid order. Set `Config.ATM.showOnlyWithOrder = false` to always show it. |
| No target prompts | Your target script must start before this one. Without a target script, use **E** and **G** prompts. |
| Dispatch alerts do nothing | Check that the dispatch script name matches, or use `custom`. Alerts depend on your dispatch version. |
| Courier does not appear | Check the meet point coordinates. The courier spawns when you are within about 120 m. |
| Need more detail | Set `Config.Debug = true` and read the server and F8 consoles. |

---

## 12. Updating

1. Back up your `config/` folder and `web/img/`.
2. Replace the resource files.
3. Restore your config and logo, and compare against the new config for added options.
4. Restart the resource. Printers are kept.

---

## Support

For support, contact **G7 Development**.
