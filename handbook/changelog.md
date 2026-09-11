---
title: Changelog
order: 130
description: The road behind us, honestly logged.
---

# Changelog

Reverse chronological. Struck-through text is a decision we made and then moved past, kept visible on purpose, because a changelog that hides its own wrong turns is just marketing.

## 0.6, and more than one hand

- The number reads 0.6. What 0.5 could not do: hold a second person on a page, survive two edits landing at once without asking anyone to throw work away, or tell you what moved while you were gone. All three are here now - editors who commit straight to git, a collision that folds instead of dead-ending, presence beside the byline, and a periphery (Activity, the digest, the agent briefing channel) that answers "what happened" without anyone taking notes.
- The same jump that took 0.1 to 0.5 rather than a release ritual: the number keeps one home, the repository's own version, and every surface that prints it - the sidebar, the agent banner - takes it from there, so what you read can never drift from what shipped.

## The published site reads like the app

- The exported site now carries the app's own reading surface, not a copy of it. `packages/render/prose.css` is one file: the app imports it, `hanji export` inlines it. It had been a second stylesheet written in August and left behind while the app moved on, and the two had drifted for weeks - highlights in four colours and the accent-tinted code panel shipped in the app and never reached the published page at all.
- What you see change: prose set at the app's reading size and heading rhythm, links in ink with an accent-tinted underline rather than painted accent throughout, inline code and code blocks carrying the accent hue, table headers in small caps, the paper grain, and an accent selection wash. A keyboard focus ring too, which the published site never had.
- A three-state theme control sits at the top of every page - auto, light, dark - remembered in the browser. ~~The published site was deliberately script-free~~; it was also the one Hanji surface where a reader could not choose. It is ten inlined lines and nothing remote; without JavaScript the page still renders in full and follows your system, exactly as before, and the control hides itself rather than sitting there dead.

## Activity shows the edits nobody committed, and the note only speaks when it can point

- On a local mount that sits below its repo root (a `notes/` folder mounted out of a larger repo), every commit row on Activity linked to a page that does not exist: git names files from the repo root wherever it runs, the feed passed those names through, and the link carried the folder twice and landed on an empty shell. The feed now speaks the mount's own paths, and a commit that also touched files above the mount loses its subject the way a subpath mount's does - a message can describe what the mount never shows.
- Activity lists the edits still sitting uncommitted in a local mount's files as one row per mount - "the files", labeled outside, "changed, not yet committed" - timed by the newest file, pages only. The freshness note announces those edits the moment an agent writes them, and the page it pointed at used to answer "nothing today". `hanji_changes` and the digest read the same feed, so an agent's session-start briefing sees them too.
- A change that moved no page - a replaced picture, a deleted page - refreshes the page and says nothing. ~~It said "Updated just now · notes" and pointed at Activity~~, which cannot show a picture; during a burst of screenshots landing in a mount that was one toast every ten seconds. An open editor no longer gets the early conflict line for it either: no page moved, so nothing changed under the draft.

## A table cell can hold more than one line

- Enter inside a table cell used to do nothing at all. A cell is a single line, so there was nothing for the key to split, and it quietly declined. Enter now puts a line break in the cell, and `⇧Enter` does the same anywhere else in the page.
- A break is written to disk as `<br>`, the only spelling a Markdown table row can carry - and now the only spelling Hanji uses for a break anywhere, so one round-trips wherever it lives. It reads back as a break too: a cell written by an agent as `first<br>second` showed you the raw angle brackets before, and now shows you two lines.
- A list inside a cell is those broken lines with their own dashes, which is as far as a Markdown table goes. Typing `- ` at the head of a cell no longer saves as `\- ` either: the escapes that guard block syntax do not belong in a cell, where no block syntax can occur.

## The note that says something changed now takes you there

- The freshness note used to name the mount and stop: "Updated just now · handbook", with no way to see what had actually moved. The sync knew the count and nothing else, so there was nothing to link to.
- Sync now reports which pages moved, capped at five and with deletions left out (a link to a page that no longer exists is worse than no link). When exactly one page moved, the note names it and offers a link straight to it. When several did, it names the mounts and points at Activity, which exists to answer "what moved". When the page that moved is the one you are reading, the refresh already showed you, so it says "this page" and offers nothing to click.
- A folder's `index.md` is named by its folder and a mount's by its mount, so the note reads "assetdemo" rather than "index".
- Toasts are also the width of what they say now. The container's width was being inherited as every notice's width, so a short one sat in a wide box with a long empty right side.

## More than one hand writes to base

- A third level for people: `read+write` makes an **editor**, whose Update commits straight to git the way the owner's does, on a mount, a folder, or one page. Viewers read, contributors suggest, editors write, the owner decides. A token never holds it: agents keep proposing.
- A contributor's conflict folds like the owner's. When the page moved under a contributor, Hanji merges the two edits before proposing, so the proposal carries both, and only a true overlap stops to ask. And **Keep my version** no longer overwrites the other side: it keeps your wording where you both changed the same lines and folds in the rest of their edit, on the commit path and the propose path alike. ~~Keep mine replaced theirs entirely~~ - that was the data-loss door, and the contributor side was about to inherit it.
- The open page checks itself every seven seconds while its tab is visible, editing or reading: an editor gets the quiet chip, a reader gets the page repainted in place, and this holds for saves made in the app, not just edits arriving from the files. The chip names the other hand where the name is honest (the last committer on a git-backed mount) and says "This page changed" on a plain folder.
- Presence, the first primitive of the room to come: a page says "Remy is editing" beside its byline, and an editor sees "Remy is also editing this page. Edits merge when you save." In-process, twenty seconds of memory, no lock, no take-over door.
- Two serializer fixes. An ordered list running past nine (`1.` to `10.`) saved with a leading space and refused to reconcile; it now writes its numbers unpadded. And a re-serialized block escaped `[[wikilinks]]` into `\[\[...\]\]`, which the link index no longer saw; wikilinks now survive an edit verbatim. The handbook's own guide index, ten wikilinked items, could not be saved before.
## A replaced image reaches the page

- An agent swapping a picture used to be invisible. The index only ever walked `.md`, so the scan that watches for change never looked at an image, reported that nothing had moved, and no open page refreshed. And even when something else did force a render, the image's URL had not changed, so the browser kept serving the copy it already held. Quitting the app was the only way through.
- Every mount now carries a fingerprint of its assets: names, sizes and modification times, never contents, so it costs the same stat scan the page check already pays for. When it moves, the mount counts as changed and the page refreshes exactly as it does for a prose edit.
- That same fingerprint is stamped into every image URL, so a changed asset arrives at an address the browser has never held. Versioned URLs can then be cached properly: images are immutable for a year instead of being re-fetched every five minutes.
- The trade, said plainly: changing one image moves the URL of every image in that mount, so they all fetch once more. The value is already computed by a scan that was happening anyway, and the alternative is hashing every image on every render.

## Highlights, in four colours

- Highlight a phrase while editing: `⌘⇧H`, the swatches in the selection bubble and the bottom toolbar, or just type `==like this==`. Four curated colours - a default yellow, then blue, green, and pink.
- The default writes portable `==x==` on disk, the spelling every other Markdown tool already understands. A chosen colour writes `<mark class="hl-blue">`, because standard Markdown has no way to name a colour. The common case stays clean Markdown; colour costs portability only when you ask for one. Both read back as the same highlight, so the editor never shows you raw angle brackets.
- Colours ride a fixed class through the sanitize allowlist, never a `style` attribute - the same rule that governs image widths and embeds. A colour the workspace does not know stays literal text rather than being quietly recoloured.
- The tint is mixed into the page's own paper, so the same four swatches stay legible when the workspace theme flips dark.

## The page keeps its chrome, and a selection keeps its text

- The header bar - back and forward, the breadcrumb, the page's actions - sticks to the top of the view now, so a long page never scrolls away from its own controls. The hairline under it belongs to the stuck state alone: at rest the bar is simply the page's first row, and the rule arrives only once there is prose running underneath for it to separate.
- Reading and editing draw the *same* bar. Entering edit mode swaps its buttons instead of rebuilding the top of the page.
- Edit is no longer a navigation. The editor mounts in place of the prose, on the page you are already on, so `⌘E` from halfway down a page leaves you exactly halfway down it. Building the editor over a long page takes a moment, and for that moment the page you were reading is what stays on screen - the two are laid over each other and trade places in one frame, so there is no blank, no placeholder, and nothing to jump back to the top.
- Selecting prose no longer opens a comment box on top of your selection. A small bar appears instead, offering **Copy** and **Comment**, and the text stays selected the whole time - so `⌘C` still works, and commenting is a deliberate press rather than the only thing a selection is allowed to mean.
- **Copy** puts the selection on the clipboard twice over: as rich text for anywhere that takes it, and as real Markdown for anywhere that does not. Headings, lists, tables, and code fences all survive the trip. A selection that crosses blocks can still be copied; only commenting needs a single block to anchor to.

## The queue triages, the strip counts hands, the digest writes itself

- The Activity page grew a strip: eight weeks of days, human work rising above a baseline in ink, agent work hanging below in the accent, outside writes worn a shade fainter on the human side - the workspace's balance of authorship at a glance. Under it, the feed takes filters (humans / agents / outside, edits / comments / reviews), by URL, bookmarkable.
- The proposals queue reads like triage now: every row carries the agent's self-review note, the diff's weight (+added/−removed), and its age; the proposal page shows the note beside the diff. The cheapest trust signal an agent can send, finally shown where the decision happens.
- `hanji digest <mount>` writes "what changed, what awaits you" as a Markdown page into the workspace - web-readable, agent-readable, carried by git like everything else, committed by Hanji under its own name. Each digest covers since the last one; a quiet window with a clear queue writes nothing at all, because the system must never notify about its own notifications. Cron it and the artifact is your morning briefing - no mail server, no push service, no channel to own.

## Agents can read comments, if you let them

- A new capability joins the token grammar: `read+comments` (stackable, so `read+propose+comments` works too). Grant it, and `hanji_list_comments` and the `GET /comments` alias hand back a page's threads - quote, author, body, resolved state, one level of replies - shaped for an agent to read, not to write. Nothing about a comment's DOM anchor internals leaks out.
- Comments stay opt-in and page-scoped: a token still needs ordinary read access to the page itself, and the capability only reaches as far as the grant that carries it. Without `+comments`, a token gets back the same "Not found" a missing page would - the opacity people already get from a locked page holds for agents and comments too. See [[Comments]] and [[Agents]].

## The version catches up

- Hanji sat at 0.1 through forty-odd arcs of work; it reads 0.5 now, closer to honest. The number keeps one home - the repository's own version - and the reading surface and the agent surface both take it from there, so what the sidebar shows can never drift from what shipped.

## The comment surface settles

- The way into a page's annotations is a comment icon in the page header now, in one row with history, share, and edit, rather than a pill floating over the prose.
- A thread reads as one unit: each on its own soft card, the author on a small accent label, the quiet actions - edit, resolve, delete - held back until you hover, and a reply tucked under its parent behind a hairline.
- The quote at the head of an anchored thread wears the very highlight its phrase wears in the page, so the panel and the prose show one mark, not two treatments of it.

## The comment box becomes a small editor

- The comment composer is a real WYSIWYG surface now, the same editing engine as the page itself in miniature: **⌘B** and **⌘I** toggle bold and italic, a line starting with `-` or `1.` still becomes a list as you type, and `:name:` completes to an emoji through the same menu the page editor uses. Enter still sends and Shift+Enter still starts a new line - the composer keeps a visible **Comment**/**Reply**/**Save** button too, so a mouse or a touch screen works exactly as well as the keyboard.
- You can edit your own comment: an Edit link swaps it back into the composer, pre-filled with its current text, and Save or Cancel take Comment's place. An edited comment carries a quiet "edited" mark next to its time, so a reader can tell a note changed after it was first written. The workspace owner can edit any comment, the same reach they already had to resolve or delete one.

## Comments read like comments

- Comment bodies support light formatting: **bold**, *italic*, and `-`/`1.` lists render for real now, instead of every note flattening to one plain paragraph. It is a small, safe renderer built as React elements, not raw HTML - there is no way a comment's body becomes markup another reader's browser executes.
- Enter sends a comment, Shift+Enter starts a new line - reaching for the mouse to click **Comment** is optional now, not the only way.
- The composer that opens on a fresh text selection focuses itself: selecting a phrase is already the intent to write about it.
- A comment's time reads like the rest of the app: "3 hours ago" rather than a bare `2026-08-25`, with the exact moment on hover.

## A place to talk about a page

- A sidebar for the marks in a page: click a highlighted phrase and the note it belongs to opens in a panel beside the prose, scrolled to and lit. Browse every annotation on the page there, filter by open or resolved, reply or resolve without leaving your place in the text.
- Select a word or sentence to comment on exactly that, not just the page: the quote gets a shared highlight in the prose for every reader. Edit the underlying text and the note detaches to "On an earlier version" at the foot of the page, quote intact, rather than pointing at words that no longer exist.
- Comments: a thread at the foot of every page, open to anyone who can read it. Reply one level deep, resolve a thread or delete a comment as its author or the owner. Comments live in Hanji's own store, never in git and never in an export - move a page and its comments follow, delete one and they go with it. See [[Comments]].

## Navigation, quick

- Moving between pages is near-instant now. The page byline was quietly walking git history on every navigation to find who touched the page and when - hundreds of milliseconds on a deep-history mount, regardless of how little history a page had. That reading is now indexed at sync time and read from the index, so a navigation costs only what the page takes to render.
- Back and forward controls sit at the top of every page, with `⌘[` and `⌘]` - a standard way through your recent trail that does not lean on the browser's own chrome.
- Clicking a page shows its shape immediately - a light skeleton where the content will land - instead of holding the previous page still until the next is ready.

## Finding things: sort a section by date or name

- A folder or mount can now sort its own children four ways: **Manual** (your curated `order:`, the default and unchanged), **Newest first**, **Oldest first**, or **A–Z** by title. The control lives on the section's row and appears once it is open; a re-sorted section stays lit so it reads at a glance.
- The date it sorts by is the one that actually means something: a date in the filename if the page carries one (notes are often `YYYY-MM-DD`-named), the page's last-commit date otherwise. So a bulk rename or re-commit that flattens git's own dates does not flatten the order - the filename's date survives it.
- The choice is per section and per person, kept in the browser: it never touches the repository or what anyone else sees. Drag-to-reorder pauses while a section is sorted, because the visible order is no longer the stored one - switch back to Manual to curate.
- Under it: every page's last-commit date is now indexed (one git pass per sync), so date order is instant to read and doesn't walk history on every render. The same date powers the page byline.

## The editor sharpens

- Code blocks take the workspace's own accent hue instead of a fixed slate, so a warm workspace gets a warm block and a green one a green block - a deep panel that reads as part of the theme, in both modes.
- Selecting an image no longer pops the text formatting bubble over it: bold/italic have nothing to act on there, and the image's own controls own that moment.
- The image controls give their chips more room, and left/right alignment show real alignment icons rather than the empty rectangles the old glyphs left where a font had no character.
- The slash menu gained **Code** - a code block was insertable by typing ``` but had no entry in the menu until now.
- The page's YAML frontmatter no longer shows as a raw "source" card while editing: it is metadata, not body content, so it stays in the file (round-tripped untouched) but out of the editor's way. Raw HTML and other unsupported blocks still show their source card, since there is nowhere else for them to live. Frontmatter is guarded, too - clear the whole page and it survives, because losing a title or an order to a stray select-all is not an edit anyone means to make.

## Small breaths

- A page's header opens up: more air between the breadcrumb and the title, and between the title and its byline, so the top of every page reads calmer.
- Images sit a touch inside the measure now instead of edge to edge, and take a wider gap above them than a paragraph does - so a figure reads as placed, not pasted.

## The page you are looking at is the page that exists

- The screen notices now. Coming back to the tab syncs your local mounts and refreshes the open page in place when something changed - client state survives, one quiet line says it happened. A sync that finds nothing costs milliseconds: Hanji stats the tree and stops.
- The byline wears a freshness line: "checked just now / 4m ago / syncing… / sync failed - retry" - when the index last agreed with the files, deliberately a different word than the page's own "updated". The mechanisms deliver freshness; the line delivers confidence in it. The toast names the mounts that moved and wears inverted paper: ink surface on light, paper on dark - ephemeral UI earns its six seconds by contrast.
- An open editor is never refreshed out from under you. It warns early - "This page changed while you were editing" appears before Update is pressed - and the save itself re-reads the disk at the last moment, so an outside edit cannot be overwritten unseen. Plain folders get the same guard, where no git history could bring an overwritten change back.
- Agents stopped reading stale: every MCP and curl read freshens local mounts first (throttled, cheap by construction).
- The Activity page: what moved across your mounts, straight from the history - edits, comments, and reviews, grouped by day, actors labeled honestly (an outside commit is never dressed as an in-app one). A "seen up to here" line rides a per-reader cursor; nothing anywhere is marked read by being seen.
- `hanji_changes` and `GET /changes?since=…`: an agent's first question at session start - what moved since I last read? - answered from the same authz-filtered feed. `GET /feed.atom` is the Atom projection; a token rides the URL, and no token means the everyone regime, exactly like a published site.
- Proposals carry the agent's self-review note (the commit body, shown with the diff), and Reject finally leaves a trace: who refused what and when, with the proposal's tip kept under a ref instead of drifting toward garbage collection.

## Export speaks Open Knowledge Format

- `hanji export --okf` shapes the Markdown mirror into an [Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format) bundle: every concept doc gets a `type` in its frontmatter (yours wins if you set one), the reserved `index.md` and `log.md` stay frontmatter-free, and the bundle root declares `okf_version`. The static site and `llms.txt` ride along unchanged, and the source in git is never touched, the shaping lives only in the copy on disk. Mounting an OKF bundle already worked, because a bundle is just Markdown on git.

## The lockup opens, the footer retires

- The workspace name is a door now: Settings and Log out lead, Documentation and Report an issue follow as real rows, and the project meta fades below - the version beside a What's new link to this page, then Contribute and Support, quietest last. The mark alone stays the one-click way home. Members see "Signed in as" where owners see Settings.
- The sidebar's bottom chrome is gone entirely - no action row, no links row, no version line, no closing hairline. The page tree gets the whole rail.
- Page history moved up beside Delete and Edit as a quiet bordered chip - and it shows for every reader now, not just principals who could edit. The byline keeps saying who and when; ~~Browse page history~~ as a trailing text link made history look like metadata instead of an action.
- The sidebar's search trigger grew into a proper field - the rail's search matters more than its old whisper suggested.
- With the footer gone and the header calm, the rail's last rule retired too: the list slides beneath the search bar edge to edge, and the gradient alone says there is more.
- The byline learned the whole paper trail: **Created by** whom and when, **updated** by whom and when - the Confluence answer, one quiet line. The creation is the page's actual first commit, followed through renames and past the history list's cap; a repeated name is not repeated.
- Appearance's reading controls merged into one room, **Reading experience**: font, width, and text size sit over the same live specimen, because that is what they always previewed on.

## The search sheds its lines

- The palette arrives instead of appearing: the scrim leads, the card settles with a quiet rise on the same curves the sheets ride. Dismissal stays instant - a keyboard surface leaves at keyboard speed.
- The palette's strokes retired - no frame, no rule under the box, no line above the footer. One calm card held by shadow and tone; the query reads in serif at reading size, and the active row is a soft inset tint instead of an edge-to-edge bar.
- The sidebar's search trigger: ~~a tonal pill with no stroke~~ - stroke-less read as unfinished at field size, so it keeps its discreet hairline; the ⌘K chip is a tint rather than a box.
- The full results page's field became the app's one emphasized field - taller, serif, full-round - because search is that page's whole job. One height everywhere else still stands; this is the deliberate exception.
- Settings tabs turned into pilled labels, the active room a quiet accent tint; the underline bar and the divider above Save both retired - space does those jobs.
- The rule between a page's header and its prose retired too, on the reading view and the history list alike: the byline's small sans already marks the boundary, and the prose opens after honest air.

## Settings, in four calm rooms

- Appearance itself found its order: **Mode**, **Colors** (light and dark palettes as quiet labeled rows), **Reading font**, and **Reading surface** - width and text size together at last, sharing one live specimen instead of each section carrying its own.
- The dense Customization tab split in two: **General** (workspace, working mode, sidebar) and **Appearance** (mode, colors, reading surface), beside People and Mount access. Each room saves itself; the divider between the page header and the tabs retired - space does that job.
- The color controls went on a diet: circular swatches, and one quiet line naming only what the contrast pass actually adjusted. ~~A per-mode card restating every color that was "kept as given"~~ - the page itself is the live preview, and a card full of nothing-happened rows was noise.

## The sidebar finds its depth

- The sidebar sits on its own surface now: a breath darker than the page in both modes, derived from the same background seed - panels told apart by tone, with the hairline kept only at the scroll boundary.
- The workspace lockup calmed down: one steady size at medium weight beside the mark, both on the same 24px line, so the pair centers optically at any name length. ~~Two sizes stepped by name length~~ - the step was why long names never quite aligned.
- The page's delete button joined its neighbors: the same bordered chip family as Share, with a danger tint drawn from one token that reads in both modes. ~~hover:bg-red-50~~ was a light-mode assumption glowing in the dark.

## Night gets a personality, fields get a hand

- Every workspace's dark background was the same cool slate, no matter what its light background said - the derivation used a constant. Auto-dark now keeps the light background's own hue at a quiet fraction of its chroma, anchored deep: a warm workspace has a warm night, a green one a green night. Detached dark seeds are untouched.
- Fields and dropdowns wear the full-round pill now, one silhouette and one height across the app - a small Korean hand in the chrome. Selects drop the native arrow for a quiet chevron with honest room on the right.
- Fewer hairlines, more air: the settings sections separate by space and their serif headings, table rows drop their per-row rules for a soft ink-tint hover, and dividers remain only where they mark a real boundary - the sidebar's scroll limits, ~~the tab bar, the action row~~.

## The details pass

- Search snippets read as prose now: frontmatter fences, heading marks, wikilink brackets, table pipes, and backticks are quieted before display - in the palette, on the results page, and in what agents receive.
- Closing a sheet no longer leaves an invisible wall behind: the scrim stops catching clicks the moment the exit animation starts, so the next click lands where you aimed it.
- Redirects after sign-in and settings saves stay on the exact host you are on. Rebuilding them from the request could respell 127.0.0.1 as localhost, and the session cookie does not follow a respelled host.
- Smaller courtesies: people rows wear their initial on a small paper seal, the logo uploader lost the browser's "No file chosen" caption, the editor's source card holds its label inside itself in both modes, the emoji button became an outline glyph in the toolbar's own ink, a page with no history yet stops holding an empty seat where the byline would sit, and keyboard focus shows one quiet accent ring everywhere.

## curl counts as an agent

- The MCP port grew plain-HTTP aliases for all four tools: `GET /pages`, `GET /page`, `GET /search`, `POST /propose` - same bearer token, same principal, same permission check, no JSON-RPC envelope and no stream framing to parse. A cron job with curl and a token is now a first-class agent; an unknown path answers with a map of the surface.

## Ask tailscaled itself

- Tailscale mode's trust rested on a deployment shape: the port is only reachable through `tailscale serve`, so the identity header is honest. `HANJI_TAILSCALE_WHOIS=1` makes the instance verify instead of assume - every header sign-in is checked against the local tailscaled's own answer for the connecting address, one cached lookup per address per minute, mismatch means the login page. Off by default; the shape contract stands unchanged without it.

## The reading surface, your size

- Settings grew a content text size slider: 17 to 25 pixels, previewed live on the page as you drag, saved with the rest of appearance. Every size inside the prose - headings, tables, code, captions - now derives from the one base, so the whole surface scales together instead of the body drifting under fixed headings.
- The slider previews on a sample paragraph right under it - the same sentence the reading-font picker sets, so the two previews read as one voice - in your chosen font, at the size under your cursor.

## The 35,000-page sync, in minutes not half-hours

- Big first syncs were superlinear: every page's full-text row was addressed by column equality, which on an FTS5 table is a scan of the whole growing index - by page 35,000, most of the work was rereading what was already written. FTS rows now share their page row's rowid and are addressed by it, index writes run in batched transactions instead of a commit per statement, and a page's frontmatter is parsed once instead of twice. Measured on the same 35,000-page corpus: ~35 minutes before, ~2.5 minutes after. Existing databases rebuild their FTS pairing once on next open, losing nothing.

## One lock, all processes

- The per-mount write lock now holds across processes, not just within one. The web front, the MCP server, and the CLI routinely share a data directory, and a save racing a sync from a different process could wedge the clone. ~~An in-process promise chain~~ The chain remains for queueing within a process, and beneath it SQLite - already in the stack - takes a real file lock per mount that the kernel releases the moment its holder exits, so a crashed process cannot leave a stale lock behind.

## Every mark, handled

- Adapt-to-mode is an alpha mask, and a mark that fills its whole canvas masks into a solid slab - which is exactly what one looked like. The mark's **coverage** is now measured alongside its luminance at upload, adapt is refused for full-bleed marks everywhere they render (with the settings page saying why), and marks stored before measuring get measured on their next settings visit.
- The sidebar and login now follow the same matrix: full-bleed marks and photos render edge to edge instead of floating inside a contrast plate (a photo in a dark chip read as a broken border). The adapt toggle stays visible for a full-bleed SVG - disabled, with the sentence saying why - instead of vanishing. And the settings page's live sidebar preview stopped inheriting its size from whatever element it replaced, which could blow the mark up to the file's intrinsic size and push the workspace name out of the lockup entirely.

## A long sync tells you where it is

- Someone pointed the welcome at thirty-five thousand pages, and the button said "Setting up…" for a quarter of an hour. Syncs now report what they are doing - fetching, then indexing with a live count over a real progress bar, then tidying - to the welcome and to the empty workspace's content doors alike. `syncMount` grew an optional progress callback; a progress endpoint serves it, public exactly while the welcome is, owner-only after.
- The same run found something worse than silence: the indexer held the server's event loop for its whole pass, so a big sync froze every request the instance should have answered - the waiting page eventually crashed on a starved fetch. The indexer now yields every hundred files: the server stays responsive, the progress endpoint actually answers, and the bar above is honest in real time.

## The review we owed ourselves

A fast-shipped arc earned itself a deep review - every finding independently verified, then fixed.

- A security hole closed: while a freshly created instance sat unconfigured, any website in any browser could have configured it with a drive-by request - and pointed it at a git URL of its choosing. Cross-site requests to the welcome are now refused.
- Small honesties: forms no longer strand their buttons when the connection drops mid-request, and an empty front page now tells "no mounts yet" apart from "mounts, but no pages yet".

## Content without a terminal

- An empty workspace now offers its content doors right on the front page: mount a folder of Markdown, or clone a repository - owner only, same validation as the welcome, through a new mounts API. ~~"Start empty" used to dead-end into a CLI snippet~~; the wire remains for those who like it.

## Sessions, untangled

- Two instances on one host were signing each other out: cookies scope by host and ignore the port, so every login overwrote the other instance's session under the same name - two dev servers side by side did it in any browser. The session cookie now folds the port into its name, and each instance keeps its own. Existing sessions get one last "please sign in"; after that it holds.
- The login form finally answers when pressed: the button reads "Signing in…" and refuses double submits.

## The secret color, corrected

- The default accent is now the celadon-ink the landing page wears: hue 196, the blue-green 비색 has always named - `#1e8485` on paper, deeper `#007374` for text. The old default leaned green; published sites pick the change up on their next export. Workspaces with their own palette keep it - this only moves the default.

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
