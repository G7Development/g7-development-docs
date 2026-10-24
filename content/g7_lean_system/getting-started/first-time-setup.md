# First-time setup and gameplay

## Open the mixing interface

Use any registered G7 Lean System item to open the interface, run `/leanmenu`, or press **F7** by default.

## Preparation flow

### Measure syrup

You need one syrup item and one empty baby bottle. Select a syrup and a line amount in the interface. The server validates the syrup key and line count, consumes the ingredients, and creates `g7_lean_bottle_filled` with syrup and line metadata.

The maximum measured bottle amount is controlled by `Config.Security.MaxBottleLines` (default `16`).

### Prepare soda

You need one soda item and one empty cup. Select the soda to create a metadata-bearing `g7_lean_cup_soda`.

### Mix a drink

You need a measured bottle, soda-filled cup, and an additional empty cup. Select a potency tier. The selected tier determines how many lines are used: Light 1, Medium 2, Strong 3, Heavy 4. The soda cup and empty cup are consumed; if the bottle has lines remaining, a new measured bottle is returned. The finished drink records syrup, soda, potency, lines, sips, and ice state.

### Add ice

Use an ice item with a valid finished drink through the interface's Add Ice action. The drink is replaced with an iced version, and the ice is consumed.

### Sip, pass, or discard

When a finished drink is active, the default controls are:

- **E:** take a sip.
- **C:** pass the drink to a nearby player.
- **X:** discard the drink.
- **G:** the current implementation displays a reminder that the drink remains saved in inventory; it does not implement a separate stash action.

The default maximum sip count is `20`, configured by `Config.Security.MaxCupSips`. When the last sip is consumed, the drink item is replaced by an empty cup.

## Persistence

There is no database requirement in the supplied resource. Recipe and sip state is carried in inventory item metadata. This relies on the selected inventory correctly preserving metadata for unique/non-stackable items.
