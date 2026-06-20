# CLAUDE.md — working notes for editing this site

James Hu's personal site + blog (https://ljameshu.com). Read this before making changes.

## What this is & how it ships
- A **Jekyll** site (heavily customized Mediumish theme) — portfolio + blog.
- Built by **GitHub Pages' native Jekyll build**. Source lives at the **root** of the `master` branch; GitHub rebuilds on every push. Custom domain via root `CNAME` (`ljameshu.com`).
- **Never commit built output.** `_site/` is the local build dir and is gitignored. (The old `docs/` folder — where the site used to be built locally and committed — is gone. Don't bring it back.)
- To publish: commit → push → merge to `master` (PR or direct). GitHub rebuilds within ~1 min.

## Build / preview locally (Windows)
- Ruby 3.3 is installed at `C:\Ruby33-x64`. Prepend `C:\Ruby33-x64\bin` to `PATH`, then:
  - `bundle exec jekyll build` → `_site/`
  - `bundle exec jekyll serve` → http://localhost:4000
- The `github-pages` gem pins the exact stack GitHub runs (Jekyll 3.10.x), so a clean local build == what GitHub will publish.
- Set `JEKYLL_ENV=production` for a production-identical build (enables the Google Analytics snippet). `repository: LJamesHu/LJamesHuSite` is in `_config.yml` so the github-metadata plugin doesn't error locally.

## GitHub Pages constraints — IMPORTANT
The native build only allows an approved plugin list. **Do not reintroduce `jekyll-paginate-v2` or `jekyll-archives`** — they were removed precisely because GitHub rejects them. In their place:
- **Pagination** = `jekyll-paginate` **v1**. It paginates `blog/index.html` → `/blog/`, `/blog/page2/`, … via `paginate` + `paginate_path` in `_config.yml`. The numbered pager is hand-built in `_includes/pagination.html`.
- **Category archives are manual pages** in `categories/` — one file per category, no plugin generating them.

If anything you build would need a non-approved plugin, extend the v1 / manual approach instead, or the site stops building on GitHub.

## Repo map
- `_posts/` — posts (`YYYY-MM-DD-slug.md`).
- `_layouts/` — `default.html` (page shell: nav, header, footer, GA, script tags), `post.html`, `page.html`, `category.html`.
- `_includes/` — `postbox.html` (list/grid post card), `featuredbox.html` (home featured card), `pagination.html`, `share.html`, `disqus.html`.
- `assets/css/screen.css` (bulk theme CSS), `assets/css/main.scss` (small custom overrides, compiled to `main.css`), `_sass/_syntax.scss` (code highlighting). Bootstrap 4 + FontAwesome load via CDN in `default.html`.
- `categories/` — manual category pages. `index.html` (home), `blog/index.html` (paginated list), `resume.md`, `404.md`, `feed.xml`.
- `_config.yml` — site config; `CLAUDE.md`, `README.md`, `Gemfile*`, `vendor` are excluded from the build.

## Workflow: add a blog post
1. Create `_posts/YYYY-MM-DD-<slug>.md` with front matter:
   ```
   ---
   layout: post
   title:  "Title Here"
   categories: [ technical ]          # existing: technical, python, business, ai
   image: assets/images/<file>.png    # optional; put the file in assets/images/
   featured: true                     # optional; shows it in the home "Featured" row
   comments: true
   ---
   ```
   Write the body in James's voice — see **Writing style** below.
2. **New category? Add its page.** Category pages are manual. If a category has no file in `categories/`, create `categories/<name>.html`:
   ```
   ---
   layout: category
   title: <name>
   category: <name>
   permalink: /<name>/
   ---
   ```
   Posts link to `/<category>/`, so the permalink must match the slugified category name.
3. **Featured logic:** the home page shows every post with `featured: true` in a 2-wide grid (usually ~4). To feature a new post "instead of X," also remove `featured: true` from X.
4. **Images:** drop the file in `assets/images/`. Note the *featured card* and *post-page* images use absolute (production) URLs, so a brand-new image looks blank in local preview until it's live — expected, not a bug. List/grid cards use relative URLs and render locally.
5. **Verify:** `bundle exec jekyll build`, then confirm the post built at `_site/YYYY/MM/DD/<slug>.html`, shows on `/blog/` and its category page(s), and (if featured) on the home Featured row. Ideally `jekyll serve` + screenshot.
6. **Publish:** commit → push → merge to `master`. Live at `https://ljameshu.com/YYYY/MM/DD/<slug>.html`.

## Workflow: rework / redesign the site
- **Shell & nav:** `_layouts/default.html` is the entire page chrome (nav bar, site title, footer, GA, scripts). The home composition is `index.html`.
- **Styling:** big changes → `assets/css/screen.css`; small overrides → `assets/css/main.scss` (Sass, compiled by Jekyll); code blocks → `_sass/_syntax.scss`.
- **Post components:** `_includes/postbox.html` (list cards) and `_includes/featuredbox.html` (featured cards).
- **Stay within GitHub Pages plugins** (see constraints). Don't add disallowed plugins.
- **Always verify before publishing:** build with the `github-pages` gem locally and screenshot the affected pages, then commit → push → merge to `master`.

## Writing style (James's voice — match this when drafting posts)
Synthesized from all posts written before mid-2026. The goal is to sound like him, not like AI marketing copy.

- **Voice:** first person, conversational, candid, lightly self-deprecating. Reads like a sharp friend explaining something he built — he's a data scientist, not a "blogger."
- **Open with a hook or blunt thesis:** *"Job hunting is terrible."* / *"Reddit tends to be a highly liberal place…"* / *"Profile sites are basically a person's way of…"*
- **Opinionated, willing to be contrarian** — questions received wisdom and backs it with his own analysis (and says when the data is weak).
- **Intellectually honest about limitations.** Reports real accuracy numbers, poor regression fits, shaky assumptions, "take it with a grain of salt." Never oversells. *Keep this honesty — it's central to his voice.*
- **Pragmatic, outcomes-focused.** Frames projects as "I had this annoying manual problem, so I built X," and emphasizes what was actually useful.
- **Quantify.** Concrete numbers, dollars, percentages, counts (`**$1,645 across 51 donations**`, "roughly 17%", "30,000 articles"). Bold the standout figures.
- **Name tools plainly:** Python, BeautifulSoup, Selenium, matplotlib, Tableau, Monte Carlo, Doc2Vec, Bootstrap, etc. Assumes a technically literate reader; explains concepts briefly when needed.
- **Structure:** motivation/context → approach (tools) → results → honest caveats → links. Longer posts use `##`/`###` headings; many are just a few flowing paragraphs.
- **End with resource links:** GitHub repos, downloadable data/PDFs, and cross-links to his own posts via `{% post_url YYYY-MM-DD-slug %}`. Embed PDFs with `<object data="{{site.url}}{{site.baseurl}}/assets/files/…">`.
- **Rhythm:** mixes longer explanatory sentences with short punchy ones and the occasional fragment (*"Didn't make enough sense so I did a simple investigation."*). Comfortable using "…" for a beat. Casual connectors: "So", "Additionally", "Tied to that", "Ultimately".
- **Length:** short-to-medium. Concise. Don't pad.
- **Avoid:** AI-marketing tone, emoji spam, over-explaining, and claiming more than the work supports. When in doubt, be plainer and more honest.
