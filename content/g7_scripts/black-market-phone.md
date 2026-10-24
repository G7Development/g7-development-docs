# G7 Black Market Phone — Installation

A Samsung-style burner phone for ordering contraband. It includes synced drop vans and dealers, burner calls with face-to-face meets, a camera and gallery, global and private chat, stock, heat and police dispatch. It works on its own or together with **G7 Black Market**.

---

## Requirements

| Resource | Required | Notes |
|---|---|---|
| [oxmysql](https://github.com/overextended/oxmysql) | **Yes** | Database |
| OneSync | **Yes** | Legacy or Infinity. Drop vans and dealers are created by the server |
| A framework | **Yes** | **QBCore**, **Qbox** or **ESX** (detected automatically) |
| An inventory | **Yes** | **g7_inventory** (recommended), ox_inventory, qs-inventory, qb-inventory, ps-inventory, codem-inventory, core_inventory or your framework's built-in inventory |
| [screenshot-basic](https://github.com/citizenfx/screenshot-basic) | For the camera | Without it, the Camera and Gallery apps hide themselves |
| g7_blackmarket | Optional | Adds shared reputation, the Contracts app, market events on phone prices and weapon serials |
| ox_lib | Recommended | Notifications and progress bars |
| ox_target / qb-target | Optional | For collecting orders and talking to dealers. Without a target script, players get `[E]` prompts |
| A dispatch script | Optional | ps-dispatch, cd_dispatch, qs-dispatch, rcore_dispatch or core_dispatch (detected automatically). It falls back to a built-in police alert |

---

## 1. Add the resource

1. Download `g7_blackmarket_phone` from your Keymaster / Tebex account.
2. Extract it into your `resources` folder. Keep the folder name **`g7_blackmarket_phone`**.

---

## 2. server.cfg

```cfg
ensure oxmysql
ensure ox_lib
ensure qbx_core               # or qb-core / es_extended
ensure g7_inventory           # or your inventory
ensure screenshot-basic       # for the camera

ensure g7_blackmarket         # optional, start it BEFORE the phone
ensure g7_blackmarket_phone
```

---

## 3. Database

**Nothing to do.** All tables are created automatically on the first start:

`g7_bmp_profiles` · `g7_bmp_orders` · `g7_bmp_messages` · `g7_bmp_stock` · `g7_bmp_photos`

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

### The burner phone item

The phone item is **`blackmarket_phone`**. How it opens depends on your inventory:

- **g7_inventory / ox_inventory / Qbox:** the item entry in the install file already opens the phone:
  ```lua
  ['blackmarket_phone'] = { label = 'Burner Phone', weight = 200, stack = false, close = true,
      description = 'Untraceable. Order from the underground.',
      client = { image = 'blackmarket_phone.png', export = 'g7_blackmarket_phone.usePhone' } },
  ```
- **QBCore / ESX:** the script registers the item as usable automatically.
- **Any other inventory:** when the item is used, run `TriggerServerEvent('g7_blackmarket_phone:server:use')` on the client, or `exports.g7_blackmarket_phone:UsePhone(source)` on the server.

To open the phone with a command as well, set `Config.Phone.command = 'bmphone'`.

---

## 5. Permissions

```cfg
# ACE
add_ace group.admin g7bmphone.admin allow
```

Groups listed in `Config.Admin.groups` (`admin`, `god` and `superadmin` by default) also count as admins.

| Command | Who | What it does |
|---|---|---|
| `/bmphoneadmin` | Admin | Opens the phone straight to the **Admin** app: overview, stock, players (heat and rep), orders (cancel or refund) and chat moderation |

---

## 6. Configure

Open `config.lua`. Settings to check first:

| Setting | Default | What it does |
|---|---|---|
| `Config.Phone.prop` | `prop_npc_phone_02` | Phone model in the player's hand. Stream your own prop and put its name here |
| `Config.Phone.ui` | Frost blue | Theme, brand, logo, position, wallpapers, frame colors and the size limits players can pick |
| `Config.Phone.ui.allowCustomize` | `true` | Lets players change wallpaper, frame color and phone size in Settings |
| `Config.PaymentAccount` | `'dirty'` | Account used to pay for orders |
| `Config.Catalog` | 16 items | Phone market stock, prices, rep locks and heat |
| `Config.Stock` | enabled | Limited stock and restocking |
| `Config.Heat` | enabled | Heat per order, how fast it cools down and the dispatch chance |
| `Config.Dispatch` | `'auto'` | Dispatch system, police jobs and minimum police online |
| `Config.Restrictions` | police / EMS, 3 zones | Blocked jobs and no-signal zones |
| `Config.Calls` | 3 dealers | Burner call dealers, their stock, rep requirements and cooldowns |
| `Config.Camera` | enabled | Photo quality, size limit and photos kept per player |
| `Config.DropLocations` | 35 locations | Drop van and meet spots |
| `Config.Logs` | `'none'` | `'discord'` (webhook) or `'fivemanage'` (API key) |

> **Test the drop locations on your map.** The 35 drop and meet spots use standard GTA streets and parking lots. If one clashes with an MLO, move it, or add `ped = vector4(...)` to place the dealer yourself.

---

## 7. Test it

1. Restart the server.
2. Give yourself a phone: `/additem [your id] blackmarket_phone 1` on g7_inventory or ox. On QB use `/giveitem`.
3. Use the phone. Order something from **Market** and wait for the driver.
4. Open **Phone → Contacts** and call **Dmitri** to set up a street meet.
5. Open **Camera**, press `Enter` to take a photo, then share it from **Gallery**.

---

## Exports

```lua
-- server
exports.g7_blackmarket_phone:UsePhone(src)
exports.g7_blackmarket_phone:GetHeat(src)
exports.g7_blackmarket_phone:AddHeat(src, amount)   -- trusted resources only
exports.g7_blackmarket_phone:GetStock(item)
exports.g7_blackmarket_phone:SetStock(item, amount)

-- client
exports.g7_blackmarket_phone:usePhone()             -- for inventory item exports
exports.g7_blackmarket_phone:IsOpen()
exports.g7_blackmarket_phone:Close()
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Phone doesn't open | Make sure the item is set up (step 4) and the player isn't on a blocked job or in a no-signal zone |
| "You don't have a burner" | `Config.Phone.requireItem = true` checks for the item on every action. Give the player `blackmarket_phone` |
| Camera / Gallery missing | Start `screenshot-basic` before the phone |
| Photos fail to save | Lower `Config.Camera.quality` or raise `Config.Camera.maxBytes` |
| Dealer doesn't answer | Every dealer has a `noAnswerChance`. Set it to `0` while testing. Ghost needs 3,500 rep |
| Van or dealer doesn't appear | OneSync must be enabled. Check the server console for errors |
| Phone prop floats or clips | Adjust `Config.Phone.offset` and `Config.Phone.rotation` |
| Phone is too big or small | Players can change the size in **Settings → Phone size**, or set `Config.Phone.ui.scale` |

Need help? Join the **[G7 Development Discord](https://discord.gg/P6XQ4N6M95)**.
