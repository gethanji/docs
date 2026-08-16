---
title: Byte-fidelity
order: 30
description: Why a one-word edit is a one-line change, and why that matters.
---

# Byte-fidelity

Here is a small test most editors fail. Open a document, change one word, and save. Now look at the diff. If four paragraphs you never touched also changed, quotes flipped, list markers rewritten, whitespace reflowed, then the tool has been rewriting your work behind your back. On a knowledge base kept in git, that is not cosmetic. It is the difference between a history you can read and a history that is noise.

Hanji refuses to do that. A one-word edit produces a one-line change. That is the whole promise, and it is harder to keep than it looks.

## How it holds

When Hanji reads a Markdown file, it does not just parse it into a tree and forget the original. It keeps a map from each block back to the exact bytes it came from. When you edit, only the blocks you actually changed are written back from the tree. Every block you left alone is written back byte-for-byte from the original, untouched. The reconciler works at the block level, so a paragraph you did not open never gets the chance to drift.

The result is quiet. A save you make when you changed nothing is byte-identical to the file you started with. There is no commit, because there is nothing to record.

## Why it is the crown jewel

Two reasons, and both are about trust.

The first is your history. When the diff of a one-word fix is one line, `git blame` still means something, a review is still readable, and a year of edits does not turn into a wall of reformatting churn. The people and the agents reading your history can trust that a change shown is a change made.

The second is the walk-away promise. Byte-fidelity is what lets Hanji claim your files are still just your files. If the tool quietly rewrote them, they would slowly become the tool's files, in a shape only it produced. Because it does not, you can leave at any moment and take exactly what you would have had if Hanji had never existed.

This is guarded, not hoped for. A corpus test round-trips more than 600 real Markdown files through the engine and asserts every one comes back byte-identical. That gate stays green, or the build does not ship. See [[Performance]] for the other numbers, and [[Principles]] for where this belief sits among the rest.
