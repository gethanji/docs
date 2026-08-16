---
title: Command reference
order: 50
description: Every Hanji CLI command, in one table.
---

# Command reference

The whole CLI, run as `pnpm hanji <command>`. It reads `HANJI_DATA_DIR` for where to work, defaulting to `~/.hanji`.

| Command | What it does |
|---|---|
| `add-mount <name> <repoUrl> <mountPath> [subpath] [--suggest]` | Add a repository, or a folder of one, as a mount. `--suggest` makes it proposal-only. |
| `remove-mount <name>` | Remove a mount and its indexed pages. The clone directory stays on disk. |
| `sync` | Fetch every mount from its remote and reindex what changed. |
| `rebuild` | Discard the index and rebuild it from the local clones. Always safe. |
| `token <name> <mount[/folder\|/page.md]=read\|read+propose>...` | Mint a bearer token for an agent, scoped to a mount, a folder, or one page. Prints it once. |
| `list` | List every mount and how many pages it holds. |
| `export <outDir> [mount...]` | Publish everything open to everyone: a browsable static site, `llms.txt`, `llms-full.txt`, and every page as plain Markdown. Mount names narrow the set; nothing widens it. |
| `rule <mount> [path] <everyone-read\|everyone-propose\|restricted\|clear>` | Set or clear a general-access rule from the command line - what Share's select does in the front. |
| `assets [mount]` | List uploaded images no page references anymore, per mount or everywhere. |

## Examples

```bash
pnpm hanji add-mount handbook git@github.com:you/product.git handbook docs
pnpm hanji sync
pnpm hanji token my-claude handbook=read+propose
pnpm hanji list
```

## The environment it reads

| Variable | Used by | For |
|---|---|---|
| `HANJI_DATA_DIR` | everything | Where clones and the index live. Defaults to `~/.hanji`. |
| `HANJI_SESSION_SECRET` | the front | Signs the owner's login session. |
| `HANJI_OWNER_PASSWORD` | the front | The single owner account's password. |
| `HANJI_GITHUB_TOKEN` | propose | Lets `hanji_propose` open real pull requests. |
| `HANJI_GIT_AUTHOR_NAME`, `HANJI_GIT_AUTHOR_EMAIL` | writes | The name and email on the commits Hanji makes. |
| `PORT` | the MCP server | The port it listens on. Defaults to 4101. |
| `HANJI_TAILSCALE_OWNER` | the front | Turns on [[On your tailnet]] and names the owner's tailnet login. |
| `HANJI_WEBHOOK_SECRET` | the front | Turns on push-to-sync at `/api/webhook/git`; deliveries must carry its signature. |
| `HANJI_POLL_SECONDS` | the front | Sync every mount on this interval. The mechanism for tailnets and local mounts. |

For the agent side of this, read [[Agents]]. For what is coming, read [[Where we are]].
