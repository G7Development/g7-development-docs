# G7 Development Docs — Portal Upgrade

This is a static, GitHub Pages / Cloudflare Pages compatible documentation site. It keeps the existing G7 script guides and adds a richer portal-style homepage and front-end tools.

## Included
- Global documentation search (`Ctrl+K` / `Cmd+K`)
- Script directory cards and resource guide navigation
- Light/dark theme toggle
- Copy buttons for code blocks
- Previous/next guide navigation and reading progress indicator
- Local keyword-based “Ask G7 Docs” helper (not a live AI service)
- Interactive server setup checklist
- Guide and homepage feedback controls (demo only; feedback is not sent to a server)
- Changelog/announcement and service overview panels (static informational content)
- Responsive mobile navigation and G7 support/store links
- Customer portal concept placeholder is explained below

## Important limitations
This is a static website. It does **not** include customer authentication, purchase verification, license lookup, support-ticket storage, real-time service monitoring, server-side AI, or a database. Those require a backend or external service integration. The “Ask G7 Docs” tool searches the documentation bundled with the site using keywords; it does not make up a technical diagnosis. Feedback and checklist progress are browser-session interactions and are not submitted or stored centrally.

## Publish with GitHub Desktop
1. Download and extract the ZIP.
2. Open GitHub Desktop and select the `g7-development-docs` repository.
3. Choose **Repository → Show in Explorer**.
4. Copy the extracted files and folders into that repository folder, replacing the old files when prompted.
5. Confirm `index.html`, `styles.css`, `app.js`, `docs.json`, `assets/`, and `content/` are at the repository root.
6. Return to GitHub Desktop, enter commit summary `Upgrade G7 Docs portal UI`, click **Commit to main**, then **Push origin**.
7. Cloudflare Pages should deploy the new version automatically. Open the production URL and hard-refresh with `Ctrl+F5` if the old design is cached.

## Adding a documentation page
Add an object to `docs.json` with `slug`, `title`, `group`, `description`, and `markdown`. If you use a new markdown file, make sure the app's data-loading setup is updated to include it; current docs content is bundled in `docs.json`.
