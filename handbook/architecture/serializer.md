---
title: The serializer
order: 10
description: "@hanji/serializer: parse a Markdown file into blocks, splice edits back byte-for-byte."
---

# The serializer

`@hanji/serializer` is the foundation, and it depends on nothing. Its whole job is the promise in [[Byte-fidelity]]: read a Markdown file, let something edit part of it, and write it back so that every byte you did not touch is exactly where it was.

## Two functions

The public surface is small.

- `parseBlocks` reads a Markdown document into a list of **source blocks**, each one carrying the exact byte range it came from. The parse never loses the original. It keeps a map back to it.
- `spliceBlocks` takes that list and a set of edits, and produces the new document. Edited blocks are re-serialized from their new content. Untouched blocks are copied back byte-for-byte from the original source. Nothing you left alone is ever reformatted.

Because unchanged blocks are copied and not regenerated, a splice that changes nothing returns the input unchanged, to the byte. That is what makes a no-op save commit nothing.

## The corpus gate

A promise like this is only as good as its test. The serializer carries a corpus gate: point it at a directory of real Markdown with `KB_CORPUS_DIR`, and it round-trips every file through parse and splice, and asserts the output is byte-identical to the input.

```bash
KB_CORPUS_DIR=/path/to/markdown pnpm vitest run packages/serializer/test/corpus.test.ts
```

It has run green over 600-plus real files. The rule is simple and absolute: the gate does not get weakened to make a change pass. If a file does not round-trip, the engine is wrong, not the test. Read [[The editor]] for how this same guarantee survives a full WYSIWYG editor sitting on top.
