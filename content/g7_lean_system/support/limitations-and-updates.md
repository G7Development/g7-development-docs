# Known limitations and update checklist

## Known limitations in version 1.0.0

- No public Lua exports are declared.
- Event names and the summary callback are internal implementation details and may change.
- The `G` control currently displays a reminder; it does not implement a dedicated inventory stow action.
- Custom animation/prop assets are not included. The package uses configurable base-game fallback models and animations.
- qs-inventory compatibility depends on the installed version's exports and item format.
- ESX inventory metadata behavior may differ from the unique metadata behavior expected by measured bottles and drinks; test this before release.
- The script has not been validated against every third-party inventory build or every server configuration.
- Rollback after a failed inventory operation is best-effort, not guaranteed.

## Update checklist

1. Back up `config.lua`, item definitions, and any custom adapter changes.
2. Read the release notes for breaking changes.
3. Replace resource files with the new release while preserving approved customizations.
4. Compare the new `config.lua` with your backup; do not blindly overwrite new settings.
5. Check whether item IDs, metadata fields, events, callbacks, or dependencies changed.
6. Restart on a staging server and test syrup measuring, soda preparation, mixing, ice, sip counts, passing, discarding, and effect cleanup.
7. Confirm all inventory items remain unique where metadata is required.
8. Publish documentation updates alongside the resource release.

## Support checklist

When reporting a problem, include the resource version, framework, inventory and version, relevant sanitized console errors, the exact action performed, and whether the issue occurs on a clean test configuration. Never share server keys or private player data.
