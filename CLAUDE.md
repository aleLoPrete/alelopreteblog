# aleloprete.it — Zola Personal Site

## Key Paths
- `config.toml` — site config (base URL, taxonomies, highlight theme, social links)
- `content/_index.md` — homepage content (bio, tagline)
- `content/about.md` — about page (uses `template = "about.html"`)
- `content/posts/` — blog posts (markdown)
- `templates/` — Tera templates: base, index, page, about, section, 404, tags/
- `sass/main.scss` — all styles (light+dark mode)
- `static/` — CNAME, favicon, img/

## Stack
- Zola SSG, SASS, vanilla JS (no framework)
- Fonts: Alegreya (body/serif headings), Alegreya SC (small-caps headings/nav), Courier Prime (code)
- Syntax highlighting: base16-ocean-dark
- Tags taxonomy with feeds
- Print-inspired, monochrome (ink-on-paper) typography — no dark mode, no accent color. Design credited to [The Proportional Web](https://owickstrom.github.io/the-proportional-web/) (MIT) on the About page.

## Navigation
- Desktop: sticky top nav with inline links (Home, About), underline indicator on active link
- Mobile: fixed bottom nav (iOS Safari style), respects `env(safe-area-inset-bottom)`
- No hamburger menu — links always visible at all screen sizes

## Social Links (About page)
Configured in `config.toml` under `[extra]`:
- `github` — GitHub profile URL
- `linkedin` — LinkedIn profile URL

Rendered in `templates/about.html` below the page title.

## Deploy
GitHub Actions → static site, custom domain via `static/CNAME` → aleloprete.it
