# G7 · Baddies VIP (`g7_vipfemale`)

The members-only pink tablet for the women of your city. Open it with `/vipfemale` (or the tablet item) and get a full in-game app:
rewards, a season pass, a live lounge, private messages, an event planner, a built-in beauty job, a custom drug, a VIP boutique, a car showroom, a safety timer and more.

**G7 Development** · Tebex: https://g7-development.tebex.io · Discord: https://discord.gg/P6XQ4N6M95

---

## Requirements
- `oxmysql` (tables are created automatically)
- OneSync
- **No ox_lib needed**

Works with **Qbox, QBCore, ESX and standalone** (auto-detected) and **g7_inventory, ox_inventory, qs-inventory, lj-inventory, qb-inventory and the ESX inventory** (auto-detected).

## Install
1. Drop `g7_vipfemale` into your resources folder.
2. Add to `server.cfg` **after** oxmysql, your framework and your inventory:
   ```cfg
   ensure g7_vipfemale
   add_ace group.admin vipfemale.admin allow
   ```
3. Add the items for your inventory from `install/`:
   | Inventory | File |
   |---|---|
   | g7_inventory, ox_inventory (Qbox) | `install/items_ox.lua` |
   | qb-inventory, lj-inventory, ps-inventory | `install/items_qb.lua` |
   | qs-inventory | `install/items_qs.lua` |
   | ESX default / esx_inventoryhud | `install/items_esx.sql` |
4. Copy `install/images/*.png` into your inventory's image folder.
5. Replace `web/img/g7.png` with your logo (square PNG, 512×512 looks best).
6. Restart the server and type `/vipfemale`.

## Files you can edit
| File | What's in it |
|---|---|
| `config.lua` | Everything: access, starter pack, daily rewards, wheel, season, challenges, events, boutique, showroom, closet, teleports, safety, Glam Squad job, Pink Diamond drug |
| `config_server.lua` | Server-only: Discord bot token and roles, Discord webhooks, **Tebex packages** |
| `data/businesses.lua` | City Guide: woman-owned businesses (with GPS + teleport), activities, jobs |
| `locales/*.lua` | English, Spanish, German, French, Brazilian Portuguese |
| `editable/client.lua` | Notifications, vehicle keys, fuel, appearance systems, stress, police dispatch |
| `editable/server.lua` | Qbox keys, police count, standalone money/item hooks |
| `bridge/*.lua` | Framework + inventory bridge |
| `web/*` | The whole tablet UI (plain HTML/CSS/JS, no build step) |

## Who can open it
`Config.Access.mode`:
- `female`: saved character gender (QB/Qbox `charinfo.gender`, ESX `sex`, standalone: female freemode ped)
- `discord`: one of the Discord roles in `config_server.lua`
- `either` / `both`
- `open`: everyone

Staff can always open it (`Config.Access.staffBypass`). Override per player with `/vipfemale_allow` and `/vipfemale_deny`.

## Features
- **Home screen** with live widgets: streak, Glam Wheel spins, season level, next event, online friends
- **Rewards**: 7-day login streak (VIPs get 2x), daily Glam Wheel, one-time Starter Pack + Starter Car, Tebex store claim
- **Season pass**: 10 levels, XP from logins, lounge, events, job, driving (tracked server side) and flowers
- **Weekly challenges**: a new set every Monday, same set for everyone, drawn from your pool
- **The Lounge**: live chat with @mentions, emoji bar, saved history, staff delete, optional Discord mirror
- **Messages**: private DMs between friends, delivered live with unread badges
- **Besties**: friends, requests, search, blocks (both ways), and up to 5 Besties for safety alerts
- **Flowers**: send a few compliments a day, they show on her profile and the Most Loved board
- **Event planner**: date/time, location (here, waypoint or a business), invite friends, RSVP, live countdown, GPS check-in rewards, reminders, host bonus
- **Glam Squad job**: freelance beauty bookings with GPS, a client ped, a nail-art mini game, pay + tips + XP
- **Pink Diamond drug**: pick orchids, cook at the lab, bag, then sell to locals in selling mode (`/vfsell` or the Plug app), with police alerts and a use effect
- **Boutique**: buy with Baddie Points, cash or bank; VIP-only items; **gift items to a friend**
- **Showroom**: cars registered to the garage and delivered next to her with keys
- **Closet**: opens your appearance system (auto-detects illenium-appearance, fivem-appearance, qb-clothing, esx_skin, or plug in your own with `Config.Closet.system = 'custom'`). Locked to a Discord role by default (`Config.Closet.require`), staff always bypass
- **City Guide**: businesses with GPS and teleports, activities, jobs
- **Get Home Safe + SOS**: a timer that sends her live location to her Besties if she doesn't check in, a hold-to-send SOS, and an alert if she disconnects with the timer running
- **Top Girls**: leaderboards for season XP, flowers, Glam Squad bookings and km driven
- **Champagne Rosé design** in light and dark mode, with a realistic metal tablet frame (rose gold, champagne, black chrome or pink gloss). Players pick their mode and frame in the Me app; set the defaults in `Config.DefaultTheme` and `Config.Tablet.finish`

## Tebex
1. In `config_server.lua`, list your packages under `ServerConfig.Tebex.packages` (each has a key, label and reward).
2. In Tebex, give the package this server command (runs on purchase, player does not need to be online):
   ```
   vipfemale_tebex {transaction} vip_month
   ```
   The last word must match a package key.
3. The player opens **Rewards → Store Claim**, pastes her transaction ID (`tbx-…`) and gets the reward. Each purchase can be claimed once.
4. Optional chargeback command: `vipfemale_tebex_revoke {transaction}`

Rewards can be any mix of `cash`, `bank`, `points`, `xp`, `items`, `vipDays` and `vehicle`.

## Staff commands
| Command | |
|---|---|
| `/vipfemale_allow [id]` | Let a player open the tablet |
| `/vipfemale_deny [id]` | Block a player from the tablet |
| `/vipfemale_clearaccess [id]` | Remove the override |
| `/vipfemale_reset [id] [starter\|vehicle\|daily\|season\|all]` | Reset claims |
| `/vipfemale_givevip [id] [days]` | Give VIP |
| `/vipfemale_removevip [id]` | Remove VIP |
| `/vipfemale_points [id] [amount]` | Give (or take with a negative number) Baddie Points |
| `/vipfemale_announce [message]` | Pink announcement to every member |

## Exports (client)
```lua
exports.g7_vipfemale:open()
exports.g7_vipfemale:close()
exports.g7_vipfemale:isOpen()
```

## Notes
- **Item images** load from your inventory's image folder automatically. Change `Config.ItemImagePath` if yours live somewhere else (for example `nui://g7_inventory/web/images/`).
- **Dirty money**: drug sales pay `black_money` by default. ESX uses the account; other frameworks get the `Config.DirtyMoneyItem` item. Set `Config.Drug.sell.account = 'cash'` if you don't use dirty money.
- **Vehicle keys / fuel / clothing / dispatch** are auto-detected in `editable/client.lua`. Add your own resource there if it isn't listed.
- **Security**: every reward, claim, purchase, message and check-in is validated on the server. Claims are atomic (no double claims), requests are rate limited and all SQL is parameterized. Identifiers are never sent to other players.
