---
title: Where we are
order: 120
description: What is shipped, what is next, and what we are still exploring.
---

# Where we are now

Hanji is early, and built in the open with its own tools. This page is written in Hanji, kept in Hanji's repository, and read through the product you are looking at now. It is honest to a fault, because pretending to be further along than you are is its own kind of lock-in. There are no dates here, because a solo, early project that promises dates is lying. The order is real, though, and it follows the rule in [[Mission and vision]]: earn trust first, charge for what an administrator wants, never gate what one person needs to write.

## Now, shipped and live

- [x] Byte-fidelity Markdown engine, guarded by a corpus gate
- [x] Git-backed mounts, sync, and a rebuildable index
- [x] The human front: reading, in-place WYSIWYG editing, history, nested navigation
- [x] The agent surface: MCP scoped read tools and propose-as-PR
- [x] Per-mount permissions on a single read path
- [x] The agent git flow, end to end: an agent proposes, you review the diff in the front, and merge (see [[Proposals]])
- [x] Search worth the name: a title-boosted, multi-word-aware backend under a `⌘K` palette and a full results page
- [x] The sidebar as an organizer: quick-create, drag to move, reorder, nest, and delete, all real git behind an instant UI
- [x] Per-section sorting: a folder or mount sorts its own children by date (newest or oldest, from the filename or the commit) or A-Z - a personal, per-section choice kept in the browser
- [x] Every page's paper trail: a byline of who began it and who last changed it, read from git history, shown on the page and powering the recency sort
- [x] A workspace made yours: name, logo, OKLCH theming with secured contrast, fonts, and a phone-ready shell
- [x] Proposal from the human front: on a suggest-only mount, Update becomes Propose, landing in the same review card
- [x] Several human principals: viewers, contributors, and editors with per-mount scopes, added by the owner in Settings; editors commit to base, contributors propose
- [x] The lock, down to a single page: path grants (mount, folder, or one file) and a Share button on the page itself
- [x] Content-first visibility: general access inherited downward, restrictions that go dark, people as the exceptions
- [x] Tailscale mode: run on a tailnet and the network signs people in, no password, grants untouched (see [[On your tailnet]])
- [x] Path grants for agent tokens: a token holds a mount, a folder, or one page - the same mechanism people have
- [x] `hanji export`: the everyone-visible content mirrored as `llms.txt` plus per-page Markdown, for the agents that read the open web
- [x] Changes arrive on their own: a signed push-to-sync webhook, and an opt-in poll loop for tailnets and local mounts
- [x] A first run without a terminal: the welcome walks three soft steps - source, details, name and owner - suggesting the name from what you picked, and signs you in
- [x] The publishing plane: `hanji export` renders a browsable static site - the hanji look, no JavaScript - beside the Markdown mirror. This handbook is published with it.
- [x] Fresh by the time you look: sync-on-focus, a byline freshness line, early conflict warning, a content-compare save guard, and agents that sync local mounts before every read
- [x] The freshness note carries a destination: it names the page that moved and links to it, or points at Activity when several did; a change that moved no page refreshes without a note
- [x] Images count as freshness too: a per-mount asset fingerprint makes a replaced picture a change the screen hears about, and stamps the image URLs so the browser cannot answer from its cache
- [x] The Activity page: what moved across your mounts - edits, comments, reviews - straight from the history, plus a local mount's edits not yet committed, with honest actor labels and a "seen up to here" line
- [x] The activity strip: eight weeks of days above the feed, human hands above the line, agents below, with filters over the feed by actor and kind
- [x] `hanji digest`: the day's changes and the waiting queue written into the workspace as a page - the artifact is the notification, cron is the only scheduler
- [x] The agent briefing channel: `hanji_changes` / `GET /changes` for "what moved since I last read", plus an Atom feed of the same, token-scoped like every read
- [x] Open Knowledge Format interop: any OKF bundle mounts as-is, and `hanji export --okf` emits a conformant one - typed concept docs, reserved index files, an `okf_version` root
- [x] One sticky chrome bar for reading and editing, and edit mode that swaps in place - same scroll position, same header, only the buttons and the toolbar change
- [x] A selection that stays selected: Copy (as Markdown *and* as rich text) and Comment offered on a bar, rather than the comment box taking the selection
- [x] Highlights in four curated colours, portable `==x==` for the default and `<mark class="hl-...">` for a chosen one (see [[Reading and writing]])
- [x] Page comments: a thread at the foot of every page, and anchored comments on an exact word or sentence, highlighted in the prose for every reader and preserved under "On an earlier version" when the text beneath them changes (see [[Comments]])
- [x] Presence on the page: who else is editing, named beside the byline and inside the editor, and a page that checks itself every few seconds while you look, so a conflict folds at save instead of surprising you

Reading and writing both feel like paper. That was the first thing that had to be true. See [[Features]] for the full list, and [[Changelog]] for what shipped when.

## Next, in build

The agent git flow shipped; what remains of the real test is a team living on it daily.

- [ ] **A proper team on a shared instance** - people and agents writing side by side, the proof no more code can produce

## Later, the paid seam

Everything an administrator needs to trust Hanji with a whole organization. This is where the revenue is, and it is deliberately not where the writing is.

| Area | What it adds |
|---|---|
| Hosted sync | A managed instance, for teams who would rather not run their own |
| Single sign-on | Log in with your identity provider |
| Audit | Who read and wrote what, and when, read straight from the git history |
| Hardening | The operational harnesses that keep an organization-size instance honest at scale |

## Exploring, not promised

Ideas we like and have not committed to.

- Real-time collaboration, reconciled with git as the source of truth
- Serving a folder as an installable agent skill
- A desktop shell around the web plane, once it can be distributed properly

Read [[Mission and vision]] for the why behind the order.
