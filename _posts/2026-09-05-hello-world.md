---
title: "Hello, world — and how this blog is put together"
date: 2026-09-05 09:00:00 +0000
tags: [meta, jekyll]
description: >-
  A short tour of the site: where posts live, how the templates fit together,
  and what to edit when you want to change something.
---

This is the first post on Automation Intel. Rather than the usual placeholder
text, here is a map of the site itself — useful the next time you want to
change something and can't remember where it lives.

## Where everything lives

```text
.
├── _config.yml          # site title, URL, plugins, permalink style
├── _posts/              # one Markdown file per post
├── _layouts/            # default / post / page HTML skeletons
├── _includes/           # header, footer, <head>, post meta partials
├── assets/css/main.css  # the entire stylesheet
├── index.html           # the post listing
├── about.md             # the About page
└── tags.html            # tag index, generated from post front matter
```

There is no theme gem. Everything the site renders is in this repository,
which means nothing can change underneath you on someone else's release
schedule.

## Adding a post

Create a file in `_posts/` named `YYYY-MM-DD-some-slug.md`. The date in the
filename is what Jekyll uses to order and date the post, and the front matter
block at the top supplies the rest:

```yaml
---
title: "A title with any punctuation you like"
date: 2026-09-05 09:00:00 +0000
tags: [automation, ci]
description: >-
  Optional. Used for the excerpt on the home page and for the
  social/meta description.
---
```

Everything below the closing `---` is ordinary Markdown. The `layout: post`
line most tutorials tell you to add is already applied automatically by the
`defaults:` block in `_config.yml`, so posts don't need to repeat it.

## Changing the look

The whole design is one stylesheet, and the palette sits in the `:root` block
at the top of `assets/css/main.css` as custom properties:

```css
:root {
  --accent: #0b6bcb;
  --wrap: 44rem;
}
```

Change `--accent` and every link, tag and focus ring follows. The dark palette
is the same set of properties redefined under a
`@media (prefers-color-scheme: dark)` query, so both themes stay in step.

## Previewing locally

If you have Ruby installed:

```bash
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>. Drafts can go in a `_drafts/` folder —
they need no date in the filename, and `--drafts` includes them in the
preview without publishing them.

That's the whole system. The next post gets into something more interesting.
