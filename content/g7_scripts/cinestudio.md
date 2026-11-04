# G7 CineStudio v3.0.0
**Cinematic photo & camera studio for FiveM**, by **G7 Development**

Fly a cinema camera anywhere around you, dial in the lens, color and world, pose your character or style your car, then hit **H** to hide *everything* and grab the frame with your desktop screenshot tool.

Store: https://g7-development.tebex.io/ · Support: https://discord.gg/P6XQ4N6M95

---

## Features

**Camera**
- Smooth freecam with acceleration, mouse smoothing, roll (Z/X), fast/slow modifiers
- Three modes: **Freecam**, **Orbit** (circle a subject, optional turntable auto-spin), **Follow** (camera locks onto a player, NPC or moving vehicle)
- **Track subject**: free camera that always keeps the subject in frame, with an aim-height offset
- 10 one-click quick angles (3/4 hero, front, back, sides, low, high, top-down, close-up, wide), sized automatically for peds or vehicles
- Precise pitch / roll / yaw inputs, level horizon, flatten, look at me, reset, copy camera coords (`vector3` for devs)
- Streams the world around the camera so far shots load properly
- Max range around the player (staff get extended range)

**Lens**
- FOV with real focal-length readout (mm) and 14–200mm presets
- Depth of field with **subject autofocus**, **center autofocus** or manual focus, plus an aperture (f-stop) control
- Camera shake styles (handheld, drift, road, vibrate, wind) with intensity, motion blur

**Look**
- 28 filters (editable in config) with strength, plus a stackable grade layer (contrast, saturation, brightness, vignette…)
- Aspect-ratio bars (2.39:1, 2:1, 1.85:1, 16:9, 4:3, 1:1, 4:5, 9:16) that stay in clean shots
- Vignette and film grain overlays
- Composition guides: thirds, golden ratio, both, center, diagonal

**World** (local, only the photographer sees it)
- Time-of-day override with presets (dawn, golden hour, dusk…)
- Weather override (14 types, snow tracks for snowy weather)
- Hide other players, hide yourself, hide NPCs, hide traffic, minimap toggle

**Subject, pose & vehicle**
- Lock onto yourself, your/nearest vehicle, or anything under the center ring
- **Pose mode (G)**: walk your character into place while the camera stays parked
- 16 poses, 12 facial expressions, body-heading slider, face camera, head follows lens, freeze in place, holster weapon
- Vehicle styling: each door / all doors, engine, lights (low/high), interior light, hazards, neon, wash, steering angle

**Clean capture**
- **H** hides the panel, HUD, hints, radar, crosshair and GTA notifications
- Optional countdown (3/5/10s) and auto-return (5/10/20s)
- Camera locks while clean so a bumped mouse can't ruin the shot
- `Config.Clean.OnHide / OnShow` hooks to hide your own HUD resources

**Paths & library**
- Keyframe camera paths (ENTER to add), per-segment timing, Catmull-Rom smoothing, ease in/out, loop, speed, clean playback for recordings
- Save **Shots** (exact frame), **Looks** (lens + color + frame presets) and **Paths**; rename and delete
- Stored on the player's PC (KVP, no database) or server-side with oxmysql

**Framework**
- Auto-detects **Qbox, QBCore, ESX** or standalone
- Auto-detects **g7_inventory, ox_inventory, qs-inventory, qb-inventory** (only used for the optional camera item)
- Access: everyone, ACE, staff groups or jobs (e.g. a news job)
- Blocks while dead / cuffed, closes on death, flags `invBusy` / `inv_busy` so inventories don't open over the camera

---

## Installation
1. Drop `g7_cinestudio` in your resources folder.
2. Add `ensure g7_cinestudio` to `server.cfg` (after your framework).
3. Put your `g7.png` logo in `web/assets/` (falls back to the G7 mark if missing).
4. Open `config.lua` and set access, limits and theme.
5. Restart the server.

### Optional camera item
1. Add the item from the `install/` folder for your inventory (g7/ox, Qbox, qb, qs or ESX SQL).
2. Copy `install/images/cinecamera.png` into your inventory's image folder.
3. Set `Config.Item.Enabled = true` (and `RequiredForCommand = true` if `/cine` should need the item).

### Database storage (optional)
Default storage is `kvp` (saved on each player's PC, nothing to install).
For server-side storage set `Config.Storage = 'database'`, make sure **oxmysql** is started. The table is created automatically (or run `sql/g7_cinestudio.sql`).

### Access examples
```cfg
# Config.Access.Ace.Enabled = true
add_ace group.admin g7.cinestudio.use allow
add_ace group.vip g7.cinestudio.use allow

# extended camera range
add_ace group.admin g7.cinestudio.staff allow
```

---

## Controls
| Action | Key |
|---|---|
| Open / close | `/cine` (optional `Config.OpenKey`) |
| Move | W A S D |
| Up / Down | E / Q |
| Fast / Slow | SHIFT / CTRL |
| Roll / reset roll | Z X / R |
| Zoom (orbit: distance, ALT+scroll = zoom) | Scroll |
| Look while the panel is open | Hold right mouse |
| Panel ↔ fly | TAB (ESC also leaves the panel) |
| Clean capture | H |
| Pose mode | G |
| Add path keyframe | ENTER |
| Exit | BACKSPACE |

All controls are in `Config.Controls`. Update `Config.KeyLabels` to match if you change them.

> Depth of field needs **Post FX: High/Ultra** and **DOF** enabled in GTA graphics settings.

## Exports (client)
```lua
exports.g7_cinestudio:Open()
exports.g7_cinestudio:Close()
exports.g7_cinestudio:IsOpen()
exports.g7_cinestudio:IsClean()   -- true while all UI is hidden
```
State bag: `LocalPlayer.state.g7_cinestudio` is `true` while the studio is open (handy for HUD scripts).

---


