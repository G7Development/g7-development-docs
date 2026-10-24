# Troubleshooting

## The resource does not start

- Confirm the folder is named `g7_leansystem`.
- Ensure `ox_lib` and the framework/inventory start before the resource.
- Check for Lua syntax errors in the server console.
- Confirm `fxmanifest.lua` is present in the resource root.

## Wrong framework or inventory is detected

Set `Config.Framework` and `Config.Inventory` explicitly. Auto detection checks Qbox before QBCore, and ox_inventory before qs-inventory and qb-inventory. When multiple resources are running, detection may not select the one players actually use.

## Items do not appear or cannot be used

- Confirm all item IDs from `install/inventory/items.lua` or `qb-items.lua` were merged into the active registry.
- Restart the relevant framework/inventory resource or server as required by your inventory.
- Check spelling and case of each item ID.
- Verify the chosen inventory exposes the API used by the bridge.

## Drink state or recipe is lost

Ensure `g7_lean_bottle_filled`, `g7_lean_cup_soda`, and `g7_lean_drink` are unique/non-stackable. Verify the inventory preserves metadata. For custom inventory formats, adapt `itemMeta()` in `server/main.lua` and test a full round-trip: create, move, relog if applicable, then use.

## UI opens but actions fail

- Check the server console for `[G7 Lean System] rejected action` messages.
- Confirm the player has the exact required ingredients.
- Check that the item metadata has not been modified by another resource.
- Verify inventory add/remove calls return success correctly.
- Test with default configuration and no unrelated inventory overrides.

## Inventory-full messages or missing ingredients after an error

The script attempts to restore consumed ingredients when an add operation fails. Rollback is best-effort and cannot guarantee recovery if the inventory API returns incorrect results, throws during an outage, or behaves differently than expected. Reproduce on a staging server and inspect inventory logs before production.

## Effects do not clear or look incorrect

Confirm `Config.Effects.Enabled` is true, test with default effect settings, and verify the resource is not being stopped unexpectedly. The resource clears common screen/camera/movement effects when stopped; custom effects added by modifications may need their own cleanup.

## Props or animations do not play

The included resource uses base-game fallback models and animation dictionaries. If you configure custom assets, confirm the files are installed, streamed, and licensed for use; then verify the model/dictionary/clip names exactly match the asset.
