---
title: Where we are
order: 120
description: What is shipped, what is next, and what we are still exploring.
---

# Where we are now

Hanji is a research lab exploring how people interact with agents. The current workspace is a working result of that research, shaped by an individual need and continued iteration. [[Mission and vision]] explains the purpose; this page describes what can be used today and what is still being explored.

## Public handbook

The public handbook has a product-oriented start page, grouped navigation, local page-title search and section links with an on-page outline. A compact header keeps site identity and theme controls together. Previous/next links follow the published guide order; Markdown sources remain available. Reading, section links and collapsible navigation work without JavaScript. Search is a client-side filter of exported public navigation, not a search into the live or private workspace.

## Current product

Hanji is an invited Mac desktop preview with three connected surfaces:

- **Think**: live canvases for notes, diagrams, tables and shared workshops. Selected notes can become issues or a written page.
- **Plan**: file-backed issues, projects, dependencies, objectives and key results, with list, board and canvas views.
- **Document**: Markdown reading and editing, scoped search, comments, history, proposals and selective publishing.

The conductor carries a standing agent conversation across the workspace. Agent use is optional and requires your own supported provider setup. Workspace insights show recorded activity across products.

[[What Hanji is]] explains the whole workspace. [[From idea to action]] follows a fictional decision across the three surfaces; [[Using Hanji]] maps the detailed guides. [[Mission and vision]] distinguishes our larger ambition from the preview available today. The [[Changelog]] records individual interface changes.

## Availability and limits

The handbook is public; the app is shared by invitation. There is no unrestricted public binary download or hosted workspace service. The preview runs on your machine or infrastructure you operate. Direct repository access is outside Hanji's permission boundary, and storage and permission models differ by surface. Back up durable configuration and journals as well as repositories.

The public site and handbook publish separately. Product figures use fictional workspaces rendered by the real application; illustrative workflows are not customer results.

## Implemented in the source and desktop preview

- [x] Simplified shared chrome: one search and shortcut entry per view, border-light list/board/objective layouts, and spacing-led drawer and conversation sections.

- [x] Refined bottom ask composer and issue drawer: an integrated workspace thread without page navigation, a configurable conductor shape, segmented agent selection, compact metadata, balanced comments, and consistent sub-issue status indicators. Canvas trace controls sit at the top; the chat shares the zoom-control baseline.

- [x] Hanji Plan (early) with drawer editing, searchable property menus, comment submission feedback, and collapsible history: sortable list columns, board, and canvas, visible saved-view navigation with consistent icons and a searchable persistent icon picker and inline view renaming, compact drawer controls, nearly opaque canvas cards with relationship highlights and semantic zoom with automatic fixed-footprint detail and sharp settled zoom and discreet status silhouettes, balanced board/canvas card layouts and aligned blocking icons, compact group widths, draggable sections with remembered browser-local positions and reset, editable project frames with creation and idea capture plus visible-card bulk edits, direct curved dependencies beneath cards with persistent endpoint markers with consistent drag/rest and bundle routing, inspectable outside blockers, and drawer-aware framing, view context and work counts, issue pages with a return path, and a command menu; optional dependency lanes, direct/full-chain tracing, pin/fit controls, outside-view prerequisite cards, and cycle handling; unified collapsible navigation with compact reversible slide-and-fade motion and view controls, a cool neutral light/dark palette with neutral primary buttons and send actions, and the workspace's own accent on its cool neutral paper, with the workspace accent for in-progress status and fixed colors for other semantic statuses and OKR health and tested text contrast, an integrated canvas arrangement switch and compact docked trace feedback with stable selection ownership and reversible entry/exit motion, and a shortcut reference with field and drawer navigation commands

- [x] Byte-fidelity Markdown engine, guarded by a corpus gate
- [x] Git-backed mounts, sync, and a rebuildable index
- [x] The human front: reading, in-place WYSIWYG editing, history, nested navigation
- [x] The agent surface: MCP scoped read tools and propose-as-branch (a pull request where GitHub is connected)
- [x] Per-mount permissions on a single read path
- [x] The agent git flow, end to end: an agent proposes, you review the diff in the front, and merge (see [[Proposals]])
- [x] Proposals on local mounts: a proposal is a branch of the folder's own repository, reviewed and merged in the same front; proposing and rejecting never touch your checked-out branch or edits; Merge fast-forwards the base in your folder only when it has no uncommitted edits to tracked files, and moves the base's ref alone when the base is checked out nowhere (see [[Proposals]])
- [x] Search worth the name: passage-level retrieval with a workspace-wide title tier, Hangul bigrams and alias classes, under a `⌘K` palette and a results page with exact counts, mount chips and a result that lands you on the passage
- [x] The agent retrieval contract: a search envelope with matched and missing terms, section anchors on `hanji_get_page`, a cursor on `hanji_list_pages`, a capped `hanji_changes`, and `hanji_links` over the page graph
- [x] The sidebar as an organizer: quick-create, drag to move, reorder, nest, and delete, all real git behind an instant UI
- [x] Per-section sorting: a folder or mount sorts its own children by date (newest or oldest, from the filename or the commit) or A-Z - a personal, per-section choice kept in the browser
- [x] Every page's paper trail: a byline of who began it and who last changed it, read from git history, shown on the page and powering the recency sort
- [x] A workspace made yours: name, logo, OKLCH theming with secured contrast, fonts, and a phone-ready shell
- [x] Proposal from the human front: on a suggest-only mount, Update becomes Propose, landing in the same review card
- [x] Several human principals: viewers, contributors, and editors with per-mount scopes, added by the owner in Settings; editors commit to base, contributors propose
- [x] The lock, down to a single page: path grants (on a pages mount: mount, folder, or one file) and a Share button on the page itself
- [x] Content-first visibility: general access inherited downward, restrictions that go dark, people as the exceptions
- [x] Mount kinds and references: Document keeps to pages mounts, a tracker registers as a work mount, and `[[HNJ-12]]` and Cited by follow each reader's grants (see [[Links and references]])
- [x] Tailscale mode: run on a tailnet and the network signs people in, no password, grants untouched (see [[On your tailnet]])
- [x] Path grants for agent tokens: on a pages mount a token holds a mount, a folder, or one page - the same mechanism people have
- [x] `hanji export`: the everyone-visible content mirrored as `llms.txt` plus per-page Markdown, for the agents that read the open web
- [x] Changes arrive on their own: a signed push-to-sync webhook, and an opt-in poll loop for tailnets and local mounts
- [x] A first run without a terminal: four named setup stages, automatic fresh folders, optional imports, an owner password and a final review, then signed-in access
- [x] The publishing plane: `hanji export` renders a browsable static site - the hanji look, no JavaScript - beside the Markdown mirror. This handbook is published with it.
- [x] Fresh by the time you look: sync-on-focus, a byline freshness line, early conflict warning, a content-compare save guard, and agents that sync local mounts before every read
- [x] The freshness note carries a destination: it names the page that moved and links to it, or points at Activity when several did; a change that moved no page refreshes without a note
- [x] Images count as freshness too: a per-mount asset fingerprint makes a replaced picture a change the screen hears about, and stamps the image URLs so the browser cannot answer from its cache
- [x] The Activity page: the workspace's journal of what moved in Document, Plan and Think and what agents did, by day, straight from the history, plus a local mount's edits not yet committed, with honest actor labels and a "seen up to here" line
- [x] The activity strip: eight weeks of days above the journal, human hands above the line, agents below, with filters by product, hand and kind of hand as links
- [x] A concurrent edit arrives early: while the editor sits idle, a cleanly folding change is merged into the open buffer and the caret is put back, instead of waiting at the save
- [x] `hanji digest`: the day's changes and the waiting queue written into the workspace as a page - the artifact is the notification, cron is the only scheduler
- [x] The agent briefing channel: `hanji_changes` / `GET /changes` for "what moved since I last read", plus an Atom feed of the same, token-scoped like every read
- [x] Open Knowledge Format interop: any OKF bundle mounts as-is, and `hanji export --okf` emits a conformant one - typed concept docs, reserved index files, an `okf_version` root
- [x] One sticky chrome bar for reading and editing, and edit mode that swaps in place - same scroll position, same header, only the buttons and the toolbar change
- [x] A selection that stays selected: Copy (as Markdown *and* as rich text) and Comment offered on a bar, rather than the comment box taking the selection
- [x] Highlights in four curated colours, portable `==x==` for the default and `<mark class="hl-...">` for a chosen one (see [[Reading and writing]])
- [x] Page comments: a thread at the foot of every page, and anchored comments on an exact word or sentence, highlighted in the prose for every reader and preserved under "On an earlier version" when the text beneath them changes (see [[Comments]])
- [x] Presence on the page: who else is editing, named beside the byline and inside the editor, and a page that checks itself every few seconds while you look, so a conflict folds at save instead of surprising you

Reading and writing both feel like paper. That was the first thing that had to be true. See [[Features]] for the full list, and [[Changelog]] for what shipped when.

## Plan in the current preview

Hanji Plan's conversations are reworked: a quiet conductor chosen on Plan's Agents page, one conversation per person over the workspace's sessions database, one panel across the three products, one composer everywhere, honest session states. A cleanup pass then made Plan faster, put its styles on one layered design system, and brought it to WCAG 2.2 AA. All of it is local for review; live Claude and GPT sessions have not been exercised by these checks. See [[Features]] for the details.

On a tailnet, Plan now signs teammates in: view on the tracker's work mount opens it, edit with a `people:` entry lets them edit as themselves, and actions that run an agent stay with the owner. See [[On your tailnet]]. In Document, **Issue** on the selection bar files a selected sentence as a Plan issue that cites the page and its heading (see [[Comments]]); in Think, **Create issue** files stickies as issues that cite the board (see [[Think canvas]]), and an issue's page shows where it came from under **From**. In Think, **Write up** turns a section or a selection into a Document page, or a proposal, that names the board it came from. Agents in Plan hold their own Document identity, a token named `agent-<handle>`: they read the knowledge base as it, a Claude session proposes through it, and a project's **Docs**, **Draft a doc** and **Draft a status** live on its group in a view's canvas. A workspace can hold several trackers, each with its own prefix and grants, and Plan, Document and Think reach them all (see [[Mounts]]). A harmony pass then put the three products on one neutral palette (Plan's), took the marks out of their chrome, gave them one product switcher, moved Settings out of Document's shell into a workspace page of its own (with a Document tab for what is Document's), quieted the conductor's dock, and made Think's board list a board of cards with a New board dialog. A second pass put Activity beside Settings as a workspace page, harmonized the search field, softened the primary button to a tint of the accent, redrew Think's islands and moved a selection's actions into one bar above the tools, and gave each board card a preview of its objects. A third pass names the product in each shell, gives the workspace pages a Back control, and puts Think's boards in folders with a sidebar and Recent as the default list.

## Next, in build

A proposed Comments/Actions split is under design review: discussion and mentions should be distinct from explicitly requested execution. The current comment-to-session routing remains in place until that behavior is resolved.

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

- Production real-time collaboration and deployment: Whiteboard has an early single-server operation-sharing prototype with Git checkpoints, with character-level editing, but not a shipped multi-server service
- Serving a folder as an installable agent skill
- A desktop shell around the web plane, once it can be distributed properly

Read [[Mission and vision]] for the why behind the order.

Canvas relationships use direct Bézier curves beneath cards, best-facing edge anchors, and persistent dot/arrow endpoint markers.

Canvas curves favor the exterior of their project frames, scoring routes by card overlap and time spent inside frames. Direct under-card curves remain a fallback; moved frames update this preference during dragging.
