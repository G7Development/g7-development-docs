# G7 Beauty — Installation

> The full step-by-step guide ships with the script in **`docs/INSTALL.md`**, and model info is in **`docs/MODELS.md`**. Follow those for exact file names. The steps below are the general order.

## Requirements
- A framework: Qbox, QBCore or ESX (auto-detected)
- A supported inventory: g7_inventory, ox_inventory, qs-inventory, qb-inventory or ESX inventory (auto-detected)
- A target: `ox_target` or `qb-target` (auto-detected)
- An appearance system for saving looks: illenium-appearance, fivem-appearance, qb-clothing or esx_skin

## Step 1 — Add the resource
Drop `g7_beauty` into your resources folder.

## Step 2 — Install the items and images
Add the items for your inventory and copy the item images into your inventory image folder (see `docs/INSTALL.md`). Items include nail kits, nail repair kits, press-ons, acetone, makeup remover wipes, lash tweezers, lash glue, strip / cluster / individual lashes, and the placeable vanity.

## Step 3 — Create the jobs
Create the two salon jobs on your framework:
- **Nail tech**
- **Makeup artist** (also runs the Lash Bar)

## Step 4 — server.cfg
```cfg
# your framework, inventory, target and appearance system here
ensure g7_beauty
```

## Step 5 — Configure
- Place the salon stations (nails, makeup, lash bed) and front desk kiosk to fit your map.
- Set up the Beauty Supply shop location (clerk and blip).
- Turn seasonal drops on or off (owners can also do this from the dashboard).

## Step 6 — Test
1. Restart the server.
2. Visit the Beauty Supply shop and buy a nail kit.
3. Run `/nails`, `/shownails` and `/nailcheck`.
4. Set yourself to the nail tech job and run `/beautystats`.

---

Store: https://g7-development.tebex.io/ · Discord: https://discord.gg/P6XQ4N6M95

Every G7 script auto-detects its framework (Qbox, QBCore, ESX) and inventory (g7_inventory first, then ox_inventory, qs-inventory, qb/ps/lj, ESX default). Always ensure G7 scripts **after** your framework, inventory and `oxmysql`.