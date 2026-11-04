# G7 CineStudio — Installation

## Requirements
- A framework: Qbox, QBCore, ESX, or standalone (auto-detected)
- `oxmysql` (only if using database storage)
- A supported inventory (only if using the optional camera item)

## Step 1 — Add the resource
Drop `g7_cinestudio` into your resources folder.

## Step 2 — Add your logo
Put your `g7.png` logo in `web/assets/` (it falls back to the G7 mark if missing).

## Step 3 — server.cfg
```cfg
# your framework here
ensure g7_cinestudio
```

## Step 4 — Set access
Open `config.lua` and choose who can use it: everyone, ACE, staff groups or jobs (e.g. a news job). For ACE, set `Config.Access.Ace.Enabled = true` and add:
```cfg
add_ace group.admin g7.cinestudio.use allow
add_ace group.vip g7.cinestudio.use allow

# extended camera range
add_ace group.admin g7.cinestudio.staff allow
```

## Step 5 — Camera item (optional)
1. Add the item from `install/` for your inventory (g7/ox, Qbox, qb, qs or ESX SQL).
2. Copy `install/images/cinecamera.png` into your inventory image folder.
3. Set `Config.Item.Enabled = true` (and `RequiredForCommand = true` if `/cine` should need the item).

## Step 6 — Storage (optional)
- Default is `kvp`: saved on each player's PC, nothing to install.
- For server-side storage, set `Config.Storage = 'database'` and make sure `oxmysql` starts first. The table is created automatically (or run `sql/g7_cinestudio.sql`).

## Step 7 — Configure
- Limits and theme in `config.lua`
- Controls in `Config.Controls` (update `Config.KeyLabels` to match if changed)
- Use `Config.Clean.OnHide` / `OnShow` to hide your own HUD resources during clean capture

## Step 8 — Test
1. Restart the server.
2. Type `/cine`.
3. Press **H** for clean capture and **G** for pose mode.

> Depth of field needs **Post FX: High/Ultra** and **DOF** turned on in GTA graphics settings.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.