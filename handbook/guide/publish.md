---
title: Publish to the web
order: 48
description: One command turns everything open-to-everyone into a website, llms.txt included.
---

# Publish to the web

Some of what a team writes deserves an audience: the docs, the handbook, the
pages you would happily show a customer. Hanji publishes exactly the content
you have opened to everyone - and nothing else. The lock decides what goes;
the exporter never widens it.

The site you are reading right now was made this way.

## Choose, then publish

```bash
# 1. open what should be public (a mount, or just a folder)
pnpm hanji rule handbook everyone-read

# 2. render it
pnpm hanji export ./site
```

`./site` is now a complete static website in Hanji's look: your sidebar, your
reading order, dark mode, zero JavaScript. Beside every HTML page sits the
same page as plain Markdown, plus `llms.txt` and `llms-full.txt` at the root -
so the agents reading the open web get your writing as text, not scraped
pixels.

## Put it on GitHub Pages

```bash
cd site
git init -b main && git add -A && git commit -m "Published with Hanji"
git remote add origin git@github.com:you/docs.git
git push origin main
# then: repository Settings → Pages → deploy from main
```

Publishing again is the same two commands: `hanji export` straight into that
clone (it cleans its own output and leaves `.git` alone), commit, push.

## Make it yours

A few optional settings shape the published site, set once from the CLI:

```bash
pnpm hanji set export_home_url https://your.site/     # the crumb's way home
pnpm hanji set export_home_label YourSite
pnpm hanji set export_site_label Handbook             # the crumb's right side
pnpm hanji set export_edit_base_url https://github.com/you/repo/edit/main/docs
```

With an edit base set, every published page ends with a quiet **suggest an
edit** link straight into your repository's web editor - a reader spots a
typo, clicks, and a pull request is two clicks away. The cheapest
contribution funnel there is.

## What stays private

Everything without an everyone rule. A restricted folder inside an open mount
stays dark, its pages and even its images never leave the building. If
nothing is open at all, the exporter refuses loudly rather than shipping an
empty site - you will never publish by accident.
