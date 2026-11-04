---
title: Links and references
order: 32
description: How a page names an issue, a project, a board or another page, and who sees each link.
---

# Links and references

A workspace can hold pages, a tracker and boards. A double-bracket link in a page can name any of them, and every link is checked against the reader's own access.

## The forms

| You write | It names | It opens |
|---|---|---|
| `[[HNJ-12]]` | an issue in one of the workspace's trackers | the issue in Hanji Plan |
| `[[project:HNJ/onboarding]]` | a project in the tracker | nothing yet: the link shows its label |
| `[[board:boards/crit#9f2c]]` | a board, or one object on it | the board in Hanji Think, with that object selected |
| `[[Roadmap]]` or `[[Roadmap#Goals]]` | a page, by its title or its file name, with an optional heading | the page, scrolled to the heading |

The same forms work in an issue's `source:` field. A label after a bar chooses the words a reader sees: `[[board:boards/crit#9f2c|board note]]`.

A page written up from a board in Hanji Think ends with such a link, `From the board [[board:boards/crit#s1|Crit › Ideas]].` for a section and `From the board [[board:boards/crit|Crit]].` for a selection, labeled with the board's title and the section's name; a section's options in Think then offer **Open write-up** to a reader of that page. Any page that links a section that way counts as its write-up. See [[Think canvas]].

A key like `HNJ-12` names an issue only when the workspace holds a tracker with the prefix `HNJ` (a workspace can hold several trackers, each with its own prefix), so a page named `Q3-2026` stays a page. For a reader who cannot read that tracker, the key is page text too, the answer that reader would get with no tracker in the workspace. A title that holds a `#`, like `Sprint #12 retro`, also stays a page: Hanji tries the whole text as a title first, and reads the part after the first `#` as a heading only when that finds nothing.

## Who sees a link

A link opens only for a reader who can read what it names. For anyone else it shows as plain text, the same text a link to something missing shows, so a link never tells a reader that a hidden issue or page exists. In Document that text has a dotted underline, and pointing at it says what is missing from the link's own words: `No issue HNJ-999 in Hanji Work`, `No page named Roadmap`, `No board at boards/crit`.

## Cited by

At the foot of a page, **Cited by** lists the issues whose body or `source:` names that page, by title or file name, in any letter case, with or without a heading. A canceled issue is left out of the list; a done one stays. Each key shows the issue's title and status when you point at it. A reader sees the list only when they can read the tracker's mount. The tracker writes these links once its folder is registered as a work mount in the same data folder; see [[Mounts]]. On a tailnet, Hanji Plan signs in each person who holds view on the tracker's work mount, so an issue opens from this list, and from an issue link, for each of them; see [[On your tailnet]].

## From and Mentioned in

In Hanji Plan, an issue's **Related work** shows its source under **From** by name: a page by its title, a board by its title, and an object on it as the board's title, a `›` and the object's first line. A page source may also be written as `docs/plan.md`, the way Document shows a page's place. A source Plan cannot open for you shows as written, with a note that says which product did not find it. **Mentioned in** lists the pages that link the issue by its key, `[[HNJ-12]]`, by their titles, for a reader who may read them.

## Docs on a project

In Hanji Plan, a project group on a view's canvas lists under **Docs** the pages that cite the project, for a reader who may read them: a page that ends with `Documentation for the [[project:HNJ/<slug>|project]].` or `Status of the [[project:HNJ/<slug>|project]] on <date>.` is one. The owner's **Draft a doc** and **Draft a status** start a session that writes such a page in the chat and proposes it when the owner says so.

## When things move

Issue keys never change and are never used twice. Pages resolve by title or file name, so moving a page to another folder keeps its links. Renaming both the title and the file breaks the links to it: `tracker lint` reports the issue sources that no longer find a page, and `board-missing` for a source whose board or object is gone, and nothing rewrites the files that link to it.

Each product also works alone. With Document alone, issue, project and board links show their labels and Cited by is empty. With Plan alone, sources show as text. Without any app, every link is readable text in git.

Issue links and Cited by open Plan at the address in `HANJI_TRACKER_URL`, which is `http://127.0.0.1:4102` unless you set it, and board links open Think at `HANJI_THINK_URL`, `http://127.0.0.1:4104` unless you set it; see [[Command reference]]. A board link opens for a reader with view on that board, or on the whole mount of boards; see [[Permissions]]. A link to one object finds it once the board has written it to its files, at a Git checkpoint: until then, the link to a new sticky shows its label, and a deleted sticky's link still opens the board.

In a browser, each product's header offers the workspace's other tools you can open, Document, Plan and Think in that order, as links to their addresses in the same tab, with the one you are in marked (Plan shows them on their own row under the workspace name); a tool that is off, or one you cannot open, has no link, and a workspace of one tool shows none.

In a browser a link to a sibling product opens in the same tab, as a page link does: an issue link in a page, Open issue or a chip on a board, and Open write-up all leave the tab where they are. ⌘ with the click on a Mac, Ctrl elsewhere, or the middle button, opens a new one. A board sends the edits it still holds before it leaves, so nothing typed a moment before the click is lost, and Back returns to the board. A link that leaves the suite, a link card's Open link for one, keeps its new tab.
