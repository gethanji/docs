---
title: The MCP server
order: 40
description: "@hanji/mcp: a small Streamable HTTP server exposing scoped tools to agents."
---

# The MCP server

`@hanji/mcp` is the hosted half of the agent surface. The other half is git itself, which agents already read. This server is for the times a scoped, live read helps: an agent that should see some mounts and not others, searching and reading through the same permission check a person gets.

## How it works

It is deliberately small and stateless. It speaks MCP over Streamable HTTP, bound to `127.0.0.1` only, and every request carries a bearer token. There is no session to hold and no state to corrupt. Each request authenticates its token to a principal, builds a fresh server scoped to that principal, answers, and tears down.

The token's scopes decide everything. The server hands the same principal to `@hanji/core`, so an agent sees exactly what its token allows and no more, and the permission logic lives in core, not here. This surface only exposes it.

## The tools

Six for everyone, and a seventh you opt into - the surface stays small enough to hold in your head.

- `hanji_list_pages`, `hanji_get_page`, `hanji_search` and `hanji_links` are the read side.
- `hanji_changes` is the periphery: what moved since a moment.
- `hanji_propose` is the write side, and it does not write. It proposes a branch.
- `hanji_list_comments` reads a page's comments, but only for a token minted with the opt-in `read+comments` capability - comments are a human side-channel, off by default.

Three boundary policies live in this package rather than in core, deliberately. The 500-row page default and its cursor, the twenty-path and 256 kB caps on `hanji_changes`, and the bounds on every search parameter are all about what an agent's context can hold, and say nothing about correctness. The Activity page, the committed digest and the Atom feed read the same feed through core, each asking for its own limit - but the items themselves are never capped: every path, no byte budget.

That last line is the whole design in one tool. An agent contributes by suggesting, never by overwriting. Read [[Agents]] for how to connect one, and [[The core]] for what sits behind these tools.

## The curl aliases

The same operations exist as plain HTTP endpoints beside `/mcp` - `GET /pages`, `GET /page`, `GET /search`, `GET /links`, `GET /changes`, `GET /comments`, `POST /propose` - same bearer token, same principal, same permission check, only the wire dialect differs. They exist because the Streamable HTTP dialect is a poor fit for a shell one-liner, and half the point of an agent-native surface is that a bash script counts as an agent. Their output is compact JSON: pretty-printing cost a flat 16% of every reader's context and bought nothing.

Each of these GET routes refuses an unrecognized or misspelled query parameter rather than dropping it, naming the key in the 400, and every MCP tool refuses an unknown argument the same way, naming it in a tool error.
