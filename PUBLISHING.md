# Publishing these study notes

This pack holds the `content/` folder and the `quartz.config.yaml` for a Quartz 5 site.

## One-time setup (about 15 minutes)

1. **Create the repo from the Quartz template.**
   Go to https://github.com/jackyzha0/quartz → **Use this template** → **Create a new repository** (e.g. `study-notes`). Public or private both work with Cloudflare Pages.

2. **Clone it and install.** You need Node.js 22 or later.
   ```bash
   git clone https://github.com/<your-username>/study-notes.git
   cd study-notes
   npm i
   npx quartz create --template obsidian --strategy new --baseUrl study-notes.pages.dev
   ```

3. **Drop in this pack.** Replace the repo's `content/` folder and `quartz.config.yaml` with the ones in this zip.

4. **Preview locally.**
   ```bash
   npx quartz plugin install --from-config
   npx quartz build --serve
   ```
   Open http://localhost:8080.

5. **Push to GitHub.**
   ```bash
   npx quartz sync --no-pull
   ```

6. **Deploy on Cloudflare Pages.**
   Cloudflare dashboard → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git** → pick the repo.

   | Setting | Value |
   | --- | --- |
   | Production branch | `v5` |
   | Framework preset | None |
   | Build command | `git fetch --unshallow && npx quartz plugin install && npx quartz build` |
   | Build output directory | `public` |

   Add the environment variable `NODE_VERSION` = `22` if the build complains about Node.

7. **Fix the base URL.** Cloudflare gives you an address like `study-notes-xyz.pages.dev`. Put that into `baseUrl:` in `quartz.config.yaml` and sync again.

## Every time after that

Write or edit notes in `content/` (Obsidian works well: open `content/` as a vault), then:

```bash
npx quartz sync
```

Cloudflare rebuilds the site automatically in about a minute.

## Adding a new chapter

Copy `content/templates/Chapter Template.md` into the right folder, name it with a number prefix (`13 Something.md`), and set `title: "13 · Something"` so it sorts in order in the sidebar. The `templates/` folder is not published.
