# G7 Development Docs Website

Custom static documentation site for G7 Development, using G7 Lean System v1.0.0 as the first resource.

## Publish with GitHub + Cloudflare Pages (no local coding required)

1. Extract this ZIP on your computer.
2. Open https://github.com/new and create a repository named `g7-development-docs`. Choose Public or Private as you prefer; a public repo makes the source visible. Do not add a README, license, or .gitignore because this folder already contains files.
3. In the new repository, choose **Add file → Upload files**. Drag in the *contents* of this extracted folder (not the ZIP and not the enclosing folder). You should see `index.html`, `app.js`, `styles.css`, and `docs.json` at the repository root. Also upload `assets/` and `content/`. Click **Commit changes**.
4. Create or sign in to Cloudflare at https://dash.cloudflare.com/ .
5. Open **Workers & Pages → Create application → Pages → Connect to Git**. Connect GitHub, authorize Cloudflare, then select `g7-development-docs`.
6. Set production branch to `main`. Choose **None** or **No framework** if offered. Set **Build command** to `exit 0` and **Build output directory** to `.` (a single period). There is no build step; the website is static HTML.
7. Click **Save and Deploy**. When it finishes, Cloudflare gives you a `*.pages.dev` address. Open it and check the sidebar, page links, and search.
8. Later updates: edit/upload files to the GitHub repository and commit; Cloudflare automatically deploys changes.

Official Cloudflare guide: https://developers.cloudflare.com/pages/framework-guides/deploy-anything/

## Local preview

Because this site loads `docs.json`, preview it through a local web server rather than opening `index.html` directly. If Python is installed, open a terminal in this folder and run `python -m http.server 8000`, then visit http://localhost:8000.

## Editing documentation

Markdown source pages are in `content/g7_lean_system/`. The live site uses `docs.json`, which contains the page text and sidebar metadata. To update a page, edit the matching `markdown` content in `docs.json` or ask G7 Development to regenerate the package from the Markdown files.

## Notes

- Accent color: `#5b376a` with lighter purple highlights.
- The first version includes G7 Lean System documentation only. Other G7 scripts can be added as more sections/pages.
- Google Fonts is loaded from an external CDN; if unavailable, the site falls back to system fonts.


## Added script installation guides

The sidebar now includes a **G7 Scripts** section with installation pages for G7 Black Market Phone, G7 Black Market, G7 Drug Sales, G7 Inventory, G7 Vape System, G7 Reports V2, G7 Admin Pockets, G7 Banking, and G7 Illegal Weapon. The original G7 Lean System documentation and Discord support links are preserved.
