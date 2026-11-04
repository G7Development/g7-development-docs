# g7_advancedrolling — G7 Development

Advanced blunt rolling & smoking for **QBCore / Qbox / ESX**, with **g7_inventory, ox_inventory, qs-inventory and qb-inventory** (plus ps / lj) support. The framework and inventory are detected automatically.

Store: https://g7-development.tebex.io/  •  Discord: https://discord.gg/P6XQ4N6M95

## Features
- **3D rolling tray** (same 3D tech as the G7 Vape System, everything built in code):
  1. Use your weed and the tray opens with your bud, your grinder and your wood
  2. Click the grinder lid to take it off, drag the bud in, put the lid back on
  3. **Press E to grind up [strain]** - progress bar (`Config.Tray.grind.duration`)
  4. The tray opens again: open the grinder, split the wood, gut the tobacco, drag the ground weed onto the wood
  5. **Press E to roll up the wood** - your custom animation plays (`Config.Tray.roll.anim`) with a progress bar, then the blunt is in your inventory
- **Hash**: every grind drops a little hash into the grinder (saved on the grinder item). Use the grinder to open a side panel and scrape it out (E), or ESC to exit. 2g of hash rolls a **Hash Blunt** (straight onto the wood, no grinding)
- Full-screen 3D: glossy enamel tray on a wood desk with real-time shadows, a machined **silver grinder** with knurled grip, and a **real bud photo** (per-strain photos supported) that grinds into real-looking ground weed
- Step tracker, pulsing guide rings on what to click or drag next, and a big key prompt for grinding and rolling up
- Pick your wrap right on the tray. Ships with the G7 tray art (`web/img/trays/g7_tray.jpg`), plus smoke, marble, leaf and carbon, or your own artwork
- **G7 UI** in a clean **baby blue** theme (or Black & White, Kush green, Black & Gold, G7 purple and more): the same design system as the G7 Vape & Lean systems (Space Grotesk + Inter, G7 theme presets, side HUD, bottom toasts, G7 logo). It has its own progress bar, skill check, puff-puff-pass card, wrap picker and a roller profile + strain guide screen
- **Theme sync**: if `g7_vapesystem` is running, the rolling UI follows the theme your admins pick live in `/vapeadmin`
- **Lighter required**: lighting up plays an animation with a lighter prop and a flickering flame. Lighters run out of fluid (uses are saved on the item) and warn you when they're low. A blunt that gets passed stays lit
- **G7 custom smoke**: every puff blows the same streamed cloud as the G7 Vape System. It's tinted per strain, pushed by the wind, hangs indoors, and every nearby player sees it. Smoke also curls off the lit end of the blunt
- Auto-detects your framework (qbx_core → qb-core → es_extended) and inventory (g7 → ox → qs → qb/ps/lj → ESX default)
- **23 strains** across 5 rarity tiers (Common → Legendary), each Indica / Sativa / Hybrid with its own flavor, effects and special buff
- **11 wraps**: 10 G7 Woods flavors + Fronto Leaf, packaged in foil pouches. Each has its own puff bonus and effect bonus
- Open wrap packs to get wraps (custom pack prop + animation)
- Pick which wrap you roll with from a menu
- 4-stage rolling with the custom rolling animation and blunt prop: split → gut → load → seal, with a skill check at every stage
- Quality system: **Perfect / Clean / Sloppy** based on how you did, saved on the blunt as metadata (quality, puffs, wrap, who rolled it)
- Fail too many checks and the wrap rips (your weed is kept)
- Tobacco guts by-product when you gut the wrap
- Optional grinder requirement
- Rolling levels and XP saved to the database. Higher tiers unlock at higher levels and give more XP, and higher levels get easier skill checks and a chance to upgrade quality
- `/rollstats` shows your stats. `/strains` opens the strain guide with every strain's tier, type, effects and lock status
- Strain buffs: Sativas give a speed boost, Hybrids give unlimited stamina, Indicas regen health, and Legendaries give speed and stamina
- Strain visuals: some strains have their own high screen effect
- Smoking system: take puffs, with networked smoke, armor, health, stress relief and a high effect (screen effect, walk style, camera shake) that changes per strain
- **Puff puff pass**: pass a lit blunt to the closest player, who can accept or decline
- Put a blunt out and keep the puffs you have left for later
- Server-side security: session tokens, timing checks, and item checks on the server, so the client can't choose how many items it gets
- Discord webhook logs (the webhook stays server-side)
- Exports for other scripts

## Installation
1. Drop `g7_advancedrolling` in your resources folder.
2. Make sure your framework, inventory, `ox_lib` and `oxmysql` start **before** it, then add:
   ```
   ensure g7_advancedrolling
   ```
3. Add the items for your inventory from `install/`:
   | Inventory | File | Where |
   |---|---|---|
   | g7_inventory / ox_inventory | `items_ox_g7_inventory.lua` | `data/items.lua` |
   | qb-inventory / ps / lj | `items_qb_core.lua` | `qb-core/shared/items.lua` |
   | qs-inventory | `items_qs_inventory.lua` | `qs-inventory/shared/items.lua` |
   | ESX default inventory | `items_esx.sql` | run in your database |
4. Copy `install/images/*` into your inventory's image folder (ox: `web/images`, qb: `html/images`, qs: `html/images`).
5. The `g7_rolling_stats` table is created automatically the first time the script starts.
6. Do a **full server restart** the first time so the streamed smoke particle (`g7rollsmoke.ypt`) and animation load.

## Items

**24 strains (23 + hash), 11 wraps, 73 items in total.** Every item has an icon in `install/images`.

### Strains
| Strain | Tier | Type | Weed item | Blunt item |
|---|---|---|---|---|
| Banana Splitz | Common | Hybrid | `weed_banana_splitz` | `blunt_banana_splitz` |
| Blue Dream | Common | Sativa | `weed_blue_dream` | `blunt_blue_dream` |
| Blueberry Muffin | Common | Indica | `weed_blueberry_muffin` | `blunt_blueberry_muffin` |
| Purple Haze | Common | Sativa | `weed_purple_haze` | `blunt_purple_haze` |
| Sour Static | Common | Sativa | `weed_sour_static` | `blunt_sour_static` |
| Strawberry Pie | Common | Hybrid | `weed_strawberry_pie` | `blunt_strawberry_pie` |
| Cotton Candy Cloud | Uncommon | Hybrid | `weed_cotton_cloud` | `blunt_cotton_cloud` |
| Grape Smack | Uncommon | Indica | `weed_grape_smack` | `blunt_grape_smack` |
| Ice Cream Kush | Uncommon | Indica | `weed_ice_cream_kush` | `blunt_ice_cream_kush` |
| Mango Tango | Uncommon | Sativa | `weed_mango_tango` | `blunt_mango_tango` |
| OG Kush | Uncommon | Hybrid | `weed_og_kush` | `blunt_og_kush` |
| Pink Lemonz | Uncommon | Sativa | `weed_pink_lemonz` | `blunt_pink_lemonz` |
| Watermelon Slush | Uncommon | Sativa | `weed_watermelon_slush` | `blunt_watermelon_slush` |
| Cookies N Kream | Rare | Indica | `weed_cookies_kream` | `blunt_cookies_kream` |
| Gelato Cake | Rare | Indica | `weed_gelato_cake` | `blunt_gelato_cake` |
| Gorilla Funk | Rare | Hybrid | `weed_gorilla_funk` | `blunt_gorilla_funk` |
| Northern Flame | Rare | Indica | `weed_northern_flame` | `blunt_northern_flame` |
| Purple Runtz | Rare | Hybrid | `weed_purple_runtz` | `blunt_purple_runtz` |
| Black Cobra | Exotic | Indica | `weed_black_cobra` | `blunt_black_cobra` |
| Lemon Cherry Drip | Exotic | Sativa | `weed_lemon_cherry_drip` | `blunt_lemon_cherry_drip` |
| Wedding Frost | Exotic | Hybrid | `weed_wedding_frost` | `blunt_wedding_frost` |
| G7 Gas | Legendary | Hybrid | `weed_g7_gas` | `blunt_g7_gas` |
| Red Stick OG | Legendary | Hybrid | `weed_red_stick_og` | `blunt_red_stick_og` |

### Wraps
| Wrap | Pack item | Wrap item |
|---|---|---|
| Original Woods | `wrap_pack_original` | `wrap_original` |
| Honey Woods | `wrap_pack_honey` | `wrap_honey` |
| Russian Cream Woods | `wrap_pack_cream` | `wrap_cream` |
| Grape Woods | `wrap_pack_grape` | `wrap_grape` |
| Banana Woods | `wrap_pack_banana` | `wrap_banana` |
| Vanilla Woods | `wrap_pack_vanilla` | `wrap_vanilla` |
| Dark Stout Woods | `wrap_pack_dark_stout` | `wrap_dark_stout` |
| Wild Rum Woods | `wrap_pack_wild_rum` | `wrap_wild_rum` |
| Sweet Aromatic Woods | `wrap_pack_sweet_aromatic` | `wrap_sweet_aromatic` |
| Cherry Vanilla Woods | `wrap_pack_cherry_vanilla` | `wrap_cherry_vanilla` |
| Fronto Leaf | `wrap_pack_fronto` | `wrap_fronto` |

### Other
| Item | What it does |
|---|---|
| `weed_grinder` | Needed for the rolling tray. Collects hash - use it to scrape the hash out |
| `tobacco_guts` | By-product from gutting a wrap |
| `weed_hash` / `blunt_hash` | Hash scraped out of the grinder (1 item = 1g) and the Hash Blunt it rolls |
| `lighter` | Needed to light a blunt. 40 lights, then it runs dry (`Config.Lighter`). Skip it if your server already has a lighter and set `Config.Lighter.item` |

Add more strains or wraps in `config.lua`. The items are registered as usable automatically.

## Controls
- Rolling: **E** start • **X** put away / cancel • skill check: press the key shown when the needle is in the zone
- Smoking: **hold E** to take a hit (the longer you hold, the bigger the hit; hold too long and you cough) • **G** pass • **X** put out
- `/bluntgrip` (admins): line the blunt up in the hand live, ENTER copies the config line
- Getting passed a blunt: **Y** accept • **N** decline

## Config notes
- `Config.Rolling.mode = 'tray'` uses the 3D rolling tray. `'classic'` uses the old skill check stages (also used automatically when `Config.UI.style = 'ox_lib'`).
- `Config.Tray.grind.duration` / `Config.Tray.roll.duration` set how long grinding and rolling up take. Put your custom lick animation in `Config.Tray.roll.anim` (stream the `.ycd` in `stream/`).
- `Config.Tray.roll.checks` = skill checks while rolling up (they decide the quality). Set it to `0` to turn them off.
- `Config.Tray.skin` = one design or a list of designs (ships with 3 G7 trays). `Config.Tray.skinMode` = `random` (new design every time the tray opens), `session` (one per roll) or `cycle`. Designs: `smoke`, `marble`, `leaf`, `carbon`, or your own image in `web/img/trays/` (the tray floor only, about 2048×1254).
- `Config.Tray.quality` = `high` (real-time shadows) or `low` for weaker PCs. `Config.Tray.grinderColor` sets the grinder color (silver by default).
- Buds on the tray: a cut-out, top-down photo of a few nugs (`web/img/buds/cluster.png`). Set `budImage = 'img/buds/yourstrain.png'` on a strain to give it its own. Purple strains tint the photo automatically.
- `Config.Hash` controls how much hash each grind collects. The hash strain is `Config.Strains.hash` (its `perRoll` is the grams needed).
- `Config.UI.style = 'g7'` uses the G7 screens. Set it to `'ox_lib'` for plain ox_lib menus, progress bars and text UI.
- `Config.Theme` takes `g7`, `frost`, `toxic`, `ember`, `gold`, `mono`, or `'custom'` with your own 4 colors. `syncWithVape = true` follows the vape system's live theme.
- `Config.Smoke` controls the cloud (size per puff, exhale length, strain tint, wind). Lines marked `[asset]` are tied to `stream/g7rollsmoke.ypt`. `style = 'basic'` switches to a vanilla GTA puff.
- The G7 menus show item images straight from your inventory's image folder (`Config.UI.images = 'auto'`). If your g7_inventory keeps images somewhere else, set the path, e.g. `'nui://g7_inventory/web/images/%s.png'`.
- `Config.UI` uses ox_lib by default. Set `progress`/`notify` to `'custom'` and edit `client/bridge.lua` to use your own G7 progress bar / notifications.
- `Config.Smoking.stress = 'auto'` uses `hud:server:RelieveStress` on QB/Qbox and `esx_status` on ESX.
- Discord logs go in `server/sv_config.lua`.

## Exports
```lua
-- server
exports.g7_advancedrolling:GetRollingLevel(source)   -- level index, label
exports.g7_advancedrolling:GetRollingStats(source)   -- { xp, rolled, perfect, ripped }
exports.g7_advancedrolling:AddRollingXP(source, amount)
exports.g7_advancedrolling:IsBusy(source)
-- client
exports.g7_advancedrolling:IsBusy()
```

## Stream assets
- `g7_rolling_pack` (pack prop), `g7_rolling_blunt` (blunt prop), and the `g7_advancedrolling@anim` animation dictionary with the `g7_advroll_blunt_clip` clip.
- These are custom model names, so the script **no longer replaces** the vanilla `prop_paper_bag_small` / `tr_prop_tr_usb_drive_01a` models across your whole server.

---


