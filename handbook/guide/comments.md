---
title: Comments
order: 35
description: A place to talk about a page, right on the page.
---

# Comments

Every page ends in a quiet thread. Anyone who can read the page can read what is said there, and add to it too - comments are discussion, not page content, so reading is the only right that gates them.

## What a comment is

A comment sits at the bottom of the page, under its own heading, separate from the prose above it. Write a note - **⌘B** and **⌘I** format as you go, a line starting with `-` or `1.` becomes a list, and `:` opens the same emoji menu the page editor uses - then press **Comment**, or just press Enter (Shift+Enter starts a new line). It appears with your name and when you wrote it. Reply to any comment once - replies are one level deep, so a thread stays a conversation, not a maze. The comment's author, or the workspace owner, can edit it afterward (an edited comment carries a quiet "edited" mark), resolve a thread (it fades but stays readable), or delete a comment outright; everyone else reads.

This is Hanji's version of the sticky note on a shared page: for the aside that is not worth its own edit, the question for a reader before you make a change, the "looks good" that closes a loop. It is not a change to the page and never a substitute for one.

## What a comment is not

Comments never enter git. They live in Hanji's own metadata store, next to mounts, users, and permissions, and they never become a commit, a file, or a line anyone `git blame`s. Export a mount to the static site or `llms.txt`, and comments do not ride along - the export, and the open web it feeds, carry the page alone. (An agent on the live MCP can read them, but only with an explicit grant - see below.)

That also means a comment follows the page's own life in git: move a page and its comments move with it, delete a page and its comments go too. Nothing is ever left pointing at a path that no longer exists.

## Commenting on a word or a sentence

A comment does not have to be about the whole page. Select a run of prose - a word, a sentence, a paragraph - and a small bar appears over it offering **Copy** and **Comment**. Press **Comment** and the composer opens right there, already focused, ready to type. (The bar is deliberate: a selection used to open the composer outright, which took the selection with it, so you could not simply copy the words you had just picked out. Now commenting is one press further on, and the text stays yours until you ask for it.) Post the note, and the selection carries a mark from then on: every reader sees the same quiet wash of the workspace accent over that exact phrase.

An annotation's wash is not the same thing as a [[Reading and writing|highlight]] you apply yourself. The wash is Hanji showing you where a note is anchored, it is always the workspace accent, and it lives in Hanji's own store; a highlight is content, it is one of four colours you chose, and it is written into the file.

A comment needs a single block to anchor to, so a selection spanning two paragraphs offers **Copy** alone.

These anchored notes live in a panel beside the page, not in the thread at its foot. Open it from the comment icon in the page header, or just click a highlighted phrase and the panel opens with that note scrolled to and lit. It lists every annotation on the page, filters by open or resolved, and lets you reply or resolve without leaving your place in the text. Each thread leads with the quote it was written against, wearing the same highlight the phrase wears in the prose, so the note reads clearly even with the page scrolled away.

That link is best-effort, not permanent. Edit the block the quote lives in, and if the quote no longer matches what is there, the note detaches: it moves down into "On an earlier version" at the foot of the panel, still showing the exact words it was written against, but no longer painted into the prose. The note is not lost, only its anchor is - the words it once pointed at have moved on.

Inline highlights use the browser's CSS Custom Highlight API, which needs a fairly current engine (recent Chrome, Edge, or Safari). On an older browser without it, the panel and its quoted text still read fine - only the painted highlight in the prose is missing.

## Agents and comments

Comments are a human side-channel by default. An agent reads them only through an explicit `read+comments` scope on its token (or `read+propose+comments`), and only on pages it can already read - a token without the capability sees nothing, the same as a page that does not exist. See [[Agents]] for the token grammar and `hanji_list_comments`.

## What's here now

Both forms of comment ship: a thread at the foot of the page for the page as a whole, and anchored threads for a specific word or sentence - highlighted right where they were written, and gathered in a panel beside the prose, reached from the page header or by clicking any highlight.
