---
title: Agents
order: 40
description: Connecting a coding agent through the MCP server.
---

# Agents

Agents get their own surface. Two of them, really: raw git, which they already know, and a small MCP server for the times a scoped, hosted read helps. Both go through the same permission check a person does.

## Mint a token

An agent authenticates with a bearer token, scoped to what it may touch - a
whole mount, a folder, or exactly one page, the same path grants people have.

```bash
pnpm hanji token <name> <mount[/folder|/page.md]=read|read+propose|read+comments|...>...
```

```bash
# a read-only agent for the handbook
pnpm hanji token docs-reader handbook=read

# an agent that can also propose changes to two mounts
pnpm hanji token my-claude handbook=read+propose notes=read+propose

# a narrow one: read one folder, propose to a single page
pnpm hanji token intern notes/reviews=read notes/reviews/draft.md=read+propose

# an agent that may also read a page's comments
pnpm hanji token reviewer notes=read+comments
```

The command prints the token once. A `read` scope lets the agent search and read within its grant. A `read+propose` scope adds the right to propose changes there. A `read+comments` scope adds the right to read that page's comments too - stack all three as `read+propose+comments` if the agent needs the full run. Anything outside its grants the agent cannot see, and cannot even tell exists - a locked page and a missing page answer identically. Content visibility rules never widen a token: an "everyone" rule speaks to people, and an agent holds exactly what it was granted. See [[Permissions]].

<figure><img src="assets/proposal-review.png" alt="An agent's proposal open for review in the front: title, diff, Reject and Merge"><figcaption>Where a propose lands: a title, a diff, and your decision.</figcaption></figure>

## Connect it

The MCP server speaks Streamable HTTP on `127.0.0.1:4101`. Point your agent at it with the token.

```bash
claude mcp add hanji --transport http http://localhost:4101/mcp \
  --header "Authorization: Bearer <token>"
```

## The tools

| Tool | What it does | Needs |
|---|---|---|
| `hanji_list_pages` | List every page the token can read, with mount, path, and title | `read` |
| `hanji_get_page` | Fetch one page's Markdown by mount and path | `read` |
| `hanji_search` | Full-text search across readable mounts | `read` |
| `hanji_propose` | Propose a page change as a branch, and a PR where GitHub is connected | `read+propose` |
| `hanji_list_comments` | Read one page's comments (threads, quotes, replies) by mount and path | `read+comments` |
| `hanji_changes` | What changed since a moment: commits with authors and pages, and comments | `read` |

## No MCP client? curl works

Every tool is also a plain HTTP endpoint on the same port, same token, no
JSON-RPC envelope and no streaming to parse. For scripts, cron jobs, and
agents that can run a shell but not an MCP client:

```bash
curl -H "Authorization: Bearer <token>" "http://localhost:4101/pages"
curl -H "Authorization: Bearer <token>" "http://localhost:4101/page?mount=handbook&path=install.md"
curl -H "Authorization: Bearer <token>" "http://localhost:4101/search?q=permissions&limit=5"
curl -H "Authorization: Bearer <token>" "http://localhost:4101/comments?mount=notes&path=idea.md"
curl -H "Authorization: Bearer <token>" "http://localhost:4101/changes?since=2026-08-27T00:00:00Z"
curl -H "Authorization: Bearer <token>" "http://localhost:4101/propose" \
  -d '{"mount":"notes","path":"ideas/from-cron.md","title":"An idea","content":"# It\n","note":"Confident on wording; check the anchor link."}'
```

`/pages`, `/search`, and `/comments` answer JSON, `/page` answers raw
Markdown, `/propose` answers the branch (and PR URL where GitHub is
connected). The permission check is the same one the MCP tools run; an
unknown path answers with a map of the surface.

## Propose, do not overwrite

## Start the session with what changed

`hanji_changes` (and `GET /changes?since=…`) answers the question a returning agent should ask first: what moved since I last read? Commits with their authors and pages, and new comments, across exactly the mounts the token can read. The intended habit: ask at session start, then brief your human in two lines instead of making them scroll. Reads freshen local mounts first (throttled to about once per ten seconds), so the answer is at most a few seconds behind the files - and there is an Atom projection of the same feed at `/feed.atom?token=<token>` for feed readers and bots (no token serves only what is open to everyone, the same rule a published site follows).

## Propose, and say what you are unsure about

A proposal accepts an optional `note`: the agent's own self-review, riding the commit body under the title. Say what you are confident about and what you are not - it shows on the review card and makes the human's yes faster. An honest "unsure whether the old anchor still resolves" is worth more than a confident silence.

`hanji_propose` never edits a page in place. It creates a branch named for the change, commits the new content there, and pushes it. If `HANJI_GITHUB_TOKEN` is set, it also opens a pull request and hands back the URL. A person reviews it and merges. That is the whole point. An agent can suggest all day, and nothing lands in your knowledge base until someone says yes.

Where does the suggestion go? Straight into the front: see [[Proposals]] for the review-and-merge side of this loop.

Read [[Command reference]] for the rest of the CLI.
