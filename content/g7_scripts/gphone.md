# The GPhone — Installation

This guide takes you from download to a working GPhone on your server. Follow the steps in order. It takes about 15 minutes.

## Requirements

| Requirement | Notes |
|---|---|
| **oxmysql** | Required. Must start before the phone. |
| **Framework** | Qbox, QBCore, ESX or standalone. Detected automatically. |
| **Inventory** | g7_inventory, ox_inventory, qs-inventory or qb-inventory (also ps / lj), or the default ESX inventory. Detected automatically. |
| **pma-voice** | Needed for call audio. Calls still ring and connect without it, but players won't hear each other. |
| **Banking (optional)** | g7_banking for bank cards in the Wallet app. Otherwise the framework bank account is used. |
| **Garage (optional)** | jg-advancedgarages, g7_garages, or your framework's default garage for the Garage app. |
| **Dispatch (optional)** | ps-dispatch for 911 calls. Without it, the phone's built-in alerts are sent to on-duty players. |

No extra screenshot resource is needed. Photos use the built-in G7 capture, and `screenshot-basic` is only used as a backup if it happens to be running.

## Step 1 — Add the resource

1. Download `g7_phone` from your Cfx.re Keymaster.
2. Unzip it and place the `g7_phone` folder in your server's `resources` folder.

> **Important:** do not rename the folder. It must stay exactly `g7_phone`, or the items and the UI won't load.

## Step 2 — Start order in server.cfg

The phone must start **after** everything it connects to. Add `ensure g7_phone` below your database, framework, inventory, voice, banking, garage and dispatch resources:

```cfg
ensure oxmysql
ensure qb-core              # or qbx_core / es_extended
ensure ox_inventory         # or g7_inventory / qs-inventory / qb-inventory
ensure pma-voice

# optional, only if you use them
ensure g7_banking
ensure jg-advancedgarages
ensure ps-dispatch

ensure g7_phone
```

If the phone starts before one of these, it won't detect it. Moving `ensure g7_phone` lower in the file fixes that.

## Step 3 — Database

Nothing to do. The phone creates all of its tables automatically the first time it starts.

If you prefer to import SQL by hand, run `install/g7_phone.sql` in your database. It's safe to run more than once.

## Step 4 — Add the items

All item files are in `install/items/`. Pick the row for your inventory, open the file, and **paste its contents into** the item file listed. Don't replace the whole file, or you'll delete your other items.

| Your inventory | Item file | Paste into |
|---|---|---|
| **g7_inventory** | `ox_inventory_and_g7_inventory.lua` | `g7_inventory/data/items.lua` |
| **ox_inventory** | `ox_inventory_and_g7_inventory.lua` | `ox_inventory/data/items.lua` |
| **Qbox** (uses ox_inventory) | `ox_inventory_and_g7_inventory.lua` | `ox_inventory/data/items.lua` |
| **QBCore** (qb / ps / lj-inventory) | `qb_and_qbox.lua` | `qb-core/shared/items.lua` |
| **qs-inventory** | `qs_inventory.lua` | `qs-inventory/shared/items.lua` |
| **ESX default inventory** | `esx.sql` | Run it in your database |

> **Note for Qbox:** use the ox_inventory file, not the QBCore one. Qbox reads its items from ox_inventory.

> **Note for the default ESX inventory:** it doesn't support item metadata, so each character can only hold one phone of each model.

The phones must stay **non-stackable**. The item files already set this (`stack = false` or `unique = true`), so don't change it.

These are the 27 items the phone adds:

| Type | Items |
|---|---|
| **Phones** | `gphone_promax`, `gphone_pro`, `gphone_fold`, `gphone_e`, `nova_ultra`, `nova_fold`, `nova_flip` |
| **Charging** | `phone_charger` |
| **Cases** | `phone_case_clear`, `phone_case_leather`, `phone_case_wallet`, `phone_case_rugged`, `phone_case_custom`, `phone_case_red`, `phone_case_blue`, `phone_case_green`, `phone_case_purple` |
| **Accessories** | `screen_protector`, `privacy_protector`, `phone_grip` |
| **SIM cards** | `sim_card`, `burner_sim` |
| **Other** | `jailbreak_tool`, `gbuds`, `g7_tag`, `home_camera`, `phone_screen` |

If an item already exists on your server with the same name, skip that one.

## Step 5 — Add the item images

Copy **every** PNG from `g7_phone/install/images/` (27 images) into your inventory's image folder:

| Inventory | Image folder |
|---|---|
| g7_inventory | `g7_inventory/web/images/` |
| ox_inventory / Qbox | `ox_inventory/web/images/` |
| qb-inventory | `qb-inventory/html/images/` |
| ps / lj-inventory | the `html/images/` folder of that inventory |
| qs-inventory | `qs-inventory/html/images/` |

## Step 6 — Set up your config

Open `g7_phone/config/config.lua`. Every setting is in this one file, and translations are in `config/locales.lua`. Check these settings before your first start:

| Setting | What to set |
|---|---|
| `Config.Framework` | Leave on `'auto'`. Change it only if the wrong framework is detected (`'qbox'`, `'qb'`, `'esx'`, `'standalone'`). |
| `Config.Inventory` | Leave on `'auto'`. If you run more than one inventory, set the one players actually use. |
| `Config.Banking` | `'auto'` uses g7_banking when it's running, otherwise the framework bank. Use `'custom'` to connect another banking script in `bridge/banking/custom.lua`. |
| `Config.Garage` | Set to the garage you use: `'jg-advancedgarages'`, `'g7_garages'`, `'framework'`, or `'auto'` to detect it. |
| `Config.Dispatch` | `'auto'` uses ps-dispatch when it's running. Use `'custom'` to connect another dispatch in `bridge/dispatch/custom.lua`. |
| `Config.Services` | The police job names that receive 911 calls. Replace them with your server's job names. |
| `Config.Alerts.Jobs` | The jobs that can send Emergency Alerts. Replace them with your server's job names. |
| `Config.AreaCodes` | The area codes new phone numbers use, e.g. `{ '213' }`. |
| `Config.RequirePhoneItem` | `true` = players need a phone item. `false` = everyone has a phone (good for standalone servers). |
| `Config.OpenKey` | The key that opens the phone (default `F1`). Players can rebind it in their FiveM key bindings. |

**Custom street names (optional):** if your server renames GTA streets, add them to `Config.StreetNames` so Maps, navigation and 911 locations use your names. Leave it empty to use the default GTA names.

## Step 7 — Photo storage (optional)

Photos work out of the box. By default they're saved in your database, with no API key needed. To store them somewhere else, change `Config.Media.Provider`:

| Provider | What you need |
|---|---|
| `'database'` (default) | Nothing. |
| `'fivemanage'` | Set `Config.Media.ApiKey` to your Fivemanage API key. |
| `'custom'` | Set `CustomUrl` to your upload host, plus `CustomField`, `CustomHeaders` and `CustomUrlPath` if it needs them. |

**Discord photo log (optional):** paste a webhook into `Config.Discord.Webhook` to post every photo to a staff channel. Players can also add their own personal webhook in **Settings → Camera & Photos**.

> API keys and webhooks stay on the server. They're never sent to players.

## Step 8 — Admin permissions (optional)

Add any of these to your `server.cfg`. Change `group.admin` to your own staff group if it's different:

```cfg
add_ace group.admin g7phone.admin allow     # App Builder
add_ace group.admin g7phone.alert allow     # Emergency Alerts
add_ace group.admin g7phone.repair allow    # /repairphone
```

## Step 9 — Custom phone models (optional)

The phone uses a default GTA phone prop, so you can skip this step. To use your own phone or case models:

1. Put your `.ydr` and `.ytyp` files in `g7_phone/stream/`.
2. Set the model name in `Config.Phones` (the `prop` of each phone), or in `Config.CaseProps` for cases.
3. Do a full server restart.

## Step 10 — Restart and test

1. Do a **full server restart** the first time. Restarting just the resource won't load the streamed files.
2. Check your server console for this line, which shows what the phone detected:

   ```text
   [g7_phone] The GPhone started · framework: qb · inventory: ox_inventory · banking: g7_banking
   ```

   If the framework, inventory or banking shown isn't the one you use, set it in `config/config.lua` (Step 6).
3. Give yourself a phone. For example, on ox_inventory, QBCore or ESX:

   ```text
   /giveitem [your id] gphone_promax 1
   ```

4. Press **F1** (or use the phone item) to open it.

Then check:

- [ ] The phone opens and the setup screen starts.
- [ ] You get a phone number with one of your area codes.
- [ ] Calling another player rings, and you can hear each other (needs pma-voice).
- [ ] The Camera takes a photo and it shows in Photos.
- [ ] The Wallet shows your bank cards (if you use g7_banking).

---

Store: https://g7-store.pages.dev/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.
