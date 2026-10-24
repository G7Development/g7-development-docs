# Commands and controls

## Command

| Command | Description |
|---|---|
| `/leanmenu` | Opens the G7 Lean System interface. |

The command is registered client-side and is not ACE-restricted in the supplied version.

## Key mapping

| Key | Default action |
|---|---|
| `F7` | Open interface (`leanmenu` key mapping) |
| `E` | Sip while a drink is active |
| `C` | Pass the drink to the nearest player within range |
| `X` | Discard the active drink |
| `G` | Displays a reminder; it does not currently implement a separate stow/storage action |

The keys are configurable through `Config.Controls`, except the F7 menu binding, which is registered using FiveM's key mapping for `leanmenu`. Players may be able to change the key mapping through FiveM settings.

## Passing drinks

The client finds a nearby player and sends the target server ID. The server checks that the target exists, is not the sender, and is within `Config.Security.PassDistance`. The server then transfers the drink metadata through the selected inventory adapter.
