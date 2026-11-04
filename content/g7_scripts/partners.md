# G7 Partners — `g7_partners`
**Relationships • Couple Progression • Marriage** — by G7 Development

A full relationship system for FiveM. Players ask each other out and level up together through weekly objectives, dates, gifts and letters. They can get engaged, have a wedding at the chapel, and keep a history of past relationships on their profile.

---

## Requirements
- **ox_lib**
- **oxmysql**
- Optional: **screenshot-basic** (photo booth) + a Fivemanage key or Discord webhook
- A framework: **QBCore**, **Qbox** or **ESX** (detected automatically)
- Optional: **ox_target** or **qb-target** (detected automatically; commands + `[E]` prompts are used if neither is running)

## Auto-detection
| | Detected | Override in `config.lua` |
|---|---|---|
| Framework | `qbx_core` → `qb-core` → `es_extended` | `Config.Framework` |
| Inventory | `g7_inventory` → `ox_inventory` → `qs-inventory` → `qb-inventory` / `ps-inventory` / `lj-inventory` → default ESX | `Config.Inventory` |
| Target | `ox_target` → `qb-target` → none | `Config.Target` |

The server console shows what was picked when the resource starts:
`[g7_partners] framework: qb | inventory: g7 | active couples: 12`

Using something else? Set `Config.Inventory = 'custom'` and fill in
`bridge/server/custom_inventory.lua` and `bridge/client/custom_inventory.lua`.

---

## Installation
1. Drop `g7_partners` into your resources folder.
2. Put your **`g7.png`** logo in `web/img/` (the UI hides the logo slot if it's missing).
3. Add the items for your inventory from `install/items/`:
   - **g7_inventory / ox_inventory** → `ox_inventory.lua` (uses `client = { export = 'g7_partners.useItem' }`)
   - **qb-inventory / ps / lj** → `qb-core.lua` (into `qb-core/shared/items.lua`)
   - **qs-inventory** → `qs-inventory.lua`
   - **Default ESX inventory** → run `esx_items.sql`
4. Copy the item images from `install/images/` into your inventory's image folder (13 items).
5. Make sure it starts **after** your framework, inventory, ox_lib and oxmysql:
   ```
   ensure ox_lib
   ensure oxmysql
   ensure g7_partners
   ```
6. Restart. **Database tables are created automatically**, and updating from 1.0 adds the new columns for you. (`install/g7_partners.sql` is there if you'd rather run it by hand.)
7. Optional jobs: create a `weddingplanner` job and make sure your judge / lawyer jobs match `Config.Court.Jobs`.

---

## Features
- **Asking out** — target a nearby player (or use `/askout` or the Nearby list in the hub). They get an in-world card they answer with **Y / N** while they keep moving. Distance, cooldowns, spam and "already taken" are all checked on the server, then checked again when they accept.
- **Couple Hub** (`/partners` or **F7**) — couple card, level ring, journey (Dating → Engaged → Married), stats, quick actions.
- **Shared levels & XP** — up to level 25 (configurable curve). XP comes from objectives, dates, gifts, letters, couple emotes and time spent together.
- **Weekly objectives** — each couple gets 5 random goals every week (Monday 00:00 UTC). Time together, walking and driving are measured **by the server from real positions**, so they can't be faked. Finishing all of them pays bonus XP.
- **Date spots** — the pier, the observatory, the Vinewood Sign and Mount Chiliad. Stay there together for 5 minutes to complete a date.
- **Gift shop** — physical shop with a ped, categories and a cart. Pay with cash or bank, have the gifts **delivered straight to your partner** with an attached note, or keep them.
- **Usable gifts** — hold roses or a teddy bear, eat chocolates, drink champagne (with a little buzz), admire perfume and jewelry. They relieve stress and restore hunger and thirst.
- **Give Gift** — target your partner and hand them something from your pockets.
- **Love letters** — write on a *Letter Note*. Your partner gets a *Sealed Letter* item and the letter shows up in the hub inbox.
- **Engagement** — propose from the hub once you reach the required level (the engagement ring moves to your partner).
- **Wedding ceremony** — both partners go to the priest, one starts the ceremony and the other confirms with "I do". There's a ceremony overlay, a kiss, confetti, wedding rings for both and a city-wide announcement.
- **Break up / divorce** — dating couples break up for free. Married couples pay a divorce fee (optionally only at the chapel). Both players get a cooldown.
- **Relationship history** — every ending is saved to both profiles (partner, stage reached, level, dates, who ended it).
- **Profiles** — nickname, bio, image link (approved hosts only) or live character headshot, plus privacy: **Everyone / Partner Only / Private**, and whether to show your partner and history.
- **Profile viewer** — target anyone to see whether they're single, dating, engaged or married (if their privacy setting allows it).
- **Shared couple stash** — gets bigger as your level goes up.
- **Partner tracker** — a map blip that only works while your partner allows it. The server checks permission and sends the position only to you.
- **Online / offline notifications** for your partner.
- **Floating hearts** above couples standing together, plus heart bursts at big moments.
- **Couple emotes** — hug, kiss, high five, cuddle and piggyback, synced between both players.
- **Top Couples** leaderboard (couples with private profiles are left out).
- **Themes** — Rosé, Crimson, Lilac, Champagne, Ocean and Mint, or a custom accent color picked by each player.
- **Discord logging** (optional webhook) and admin tools.


### New in 1.1
- **Dark mode** — neutral black UI by default. Players can switch to the tinted look in Settings.
- **Love Language** — each player secretly picks one. Actions that match your partner's language earn 1.5x XP. Guess theirs once a day for a bonus.
- **"In Sync" slow dance** — a two-player rhythm minigame from the couple emote menu. The server scores accuracy and how in sync you were.
- **Love Locks** — buy a lock at the gift shop and place it on the pier railing. It stays there for everyone to see, and cracks if you break up.
- **Where It Started** — where you met, your first date, the proposal and the wedding are saved with GPS. On your monthly anniversary, go back to where you met for bonus XP.
- **Thinking of You pings**, **Partner SOS** (flashing blip + GPS, even with tracking off) and **shared moods** (shown above your head to your partner only).
- **Comfort aura** — standing near your partner slowly lowers stress.
- **Secret Admirer** — single players send anonymous gifts and notes from the gift shop. The recipient gets 3 guesses, and a correct guess lets them ask the admirer out from anywhere.
- **Lonely Hearts board** at the gift shop — pin an ad; replies land in your Inbox.
- **Speed Dating events** — `/speeddating start [venue]` runs round-robin rounds with a marker over your date, then private picks. Mutual picks become matches.
- **Joint account** — deposits and withdrawals; big withdrawals need your partner's Y/N. Exports included for g7_banking.
- **Prenup** — signed when the ceremony starts. Decides how the joint account and stash are split on divorce. Leftover stashes stay openable from the hub.
- **Wedding venues** (chapel, beach, yacht), a **wedding planner job** (fee cut + manages invites), **RSVPs**, a **guestbook** guests sign at the venue, and a **bouquet toss** to a random single guest.
- **Contested divorce** — the other partner can contest; judges or lawyers rule on cases at Family Court (cases are auto-granted after 48h).
- **Photo booth** — a 4-shot strip with poses and a countdown. You get a photo strip item, and the shots are saved to a couple **Scrapbook**.
- **Second Chance** — exes who get back together within 30 days keep 60% of their old level.
- **Wishlist** — heart items at the gift shop. Gifting something from your partner's list pays 3x XP.
- **Seasonal calendar** — Valentine's, Halloween and the Holidays give an XP multiplier plus a limited-time objective.
- **Couple pet** — adopt together, feed, walk and pet it. A neglected pet runs away; after a breakup it goes to whoever cared for it more.
- **Inbox** for admirers, board replies and speed dating matches.

---

## Commands
| Command | What it does |
|---|---|
| `/partners` (F7) | Open the Couple Hub |
| `/askout` | Ask the closest player out |
| `/viewprofile` | View the closest player's profile |
| `/coupleemote` | Couple emote menu |
| `/cancelemote` | Stop a looping couple emote (or press **X**) |
| `/partnersadmin [info\|addxp\|setlevel\|end] [id] [amount]` | Admin (`group.admin`) |
| `/partnersadmin start [id] [otherId]` | Admin: make two players a couple right away (skips the request, distance and cooldown, for testing) |
| `/speeddating start [venueId]` / `stop` | Admin: run a speed dating event |
| `/thinkingofyou` | Send your partner a heart |
| `/sos` | Send your partner an SOS (bind a key in GTA settings) |

## Exports (server)
```lua
exports.g7_partners:GetPartner(source)            -- { source, citizenid, name, status, level } or nil
exports.g7_partners:GetRelationship(source)       -- { id, status, level, startedAt, marriedAt } or nil
exports.g7_partners:IsMarried(source)             -- boolean
exports.g7_partners:AddCoupleXP(source, amount, reason)
exports.g7_partners:AddObjectiveProgress(source, 'date_spot', 1)
exports.g7_partners:GetJointBalance(source)        -- joint account balance or nil
exports.g7_partners:JointDeposit(source, amount)    -- e.g. from g7_banking
exports.g7_partners:JointWithdraw(source, amount)
```
## Exports (client)
```lua
exports.g7_partners:OpenHub('objectives')
exports.g7_partners:GetState()
```

---

## Config highlights
Everything is in `config.lua`, with a comment on every option. The most common ones:
- `Config.Marriage.Venues` / `Config.GiftShop.Ped.coords` / `Config.Court.Ped.coords` / `Config.PhotoBooth.Coords` — move venues, shop, court and booth to fit your map (the yacht venue needs the yacht IPL loaded)
- `Config.PhotoBooth.Upload.MaxWidth` / `MaxSizeKB` — every photo is resized and compressed in the UI before it uploads (a 4K screenshot goes from ~7 MB to under 1 MB), so hosts never reject it as "file too large". Lower `MaxSizeKB` if your host's limit is smaller.
- `Config.PhotoBooth.Upload` — screenshot-basic uploads from the player's game, so the key / webhook is sent to players while they use the booth. Use an upload-only key. Set `Provider = 'custom'` and edit `PhotoBoothUpload` in `client/photobooth.lua` to use g7_cinestudio instead.
- `Config.Dates.Spots` — add your own date spots
- `Config.Objectives.Pool` — targets are written in **miles / minutes / counts**
- `Config.Units = 'imperial'` — players see miles and feet (`'metric'` for km)
- `Config.Profiles.ImageHosts` — which image hosts players can link from

## Support
Store: https://g7-development.tebex.io/ • Discord: https://discord.gg/P6XQ4N6M95

---


