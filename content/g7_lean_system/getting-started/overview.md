# Overview and requirements

G7 Lean System adds an in-game interface for preparing fictional roleplay drinks. Recipe state is stored on inventory items as metadata, so a measured bottle can retain its syrup type and remaining lines, and a finished drink can retain its recipe, potency, ice state, and remaining sips.

## Features in the package

- G7-branded NUI interface.
- Five syrup profiles and two soda profiles.
- Measured syrup bottles and soda-filled cups.
- Four potency tiers: Light, Medium, Strong, and Heavy.
- Ice state and per-drink sip tracking.
- Configurable screen, camera, movement, ragdoll/blackout, and optional armor effects.
- Configurable prop models, animation dictionaries, and animation clips.
- Framework and inventory bridge code.
- Included inventory item examples and PNG inventory icons.

## Requirements

- A FiveM server running GTA V.
- `ox_lib` started before this resource.
- A supported framework integration: QBCore, Qbox, or ESX.
- A supported inventory: ox_inventory, qb-inventory, a compatible qs-inventory build, or a custom adapter.
- All G7 items registered in the active inventory.

The bridge has a fallback detection value named `standalone`, but the supplied resource does not implement a complete standalone inventory/framework. Treat QBCore, Qbox, and ESX as the framework options unless you build and test your own integration.

## Resource layout

```text
g7_leansystem/
├── client/main.lua
├── server/bridge.lua
├── server/main.lua
├── shared/items.lua
├── config.lua
├── fxmanifest.lua
├── install/inventory/
│   ├── items.lua
│   ├── qb-items.lua
│   └── images/
└── web/
    ├── index.html
    ├── style.css
    ├── app.js
    └── assets/g7-logo.png
```

The `stream/animations/` and `stream/props/` directories are asset locations; the package uses configurable base-game fallback props/animations and does not include custom `.ycd`, `.ydr`, or `.ytyp` assets.
