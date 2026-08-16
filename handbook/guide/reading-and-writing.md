---
title: Reading and writing
order: 30
description: The human front: reading, editing, history, and new pages.
---

# Reading and writing

Everything here happens at `http://localhost:4100`, after you sign in as the owner. It is meant to feel like an editorial tool, not a git client.

## Reading

Pages open on warm paper, set in a serif for long reading. The sidebar on the left is your mounts and their folders, nested and collapsible, and it expands to show where you are. A breadcrumb across the top tells you the path, collapsing its middle on deep pages so it never runs off the screen. Wikilinks, written `[[by title]]`, resolve to the page they name, and only to pages you are allowed to open.

Search is a keystroke away. `⌘K` opens the palette: ranked results with the matching passage highlighted, your recently changed pages when the box is still empty, and the keyboard all the way to Enter. When you want to dig, the full results page holds every match with highlighted passages and per-mount filters. Multi-word queries rank pages by how many of your words they share, adjacent phrases first, and the word you are still typing already matches as a prefix.

<figure><img src="assets/search.gif" alt="The search palette opening with ⌘K, results ranking as the query is typed, and Enter opening a page"><figcaption>⌘K, a few letters, Enter. The keyboard the whole way.</figcaption></figure>

## Writing

Editing is deliberate, and it is one gesture.

1. Press **Edit**, or `⌘E`. The page becomes editable in place, on the same measure you were reading.
2. Change what you came to change. It is a real editor: Markdown shortcuts, a slash menu to insert, tables with handles, task lists, images, embeds, and emoji (the toolbar's picker, or type `:` and a few letters).
3. Press **Update**. Hanji writes a single commit behind the scenes, with a clean message, and drops you back into reading.

If you changed nothing, nothing is committed. There is no draft state to babysit, and no save button that lies. And because edits are byte-fidelity, the commit for a one-word fix is a one-line diff. See [[Byte-fidelity]].

<figure><img src="assets/edit-update.gif" alt="The whole editing gesture: Edit, a line typed in place, Update, and the fresh byline"><figcaption>The whole gesture: Edit, change, Update. One commit happened behind it.</figcaption></figure>

<figure><img src="assets/input-rules.gif" alt="Markdown shortcuts converting live: hashes into a heading, stars into bold, a dash into a list"><figcaption>Markdown as you type: hashes make headings, stars make bold, a dash starts a list.</figcaption></figure>

<figure><img src="assets/editor-toolbar.png" alt="The editor's floating bottom toolbar: format marks, blocks, inserts, and Update"><figcaption>The whole chrome of edit mode: one floating bar, and the page itself.</figcaption></figure>

<figure><img src="assets/slash-menu.png" alt="The slash menu open in the editor, offering Image, Video, Embed, and Table"><figcaption>Type a slash on a fresh line to insert.</figcaption></figure>

## When two edits collide

Hanji notices if the page changed underneath you between the moment you opened the editor and the moment you pressed Update. Rather than overwrite the newer version, it stops and tells you, so you never clobber a change you did not see. Reload, reapply your edit on top of the current page, and Update again.

## New pages

Making a page is the same gesture pointed at a path that does not exist yet. The fastest way is the + that appears at the end of every sidebar row: on a mount it creates at the root, on a folder it creates inside it, and on a page it nests, converting that page into its section's landing with the new page beneath. Give it a title, write, and Update. That first Update is the commit that creates the file.

<figure><img src="assets/new-page.gif" alt="A new page: a title typed over Untitled, a first line, Update, and the page exists in the tree"><figcaption>A title, a line, Update. The file now exists, and the sidebar already knows.</figcaption></figure>

## Organizing the sheet

The sidebar is not just a map. It is where you arrange the collection.

- **Move**: drag a page or folder onto another folder, or onto a mount's header for its root. A folder brings everything inside it, and the commit behind the drop is a real `git mv`, so history follows the page.
- **Reorder**: drop on the edge of a sibling row, where the insertion line shows. Hanji writes the new arrangement into the pages' `order:` frontmatter, as one commit.
- **Nest**: drop onto the middle of a page and it becomes a section landing with the dragged page inside, the same conversion the + performs.
- **The rail too**: drag one mount header onto another to reorder the mounts themselves.

Drops apply instantly; git catches up behind. If something blocks a move, the tree snaps back and a toast tells you why. And if you were reading the page you just moved, the address quietly follows it.

## Deleting

Next to Edit sits a quiet trash. The first click arms it into an explicit red question, the second deletes: a `git rm` and one commit. Deleting a section landing takes its children with it, the inverse of nesting. Nothing is ever truly gone, though. The repository remembers every sheet, and history can bring one back.

## History

Every page carries its past. Open its history to see who changed it and when, as a list of revisions with a byline, and open any revision to read the page as it was then. Because the store is git, this is not a feature bolted on top. It is the commits, shown well.

<figure><img src="assets/history.gif" alt="Browsing a page's history: the list of revisions with bylines, then reading one as it was"><figcaption>The page's past: every revision, and any of them readable as it was.</figcaption></figure>

Read [[Agents]] next, to let a coding agent read and propose against these same pages.
