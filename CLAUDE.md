# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Reuben Low's personal academic/portfolio site (https://ReubenLow.github.io), built on the
[al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. The theme is vendored into the repo
(not consumed as a gem), so `_layouts/`, `_includes/`, `_sass/`, and `_plugins/` are all editable —
but they are upstream files, and most day-to-day work is content in `_projects/`, `_posts/`,
`_news/`, `_pages/`, and `assets/`.

Upstream docs kept in the repo and excluded from the build: `INSTALL.md`, `CUSTOMIZE.md`, `FAQ.md`.

## Commands

Local dev via Docker (recommended — the image carries ImageMagick, nbconvert, and the gem set):

```bash
docker compose pull && docker compose up          # serves on http://localhost:8080, livereload on 35729
docker compose -f docker-compose-slim.yml up      # smaller image, same behavior
docker compose up --build                         # rebuild from ./Dockerfile after Gemfile changes
```

Native (needs Ruby + Bundler, Python + `pip install nbconvert`, and ImageMagick on PATH):

```bash
bundle install
bundle exec jekyll serve            # dev server with watch
bundle exec jekyll build            # one-shot build into _site/ (this is also bin/cibuild)
JEKYLL_ENV=production bundle exec jekyll build   # production build (minification, analytics, etc.)
```

Formatting — Prettier with the Shopify Liquid plugin is **enforced by CI** on every push/PR to
`master` (`.github/workflows/prettier.yml`), and a failing check posts an HTML diff on the PR:

```bash
npm install                 # installs prettier + @shopify/prettier-plugin-liquid
npx prettier . --check      # what CI runs, but see the CRLF caveat below
npx prettier . --write      # fix
```

`printWidth` is 150; `.prettierignore` exempts minified/vendored assets and `_posts/2015-10-20-math.md`.

**CRLF caveat on Windows.** This clone has `core.autocrlf=true`, so the working tree is CRLF while
Prettier's `endOfLine` default is `lf`. `npx prettier . --check` therefore reports _every_ file as
failing locally, whatever its real state, while CI (which checks out LF on Linux) sees the truth. To
reproduce CI for one file, check the committed content instead:

```bash
git show HEAD:<file> | npx prettier --stdin-filepath <file> --check
```

`npx prettier --write <files>` is still safe and correct: it writes LF, and Git stores LF either way,
so the resulting diff contains only real formatting changes.

There is no test suite — "passing" means the Jekyll build succeeds and Prettier is clean.

## Deployment

Push to `master` → `.github/workflows/deploy.yml` builds with `JEKYLL_ENV=production`, runs
`purgecss -c purgecss.config.js` against `_site/`, and publishes `_site/` to the `gh-pages` branch,
which GitHub Pages serves. Do not hand-edit `gh-pages`.

`bin/deploy` does the same thing locally (build → purgecss → force-push `gh-pages`); it is destructive
to the working tree — prefer letting CI deploy.

Other workflows (`axe.yml`, `broken-links*.yml`, `lighthouse-badger.yml`, `deploy-image.yml`,
`docker-slim.yml`) are upstream accessibility/link/image-publishing jobs, not part of the site build.

## Architecture

Everything is Jekyll + Liquid; there is no JS build step. Rendering flows
`_layouts/*.liquid` → `_includes/*.liquid` → `_sass/` (compiled through `assets/css/main.scss`, the
only SCSS entry point, which is Liquid-templated so it can read `_config.yml` values).

**`_config.yml` is the control panel.** Most behavior is feature-flagged there rather than coded per
page: `enable_*` toggles (masonry, math, darkmode, project categories, medium zoom, progressbar…),
`collections`, `jekyll-archives` permalinks and `display_tags`/`display_categories`, `scholar`
settings, `imagemagick` responsive-WebP generation, `external_sources` for pulling in outside posts,
and `third_party_libraries` — a pinned URL + SRI-hash registry that every `_includes/scripts/*.liquid`
reads, so adding or upgrading a front-end library means editing that block, not hardcoding a CDN tag.
Changing `_config.yml` requires restarting the server (the Docker entrypoint watches it and restarts
Jekyll automatically).

**Content collections** (`output: true`):

- `_projects/*.md` → `/projects/<name>/`. Front matter drives the grid on `_pages/projects.md`:
  `category` must be one of that page's `display_categories` (currently `[work]`, and every project
  uses `work`) or the project won't appear; `importance` sorts within a category; `img` is the card
  thumbnail. Note the page prints a heading for each entry in `display_categories` before filtering,
  so adding a category there with no projects renders an empty heading.
- `_news/*.md` → announcements on the about page (`announcements.limit` in config).
- `_posts/*.md` → `/blog/:year/:title/`, plus year/tag/category archives from `jekyll-archives`.

**Pages** live in `_pages/` (added to the build via `include: ["_pages"]`). Navbar order and
visibility come from `nav: true` + `nav_order` in front matter; `_pages/dropdown.md` shows the
submenu pattern.

**Local plugins** in `_plugins/` are Ruby hooks that run at build time — `cache-bust.rb` (the
`bust_file_cache` Liquid filter used on asset URLs), `download-3rd-party.rb` (vendors CDN libs
locally when `third_party_libraries.download: true`), `external-posts.rb` (fetches
`external_sources` RSS at build), `google-scholar-citations.rb`, `details.rb`, `file-exists.rb`,
`remove-accents.rb`.

**Media in content** goes through includes rather than raw HTML: `{% include figure.liquid %}`,
`video.liquid`, `audio.liquid` — these handle responsive WebP variants, lazy loading, and zoom.
Images referenced from `assets/img/` get 480/800/1400px WebP siblings generated by
`jekyll-imagemagick` at build time.

**CV page** (`_pages/cv.md`, layout `_layouts/cv.liquid`) has two modes: if `assets/json/resume.json`
exists it renders the [JSON Resume](https://jsonresume.org/) schema through `_includes/resume/*`;
otherwise it falls back to `_data/cv.yml` through `_includes/cv/*`. This site has `resume.json`, so
**that file is the live source** and `_data/cv.yml` is dead. `cv_pdf:` in the page front matter points
at a file in `assets/pdf/`.

Search (Ctrl/Cmd-K) is generated at build time by `_includes/scripts/search.liquid`, which walks
`site.pages`, posts, projects, and news into a `ninja-keys` dataset — new content is indexed
automatically.

## Leftover upstream demo content

The theme ships with Albert Einstein placeholder data that was never fully replaced. When touching
these areas, expect the demo values and replace rather than extend:

- `_bibliography/papers.bib` and `scholar.last_name/first_name: Einstein` in `_config.yml` — the
  publications page renders Einstein's papers.
- `assets/json/resume.json` — real name/email, but placeholder San Francisco address and
  `AlbertEinstein` social profiles.
- `_pages/profiles.md` (`about_einstein.md`), `_news/announcement_*.md`, and the 30-odd `_posts/`
  that are theme feature demos. The only authored post is `2024-07-23-sep2docs.md`.
- `site.description`, `disqus_shortname`, and `external_sources` still carry al-folio defaults.

Also note `<div class="videorow">` is used in several `_projects/*.md` but has no CSS rule anywhere —
it currently does nothing.
