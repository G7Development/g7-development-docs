# G7 Reports V2 — Installation

G7 Reports is a report and ticket system for FiveM. Players send reports and chat with staff, and staff manage everything from a dashboard. This guide takes about five minutes.

## Requirements

| Requirement | Notes |
|---|---|
| FiveM server | Any recent artifact with Lua 5.4 support |
| [oxmysql](https://github.com/overextended/oxmysql) | Required. Used for the database |
| Framework | **ESX**, **QBCore**, **Qbox** or standalone. Detected automatically |

No other dependencies are needed. Fonts and icons are bundled, so the UI works without an internet connection.

## 1. Add the resource

1. Download and extract the resource.
2. Place the `g7-reports` folder in your server's `resources` directory.
3. **Keep the folder name exactly `g7-reports`.**

## 2. Edit `server.cfg`

Make sure `oxmysql` and your framework start **before** G7 Reports:

```cfg
ensure oxmysql
ensure es_extended      # or qb-core / qbx_core
ensure g7-reports
```

## 3. Give your staff access

Staff can open the panel with `/reports`. Access is granted if **any** of the following is true:

- the player has the ACE permission `g7-reports.staff`
- the player belongs to an ACE group listed in `Config.StaffGroups`
- the player has a matching framework group (ESX group, QBCore / Qbox permission)

### Recommended: permissions in `server.cfg`

Add this to your `server.cfg`:

```cfg
# Everyone in the admin group can use /reports
add_ace group.admin g7-reports.staff allow
add_ace group.mod   g7-reports.staff allow
```

To give a single person access, add their identifier to a group, or grant the permission directly:

```cfg
add_principal identifier.license:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx group.admin

# or, for one person only:
add_ace identifier.license:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx g7-reports.staff allow
```

> **Tip:** If you already use ESX groups or QBCore permissions, you don't need any ACE lines. Anyone whose group is in `Config.StaffGroups` (`admin`, `superadmin`, `god`, `mod`, `moderator` by default) already has access.

## 4. Start the server

On first start the script creates its database tables automatically. You should see this in the console:

```
[g7-reports] Database ready.
[g7-reports] V2 started — framework: esx
```

No SQL file needs to be imported. The tables created are `g7_reports` and `g7_reports_messages`.

## Commands

| Command | Who | Description |
|---|---|---|
| `/report` | Everyone | Create a report, follow your reports, chat with staff, rate the result |
| `/reports` | Staff | Dashboard, all reports, claim, close, reopen, teleport, bring, return, spectate, private notes, settings |

Command names can be changed in `Config.Commands`. Optional keybinds are available in `Config.Keybinds`. The spectate exit key defaults to **F4**.

## Configuration

Everything is in `config/config.lua`.

| Option | Default | Description |
|---|---|---|
| `Config.Framework` | `'auto'` | `'auto'`, `'esx'`, `'qbcore'`, `'qbox'` or `'standalone'` |
| `Config.Locale` | `'en'` | `'en'` or `'pt'` |
| `Config.Commands` | `report` / `reports` | Player and staff command names |
| `Config.Keybinds` | all off, spectate `F4` | Optional keys to open the panels |
| `Config.StaffGroups` | `admin, superadmin, god, mod, moderator` | Groups that count as staff |
| `Config.AcePermission` | `'g7-reports.staff'` | ACE permission that grants staff access |
| `Config.Cooldown` | 60 s, max 3 open | Anti-spam: time between reports, open reports per player, chat delay |
| `Config.Categories` | 7 categories | Report categories, each with an icon, colour and priority |
| `Config.Priorities` | low to critical | Priority labels and colours |
| `Config.Limits` | title 5-80, description 10-1000 | Text length limits |
| `Config.QuickReplies` | 5 replies | Canned replies shown to staff above the chat box |
| `Config.Options` | see file | Toggle teleport, bring, return, spectate, ratings, page size and auto-close |
| `Config.Notify` | 5 s, sound on, top-right | Default notification behaviour |
| `Config.Discord` | disabled | Webhook logging for new, claimed, closed and rated reports |

### Discord logging

```lua
Config.Discord = {
    Enabled = true,
    Webhook = 'https://discord.com/api/webhooks/...',
    BotName = 'G7 Reports',
    Avatar  = '',
    Events  = { NewReport = true, Claimed = true, Closed = true, Rated = true },
}
```

### Adding a category

```lua
{ id = 'exploit', label = 'Exploit', icon = 'bug', color = '#ffd166', priority = 'high' },
```

Then add the translated name to each file in `locales/`:

```lua
cat_exploit = 'Exploit',
```

Available icons: `flag`, `shield`, `crosshair`, `bug`, `gavel`, `user`, `life-buoy`, `message`, `car`, `help`.

### Translations

All text lives in `locales/en.lua` and `locales/pt.lua`. To add a language, copy `en.lua`, rename it (for example `es.lua`), change the table key to `Locales['es']`, translate the values, and set `Config.Locale = 'es'`.

## Customising notifications and permissions in code

`editable/exports.lua` lets you plug G7 Reports into the rest of your server without touching the core files:

- `G7.Notify(...)` - use ox_lib, esx_notify or any notification system
- `G7.IsStaff(src)` - your own staff check (return `true`, `false`, or `nil` for the default logic)
- `G7.OnEvent(event, data)` - runs on `created`, `claimed`, `unclaimed`, `closed`, `reopened` and `rated`

## Troubleshooting

**`/reports` says I don't have permission**
Check that your ACE line is in `server.cfg`, that you restarted the server (or ran `refresh` then `restart g7-reports`), and that the group name matches.

**Database errors on start**
Make sure `oxmysql` starts before `g7-reports` and that its connection string is correct.

**The panel doesn't open or looks unstyled**
Confirm the folder is named `g7-reports` and that the `files { ... }` block in `fxmanifest.lua` is unchanged. Press F8 in game to check for NUI errors.

**Toast notifications use the default position**
Staff can change position, sound and duration in the panel under **Settings**. These are saved to `g7_settings.json` in the resource folder, so the server needs write access to it.

**Spectate says the player is offline**
The reporter must be online and streamed in. Spectate waits up to 5 seconds for their character to load.

## Updating

1. Back up `config/config.lua` and `g7_settings.json`.
2. Replace the resource folder with the new version.
3. Restore your config and settings files.
4. Restart the resource with `restart g7-reports`.

Database tables are preserved between updates.

## Support

If you need help, contact us through our support channel.
