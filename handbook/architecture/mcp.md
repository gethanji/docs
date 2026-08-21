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

Four, and no more than four, because the surface should be small enough to hold in your head.

- `hanji_list_pages`, `hanji_get_page`, and `hanji_search` are the read side.
- `hanji_propose` is the write side, and it does not write. It proposes a branch.

That last line is the whole design in one tool. An agent contributes by suggesting, never by overwriting. Read [[Agents]] for how to connect one, and [[The core]] for what sits behind these tools.

## The curl aliases

The same four operations exist as plain HTTP endpoints beside `/mcp` - `GET /pages`, `GET /page`, `GET /search`, `POST /propose` - same bearer token, same principal, same permission check, only the wire dialect differs. They exist because the Streamable HTTP dialect is a poor fit for a shell one-liner, and half the point of an agent-native surface is that a bash script counts as an agent.
