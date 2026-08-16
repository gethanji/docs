---
title: The web front
order: 50
description: "apps/web: the Next.js human front that hides git and renders on warm paper."
---

# The web front

`apps/web` is the face a person sees. It is a Next.js application, and its entire job is to make a git-backed knowledge base feel like a calm editorial tool, with git nowhere in sight.

## What it does

- **Renders.** Pages are rendered per block through a sanitizing pipeline, so what reaches the browser is safe and clean. Wikilinks are resolved and scoped to what you can read. The result is the warm paper you are reading on.
- **Edits.** It places [[The editor]] into the page in place, and on Update it asks core to commit. A no-op change commits nothing. A collision with a newer version stops with a conflict rather than clobbering it.
- **Searches.** One FTS5 backend (bm25 with a heavy title boost, hit-marked snippets, multi-word coverage ranking) serves the `⌘K` palette, the results page, and the agents' `hanji_search` alike.
- **Organizes.** Sidebar drag & drop is optimistic: pure tree transforms apply instantly while core does the git work behind, and a failure reverts with a toast. A move is `git mv`; a reorder writes `order:` frontmatter as one commit.
- **Guards the door.** The front is single-owner today, behind a signed session. Every read it makes still goes through core's one permission check, so the front has no private path around permissions.

## What it is not

It is not the source of truth, and it is careful never to become one. It holds no content of its own. Everything it shows comes from core, which reads from git. Turn the front off, and your knowledge is exactly where it was, in your repositories. That is the point of building it this way. See [[The core]] for what it sits on, and [[The two faces]] for the design behind having a human front at all.
