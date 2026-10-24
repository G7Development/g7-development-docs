# NUI callbacks and action flow

The NUI page is declared as `web/index.html` in `fxmanifest.lua`. The browser communicates with the client Lua script through three registered NUI callbacks.

## NUI callbacks

| Callback | Input | Response / behavior |
|---|---|---|
| `close` | No required fields | Closes the UI and returns `{ ok = true }`. |
| `refresh` | No required fields | Requests `g7_lean:server:summary` through ox_lib and returns the summary table. |
| `action` | `action` string and optional `payload` table | Closes the UI, runs the matching progress/animation, then sends the action to the server if progress succeeds. Returns `{ ok = true }` to the browser. |

## Action lifecycle

1. The UI submits an action and payload.
2. The client closes the UI and runs a progress action using the configured duration.
3. For passing, the client identifies a nearby player and fills in `payload.target`.
4. The client triggers the internal server action event.
5. The server checks the cooldown, action values, ingredients, item metadata, and distance where relevant.
6. The server removes/creates items and sends notifications or effect events.

The immediate NUI response `{ ok = true }` confirms the browser callback was received; it does not guarantee the server-side inventory action succeeded. Use in-game notifications and inventory state to confirm the result.
