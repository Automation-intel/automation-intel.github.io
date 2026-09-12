# automation-intel.github.io

The source for [automation-intel.github.io](https://automation-intel.github.io) —
a [Jekyll](https://jekyllrb.com/) blog published with GitHub Pages. No theme
gem, no JavaScript: the templates and the single stylesheet in this repo are
the whole site.

## One-time setup

In the repository's **Settings → Pages**, set **Source** to **GitHub Actions**.
The workflow in [`.github/workflows/pages.yml`](.github/workflows/pages.yml)
then builds and deploys the site on every push to `main`.

## Writing a post

Add a Markdown file to `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
title: "The post title"
date: 2026-09-11 08:30:00 +0000
tags: [automation, ci]
description: >-
  Optional one- or two-line summary, used for the home page excerpt
  and the page's meta description.
---

The body, in Markdown.
```

`layout: post` is applied automatically by the `defaults:` block in
`_config.yml`, so posts don't need it. The date in the filename sets the
publish date and the URL (`/YYYY/MM/DD/slug/`). Posts dated in the future are
not published until that date passes.

Work in progress can live in a `_drafts/` folder with no date in the filename;
`jekyll serve --drafts` shows it locally without publishing it.

## Previewing locally

```bash
bundle install
bundle exec jekyll serve --livereload    # http://localhost:4000
```

## Layout

| Path | What it is |
| --- | --- |
| `_config.yml` | Site title, tagline, URL, plugins, permalink style |
| `_posts/` | One Markdown file per post |
| `_layouts/` | `default`, `post` and `page` HTML skeletons |
| `_includes/` | `head`, `header`, `footer`, `post-meta` partials |
| `assets/css/main.css` | The entire stylesheet; palette lives in `:root` |
| `index.html` | Post listing |
| `tags.html` | Tag index, generated from post front matter |
| `about.md` | About page |
| `404.html` | Not-found page |

## Customising

- **Title, tagline, description, author, email** — `_config.yml`.
- **Colours and width** — the custom properties in the `:root` block of
  `assets/css/main.css`. The dark theme redefines the same properties under a
  `prefers-color-scheme` query.
- **Navigation links** — `_includes/header.html`.

## Generated automatically

`/feed.xml` (Atom, via `jekyll-feed`), `/sitemap.xml` and `/robots.txt` (via
`jekyll-sitemap`), and per-page SEO/OpenGraph tags (via `jekyll-seo-tag`).

## Custom domain

Add a `CNAME` file at the repository root containing the bare domain
(e.g. `blog.example.com`), point DNS at GitHub Pages, and update `url:` in
`_config.yml` to match.
