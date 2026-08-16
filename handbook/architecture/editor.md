---
title: The editor
order: 30
description: "@hanji/editor: a ProseMirror WYSIWYG that edits Markdown and keeps byte-fidelity per block."
---

# The editor

`@hanji/editor` is where the hard promise gets harder. Byte-fidelity is one thing when a person edits raw Markdown. It is another thing entirely to put a full WYSIWYG editor in front of them, let them work in rich blocks, and still write back Markdown that touches only what they changed.

## How it keeps the promise

The editor is built on ProseMirror, with `prosemirror-markdown` translating between Markdown and the editing model, over the same [[The serializer]] underneath. The trick is that it tracks which blocks a person actually edited. On Update, only those dirty blocks are re-serialized. Every block left alone is written back from its original bytes, the same reconciliation the serializer does, carried up into a live editor.

So you get tables with real controls, task lists you can check, a slash menu, images, and embeds, and the diff of your one-word fix is still one line. The rich editing experience and the clean git history are not in tension here, because the editor was built to keep both.

## What is inside

The package is a handful of focused files: the schema that defines the editable blocks, the parse and serialize steps that bridge Markdown, the reconciler that decides what to rewrite, and the table handling, which is its own small world of hover controls and moves. Tables move with a built-in ProseMirror command rather than a custom transform, so a moved row carries its per-cell alignment for free.

Read [[The web front]] for how this editor is placed into the reading experience, and [[Byte-fidelity]] for the promise it protects.
