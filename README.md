# drkbalsara.com

Static marketing site for **Dr. Balsara** — compassionate at-home pet euthanasia in Vancouver and the Lower Mainland.

Built with [Hugo](https://gohugo.io/) (extended) and deployed automatically to **GitHub Pages** via GitHub Actions.

---

## Stack

- **Hugo extended** `v0.152.2` — static site generator, with built-in SCSS/LibSass pipeline.
- **Custom in-tree theme** — layouts and SCSS live under `layouts/` and `assets/`. No external theme dependency.
- **GitHub Pages** + GitHub Actions — automatic builds and deploys on every push to `main`.

## Local development

Requires Hugo extended (`brew install hugo` on macOS, `dnf install hugo` on Fedora, or download from the [releases page](https://github.com/gohugoio/hugo/releases) — must be the extended build for SCSS).

```bash
hugo server -D
```

The site will be served at <http://localhost:1313/>. Edits are live-reloaded.

To produce a production build locally:

```bash
hugo --minify --gc
```

Output lands in `public/`.

## Project layout

```
.
|-- archetypes/         # front-matter scaffold for `hugo new`
|-- assets/scss/        # source SCSS (compiled by Hugo Pipes)
|-- content/            # markdown pages
|   `-- service-areas/  # one stub page per Lower Mainland city (local SEO)
|-- layouts/            # in-tree theme: base templates + partials + shortcodes
|   |-- _default/       # baseof.html, single.html, list.html
|   |-- partials/       # head, header, footer, schema (JSON-LD), contact-cta
|   `-- shortcodes/     # faq-list
|-- static/             # CNAME, robots.txt, favicon, /og-image.jpg (TODO)
|-- .github/workflows/  # Hugo build + Pages deploy
`-- hugo.toml
```

## Editing copy

All page copy lives in `content/*.md`. Each file has a YAML front-matter block with SEO fields:

- `title` / `metaTitle` — page heading and `<title>` (metaTitle wins for the `<title>` tag)
- `description` — meta description
- `keywords` — comma-separated array, included as `<meta name="keywords">`
- `lede` — optional short subtitle shown below the H1
- `faq` — only on `content/faq.md`; populates both the visible accordion and the `FAQPage` JSON-LD

Search the codebase for `TODO: copy` to find every block the writer should rewrite.

## Site configuration

Site-wide values (phone, email, hours, geo, service-area list, brand color) live under `[params]` in [hugo.toml](hugo.toml). Update them once and they propagate through every page, schema block, and meta tag.

## SEO

The theme bakes in:

- Per-page `<title>`, meta description, canonical, Open Graph, Twitter Card
- JSON-LD `VeterinaryCare` (subtype of `LocalBusiness`) on every page, with `areaServed` enumerating every city listed in `params.serviceAreas`
- `BreadcrumbList` JSON-LD on inner pages
- `FAQPage` JSON-LD on `/faq/` (sourced from `faq` array in front matter)
- Auto-generated `sitemap.xml`
- Geo meta (`geo.region`, `geo.placename`, `ICBM`)
- Per-city landing pages under `/service-areas/<city>/` for local long-tail queries

After launch, **submit the sitemap** at:

- [Google Search Console](https://search.google.com/search-console) — add property `https://drkbalsara.com/`, then submit `https://drkbalsara.com/sitemap.xml`
- [Bing Webmaster Tools](https://www.bing.com/webmasters)

## Deployment

A push to `main` triggers `.github/workflows/hugo.yml`, which:

1. Installs Hugo extended `0.152.2`
2. Builds with `hugo --minify --gc`
3. Uploads `public/` as a Pages artifact
4. Deploys to GitHub Pages

### Runtime behavior

Deployments are triggered by pushes to `main`. If a deployment fails, inspect the latest workflow run in GitHub Actions, fix the issue locally, and push again.

## Outstanding TODOs before launch

- [ ] Replace placeholder phone, email, and hours in `hugo.toml` (`[params]`).
- [ ] Add real Open Graph image at `static/og-image.jpg` (1200x630, JPG or PNG).
- [ ] Add `static/apple-touch-icon.png` (180x180) and `static/favicon.ico` if a richer favicon set is desired.
- [ ] Replace every `TODO: copy` block in `content/` once the writer's copy arrives.
- [ ] Confirm credentials and registration number on `content/about.md`.
- [ ] Have a lawyer review `content/privacy.md` before launch.

## License

All content (text, images, branding) is &copy; Dr. Balsara. Code is private to this repository.
