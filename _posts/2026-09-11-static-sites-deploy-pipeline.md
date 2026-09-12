---
title: "Why a static site is the easiest thing you'll ever deploy"
date: 2026-09-11 08:30:00 +0000
tags: [automation, ci, jekyll]
description: >-
  Static site generators turn a deploy into a build artifact copy. Here's
  what that buys you, and the handful of places it stops being enough.
---

Every deployment problem is easier when the thing being deployed is a folder
of files. No process to restart, no database migration to sequence, no
connection pool to drain. The build produces a directory; something copies it
somewhere; the change is live.

## The pipeline, in full

For this site, the entire deploy is a GitHub Actions workflow that does three
things:

1. Check out the repository.
2. Run `jekyll build`, which writes `_site/`.
3. Upload `_site/` as a Pages artifact and deploy it.

That's it. The interesting property isn't that it's short — it's that every
step is *idempotent and stateless*. Re-running the workflow on the same commit
produces the same site. There's no partial state to reconcile if step three
fails halfway.

## What you get for free

| Concern | How it disappears |
| --- | --- |
| Rollback | Re-run the workflow on the previous commit |
| Scaling | It's a CDN serving files |
| Security patching | No server-side runtime is exposed |
| Staging | A pull request preview is another build of the same input |

The security point is worth dwelling on. A dynamic blog engine has a login
form, a database and an upload path — three things that need patching
forever. A static site has none of them. The attack surface is the build
pipeline and the hosting provider, and the build pipeline only runs on commits
you merged.

## Where it stops being enough

Static generation is not a universal answer, and the boundary is fairly
crisp. You need something dynamic once you need **per-request state**:

- **Per-user content.** Anything behind a login.
- **Write paths.** Comments, likes, form submissions. These can be delegated
  to a third-party service or a small function, but they're no longer static.
- **Search over a large corpus.** Fine as a client-side index up to a few
  thousand documents; painful beyond that.
- **Content that changes without a commit.** If a non-technical editor needs
  to publish at 2am, "open a pull request" is the wrong interface.

Notice that none of those are about traffic volume, which is the reason people
usually reach for a dynamic stack. Volume is the one thing static handles
best.

## The rebuild-time trap

The one real scaling limit is build time, and it bites sooner than expected.
Most generators re-render the whole site on every change, so build time grows
roughly linearly with page count:

```bash
# A useful thing to watch as a site grows
time bundle exec jekyll build --profile
```

`--profile` prints per-template render times, which is usually enough to find
the one include being rendered inside a nested loop. A site of a few hundred
pages builds in seconds. At tens of thousands, you start caring about
incremental builds — and at that point the honest answer is that you've
outgrown the simple version, and it's worth being deliberate about what you
move to instead.

Until then: a folder of files, copied somewhere. It's hard to beat.
