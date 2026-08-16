---
title: How it's built
order: 80
description: The four packages and one app that make up Hanji, and how a read flows through them.
---

# How it's built

Hanji is a small pile of parts with clear seams. Four packages and one web app, each doing one thing, each testable on its own. Here is the whole map.

## The parts

| Part | What it is | Depends on |
|---|---|---|
| `@hanji/serializer` | The byte-fidelity Markdown engine: parse a file into blocks, splice edits back without touching what you left alone | nothing |
| `@hanji/core` | The rail: mounts, git, the SQLite index, search, permissions, propose, history | the serializer |
| `@hanji/editor` | The WYSIWYG surface: a ProseMirror editor that edits Markdown and preserves byte-fidelity per block | the serializer |
| `@hanji/mcp` | The agent surface: a small Streamable HTTP server exposing scoped tools | core |
| `apps/web` | The human front: a Next.js app that composes the editor and core into a reading and writing experience | core, editor |

Two things stand out on that table. The serializer depends on nothing, because byte-fidelity is the foundation everything else trusts. And nowhere in the column is a model, an AI SDK, or a metering client, because there is none.

## How a read flows

1. A repository is a **mount**. `@hanji/core` clones it and walks the tree, indexing each Markdown file into SQLite, with a full-text index and a table of wikilinks.
2. A read, from a person or an agent, names a mount and a path. It goes through the one permission check in core, which filters to the mounts the caller may see.
3. The bytes come back from the index, rendered on the front or handed to the agent as Markdown.

The index is a cache. Delete it, run `rebuild`, and it comes back from the clones, because git is the only thing that has to be trusted.

## How a write flows

A person edits on the front, and `apps/web` asks `@hanji/editor` for the new Markdown, block-fidelity intact, then core commits it to git. An agent instead calls `hanji_propose`, and core opens a branch. One path writes, the other suggests, and both end in git.

Read the parts in depth: [[The serializer]], [[The core]], [[The editor]], [[The MCP server]], and [[The web front]].
