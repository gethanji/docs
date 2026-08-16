---
title: Changelog
order: 130
description: The road behind us, honestly logged.
---

# Changelog

Reverse chronological. Struck-through text is a decision we made and then moved past, kept visible on purpose, because a changelog that hides its own wrong turns is just marketing.

## Solo and team, told apart

- A working-mode switch in Settings: **Solo** keeps the chrome away - no Share buttons, no People or Mount access tabs - because an owner alone has nobody to share with. **Team** brings it all back. A fresh instance starts solo; one that already has people or rules counts as team on its own, so nothing existing loses its buttons.

## The door for contributors

- Published pages can end with a quiet **suggest an edit** link into your repository's web editor (`export_edit_base_url`) - a typo becomes a pull request in two clicks. This site turns it on the day the code is public.
- A `set` command joined the CLI, so the export settings are one line each instead of a database visit.
- [[Contributing]] now says the quiet part plainly: agent-assisted contributions are welcome - a human signs and answers, the gates judge the code.

## The first run lost its terminal

- A brand-new instance now greets you with a welcome screen: name the place, choose the owner's password, point it at a folder of Markdown or a git URL - and land signed in, on your pages. No environment variables, no CLI, nothing to wire by hand.
- One shot, by design: the moment an owner exists, the welcome endpoint is gone. A configured instance cannot be taken over through it, and a failed attempt never leaves you configured-but-broken - the password is stored last, after everything fallible succeeded.
- Env still wins everywhere, so server deployments keep their exact shape. The session secret heals itself too: absent from the env, one is generated once and kept.
- This is the soul of the Mac app taking shape: the shell will only launch the servers and open the window - the welcome belongs to the product, so every self-hoster gets it.

## The front door serves you first

- The handbook's home now leads with what every reader actually came for: a five-minute [[Quick start]], then a door for each shape Hanji is lived in - solo, tailnet, agents, publishing. The manifesto still matters; it just stops standing in front of the person mid-task.
- Two guides joined: [[Quick start]] and [[Publish to the web]] - the second one describing exactly how the site you are reading came to exist.
- The published site grew a masthead cover built from the workspace's own landing page, and an optional way home to the site that sent you (`export_home_url`).

## The handbook publishes itself

- `hanji export` grew its browsable half: a static site in the hanji look - warm paper, the serif, the sidebar tree - with not one line of JavaScript. Folders collapse with `<details>`, dark mode rides the system preference, and every link is relative, so the site serves from any path.
- The rendering pipeline moved into its own package, shared verbatim between the web front and the exporter. There is exactly one way Hanji turns Markdown into HTML, and the sanitize rules travel with it.
- Only assets referenced by exported pages ride along: an image on a restricted page stays as dark as the page.
- A `rule` command joined the CLI, so a fresh instance can open content to everyone without touching the front - the piece a publish pipeline needs.
- And this is not hypothetical: the page you are reading is served by that exporter.

## Changes arrive on their own

- Push-to-sync: set one secret, add one webhook to the repository, and a push syncs the matching mount seconds later. Deliveries are signature-verified and answered immediately - the git work happens behind the response, serialized by the same mount lock as every other write. ~~A GitHub App~~ A plain signed webhook: the App, with its registration and installation machinery, was the roadmap's word and would have bought auto-configuration at the price of tying v1 to GitHub. The endpoint speaks to anything that can sign a POST.
- The poll loop: `HANJI_POLL_SECONDS` syncs everything on an interval, because a tailnet has no inbound path for webhooks and polling stays first-class. It also covers local mounts, so edits made outside Hanji show up without asking.

## The export honors the lock, because it is the lock

- `hanji export <dir>` mirrors the workspace as `llms.txt`, `llms-full.txt`, and a per-page `.md` tree - the shape agents on the open web actually read. What lands on disk is exactly what a person with an account and no grants could see: the everyone regime, computed by the same authz predicate as every read in the product. No flag widens an export; mount names only narrow one.
- The exporter refuses a directory it didn't write, refuses an empty everyone-set, and re-exports clean over its own output.

## Agents get the same lock people have

- Token scopes grew a path: `hanji token intern notes/reviews=read` grants one folder, `notes/reviews/draft.md=read+propose` exactly one page. Tokens and people now share a single allow-region mechanism in the one authz predicate; the only asymmetry left is deliberate - "everyone" rules speak to people, never to tokens.
- Old mount-level scopes migrate themselves on the next start; nothing to run.

## The tailnet signs you in

- Tailscale mode: run the instance behind `tailscale serve`, set one variable, and the tailnet's identity signs people in - the owner by env match, everyone else through a login linked on their person sheet. No password typed, no session ceremony. See [[On your tailnet]].
- The mapping answers who, never what: grants stay exactly what the owner assigned, agents stay bearer-token only, and an unlinked tailnet visitor gets a login page that says who it saw and why they are not in yet.
- Trust is opt-in and explicit: the headers are only honored when `HANJI_TAILSCALE_OWNER` is set, and the docs state the deployment contract that makes them unforgeable.

## The lock reaches a single page

- Visibility became content-first, the way every knowledge tool taught people to think: general access (everyone reads, everyone suggests, or restricted) set on a mount, folder, or page, inherited downward, deepest rule wins. People are the exceptions that punch through. Agents never inherit "everyone" - tokens see exactly their scopes.

- Grants grew a path: a person can hold a mount, a folder, or exactly one page. The 1:1 note two people share is invisible to everyone else - tree, search, links, all of it - and a locked page still looks identical to one that does not exist.
- Sharing moved onto the page: a Share button, a name, read or suggest, done. Revoking lands on the person's next request.
- People can now be added bare, with no access at all, and given pages one Share at a time.

## The sidebar becomes a place you organize

- A second pair of hands: the owner adds people in Settings with a name, a password, and per-mount scopes. Viewers read; contributors propose through the same review card as agents. Owner powers stay the owner's.
- Suggest-only mounts opened to people: Update becomes Propose in the editor, and the proposal waits in the same review card an agent's would. One ceremony for every writer.

- A + at the end of every mount, folder, and page row creates a new page in that context. The + on a page nests: the page becomes its section's landing and the new page starts inside it.
- Pages and folders drag: onto a folder or a mount to move there, onto a row's edge to reorder among siblings, onto a page's middle to nest into it. A folder brings its whole subtree, and mounts drag to reorder the rail itself.
- Drops land instantly. The tree moves first, the git work follows, and a failure snaps back with a toast that says why. Reordering is one commit writing `order:` frontmatter; a move is a real `git mv`, so history follows the page.
- Pages can be deleted: a quiet trash next to Edit, armed by a second click, backed by `git rm`. Nothing is truly lost; the repository keeps every sheet.

## Search grew into a real feature

- One backend upgrade: bm25 ranking with a heavy title boost, snippets that mark their hits, prefix matching on the word you are still typing, and multi-word queries ranked by how many of your words a page shares, adjacent phrases first.
- Two surfaces on it: a `⌘K` palette (ranked results, recent pages when empty, keyboard all the way) and a bookmarkable `/kb/search` page with highlighted passages and per-mount filters.
- Agents search through the same ranking, via `hanji_search`.

## The workspace becomes yours

- A workspace name and logo. An SVG adapts its color to light and dark; any other mark gets a mode-safe plate when contrast demands one.
- Colors from three seeds, reinterpreted per mode in OKLCH with readability secured for you, plus curated reading fonts. Your seeds are never altered, only reinterpreted, and the settings page shows which is which.
- Emoji in the editor, three ways: a toolbar picker, `:query` suggestions at the caret, and GitHub-style `:tada:` conversion on the closing colon.
- Proposals left the content list. A card appears under the search box only while something waits, and disappears when the queue is empty.
- Versioning starts at 0.1.0, printed quietly at the bottom of the sidebar.
- A real phone experience: a top bar, a drawer, the same sidebar.

## The agent git flow

- The handbook itself now takes proposals: its mount points at the real repository, so agent contributions to these pages arrive as branches and get reviewed right here.
- Proposals now land in the front: a pending badge in the sidebar, a line diff, and Merge or Reject. Merging is git all the way down: a merge commit on base, the branch deleted, the page and index updated. See [[Proposals]].
- A proposal that no longer merges cleanly opens with a warning and a disabled Merge. No conflict screen, by design: re-propose or reject.
- Along the way the handbook itself found and drove product fixes: sidebar ordering by frontmatter `order`, SVG assets served safely, image width, alignment and captions, local path mounts, a boot smoke test, and an editor that loads only when you edit.

## The editor, and everything around it

- Editable GFM tables, with Notion-style controls: hover handles, boundary insert, drag to move.
- GFM parity in the editor: interactive task-list checkboxes, strikethrough, bare-URL autolinks.
- A collapsible nested sidebar built from page paths, plus a middle-collapsing breadcrumb.
- The editor grew from ~~a block-scoped textarea~~ into a full WYSIWYG surface, with dirty-block byte-fidelity intact.
- A slash menu to insert, image upload into the repo's own `assets/`, and domain-allowlisted embeds.

## v0.1, the foundation

- The byte-fidelity serializer, its corpus gate green on 600-plus real Markdown files.
- The git-backed core: mounts, sync, a SQLite and FTS5 index, permissions on a single read path.
- The MCP agent surface: scoped read tools and propose-as-PR, one bearer token per agent.
- The first web front. Reading only at first, then editing once the craft was locked.

## Decisions we walked back

- Non-Markdown blocks: ~~custom directives~~ plain HTML blocks, so the files stay portable and you can always walk away.
- The editor engine: ~~Tiptap~~ ProseMirror with prosemirror-markdown. We chose byte-fidelity over the faster start.
- The name: ~~kb-rail~~ Hanji. A codename is a placeholder. This one is a promise.

See [[Where we are]] for what is shipping now.
