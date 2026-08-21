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
- [x] A workspace made yours: name, logo, OKLCH theming with secured contrast, fonts, and a phone-ready shell
- [x] Proposal from the human front: on a suggest-only mount, Update becomes Propose, landing in the same review card
- [x] Several human principals: viewers and contributors with per-mount scopes, added by the owner in Settings
- [x] The lock, down to a single page: path grants (mount, folder, or one file) and a Share button on the page itself
- [x] Content-first visibility: general access inherited downward, restrictions that go dark, people as the exceptions
- [x] Tailscale mode: run on a tailnet and the network signs people in, no password, grants untouched (see [[On your tailnet]])
- [x] Path grants for agent tokens: a token holds a mount, a folder, or one page - the same mechanism people have
- [x] `hanji export`: the everyone-visible content mirrored as `llms.txt` plus per-page Markdown, for the agents that read the open web
- [x] Changes arrive on their own: a signed push-to-sync webhook, and an opt-in poll loop for tailnets and local mounts
- [x] A first run without a terminal: the welcome walks three soft steps - source, details, name and owner - suggesting the name from what you picked, and signs you in
- [x] The publishing plane: `hanji export` renders a browsable static site - the hanji look, no JavaScript - beside the Markdown mirror. This handbook is published with it.

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
