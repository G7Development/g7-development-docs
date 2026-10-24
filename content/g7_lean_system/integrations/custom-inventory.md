# Custom inventory adapter

Use this page when your inventory is not compatible with the supplied `ox`, `qb`, or `qs` bridge paths.

## Select custom mode

```lua
Config.Inventory = 'custom'
```

Implement the adapter functions in `Config.CustomInventory` in `config.lua`, or extend `server/bridge.lua` using your inventory's documented API.

```lua
Config.CustomInventory = {
    count = function(source, item)
        -- Return the player's numeric count for item.
    end,
    remove = function(source, item, count, metadata)
        -- Remove item/count. Return true only when successful.
    end,
    add = function(source, item, count, metadata)
        -- Add item/count with metadata. Return true only when successful.
    end,
    registerUse = function(item, callback)
        -- Register a usable item; invoke callback(source, itemData) on use.
    end
}
```

## Contract requirements

- `count(source, item)` returns a number, or a value convertible to a number.
- `remove(source, item, count, metadata)` returns `true` only after removal succeeds.
- `add(source, item, count, metadata)` returns `true` only after the item was added successfully.
- `add` must preserve supplied metadata.
- `registerUse(item, callback)` must register the usable item and call `callback(source, itemData)` when used.
- Unique/metadata-bearing items must not stack with different metadata.
- Validate item counts and metadata using the inventory's real API; do not trust client-provided state.

## Important implementation note

The current `G7Bridge` wrapper calls the custom functions from `server/bridge.lua`. Simply defining `Config.CustomInventory` is not enough unless each function is implemented and returns the required values. Test failed additions, full inventories, metadata round-trips, disconnects, and concurrent actions. Rollback attempts cannot guarantee recovery if the inventory itself errors or returns inaccurate status.
