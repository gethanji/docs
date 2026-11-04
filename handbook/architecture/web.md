---
title: The web front
order: 50
description: "apps/web: the Next.js human front that hides git and renders on cool neutral paper."
---

# The web front

`apps/web` is the face a person sees. It is a Next.js application, and its entire job is to make a git-backed knowledge base feel like a calm editorial tool, with git nowhere in sight.

## What it does

- **Renders.** Pages are rendered per block through a sanitizing pipeline, so what reaches the browser is safe and clean. Wikilinks are resolved and scoped to what you can read. The result is the cool neutral paper you are reading on.
- **Edits.** It places [[The editor]] into the page in place, and on Update it asks core to commit. A no-op change commits nothing. A collision with a newer version stops with a conflict rather than clobbering it.
- **Searches.** One FTS5 engine (title, section and Hangul bigram tables, bm25 with a title boost, passage scoring, alias classes, multi-word coverage) serves the `⌘K` palette, the results page, and the agents' `hanji_search` alike. `/api/search` answers one envelope rather than an array: the hits, both counts, the mount facets, the terms that matched nothing anywhere and the titles that come closest. The palette makes four guarded kinds of request against it - the starter rows, the title tier, the full search, the typo suggestions - each with its own AbortController, so a slower answer can never paint over a newer one. The results page calls no route: it runs the same `searchResult` inline, exact pass included, because it renders on the server anyway. `/api/search/click` is a session-gated write that fills the clicked rank and path on the query just logged and answers no body. `/api/reindex` is owner-only, GET for the integrity verdict and POST for one re-derivation at a time, reporting through the same in-process status map a sync uses.
- **Organizes.** Sidebar drag & drop is optimistic: pure tree transforms apply instantly while core does the git work behind, and a failure reverts with a toast. A move is `git mv`; a reorder writes `order:` frontmatter as one commit.
- **Files issues.** `POST /api/issue` turns a reading-mode selection into an issue in one of the workspace's trackers. It loads `@hanji/tracker/ops` in the server process, the entry that never reaches Plan's agent runners, and it checks a same-origin request, read on the page, write on the work mount and, unless the reader is the owner, a `people:` key before it writes. `DELETE /api/issue` is Undo: it cancels the issue in one commit. Every refusal of scope is the same 404. A draft the tracker refuses gets a 400 with the tracker's words; any other failure is logged on the server and answers a fixed 500 that names no path.
- **Guards the door.** The front is single-owner today, behind a signed session. Every read it makes still goes through core's one permission check, so the front has no private path around permissions.

## What it is not

It is not the source of truth, and it is careful never to become one. It holds no content of its own. Everything it shows comes from core, which reads from git. Turn the front off, and your knowledge is exactly where it was, in your repositories. That is the point of building it this way. See [[The core]] for what it sits on, and [[The two faces]] for the design behind having a human front at all.

This page describes the Document web front. The separate [[Think canvas]] has its own canvas client and a durable server journal before Git checkpoints; it does not change this Document architecture.

## Activity loading

`/kb/activity` authenticates and returns its shell without opening tracker history. Its client requests `GET /api/activity`, which authenticates independently, runs the same scoped journal/session loader, and answers JSON with `Cache-Control: private, no-store` and `Vary: Cookie`. No rendered cross-user result cache is introduced. Raw Git windows retain their existing HEAD-aware cache; independent readable source discovery runs concurrently. Product, hand, actor and cursor filters are applied by the journal, never by hiding unauthorized rows in the browser. The response keeps the server’s calendar day and timezone for consistent grouping. Failed requests are retryable and a 30-second client deadline never blocks product navigation.
