---
title: Command reference
order: 50
description: Every Hanji CLI command, in one table.
---

# Command reference

The whole CLI, run as `pnpm hanji <command>`. It reads `HANJI_DATA_DIR` for where to work, defaulting to `~/.hanji`.

| Command | What it does |
|---|---|
| `add-mount <name> <repoUrl> <mountPath> [subpath] [--suggest] [--local] [--kind pages\|work\|think]` | Add a repository, or a folder of one, as a mount, and print its kind. `--suggest` makes it proposal-only; `--local` mounts a folder on this machine in place, stored as its absolute path; `--kind` sets the kind by hand (see [[Mounts]]). |
| `remove-mount <name>` | Remove a mount and its indexed pages. The clone directory stays on disk. |
| `sync` | Fetch every pages mount from its remote and reindex what changed. Plan and think mounts are left to Plan and Think. |
| `rebuild` | Discard the index and rebuild it from the local clones. Always safe. |
| `token <name> <mount[/folder\|/page.md]=read\|read+propose\|read+comments\|read+propose+comments>...` | Mint a bearer token for an agent, scoped to a mount, or on a pages mount a folder or one page - add `+comments` to also grant that scope's page comments. Prints it once. |
| `revoke-token <name>` | End a token by its name. Its grants go with it: an MCP client that held it is refused from its next request; a Hanji Plan session keeps its Document tools until it sleeps, and its next start finds none. |
| `list` | List every mount with its kind, and how many pages each pages mount holds. |
| `export <outDir> [mount...] [--okf]` | Publish everything open to everyone: a browsable static site, `llms.txt`, `llms-full.txt`, and every page as plain Markdown. Mount names narrow the set; nothing widens it. `--okf` also shapes the Markdown mirror into an [Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format) bundle, described in [[Publish to the web]]. |
| `rule <mount> [path] <everyone-read\|everyone-propose\|restricted\|clear>` | Set or clear a general-access rule from the command line - what Share's select does in the front. |
| `set <key> <value>` | Write an instance setting from the table below. |
| `tools [write,work,think]` | Print the tools this workspace has on, or set them: a comma-joined list of `write`, `work` and `think`, at least one. Turning a tool off keeps its mount and its files; a mount added for a tool that is off waits until the tool is on. |
| `assets [mount]` | List uploaded images no page references anymore, per mount or everywhere. |
| `digest <mount> [path] [--since <iso>]` | Document "what changed, what awaits you" as a Markdown page into the mount (default `digest.md`), committed by Hanji itself. Covers since the previous digest; a quiet window with an empty review queue writes nothing. |

One caution on `digest`: the page is written with the owner's eyes and summarizes the whole workspace, so it belongs in a mount whose readers may see all of that. In a mixed-audience workspace, point it at a restricted mount.

## Examples

```bash
pnpm hanji add-mount handbook git@github.com:you/product.git handbook docs
pnpm hanji sync
pnpm hanji token my-claude handbook=read+propose
pnpm hanji list

# the digest as a morning ritual: cron writes the page, you and your
# agents read it like any other page - no mail server involved
# 0 7 * * *  cd /path/to/hanji && pnpm hanji digest notes
pnpm hanji digest notes
```

## The settings it holds

`hanji set <key> <value>` writes these, and Settings in the front writes the same rows except the last. They live in `config.db`, the durable database; the index can be rebuilt at any time and never holds them.

| Key | What it sets |
|---|---|
| `workspace_name` | The name in the sidebar and on the published site. |
| `workspace_mode` | Whether this instance runs as one person's or a team's. |
| `search_aliases` | `<mount name>/<page path>` of the page declaring this knowledge base's synonym classes, written `term = alias, alias` or as a two-column table row - see [[Reading and writing]]. Unset means no aliases. |
| `search_log` | `off` turns off the local query log; default on. The log lives in `config.db`, so an entry outlives the page it names, and no principal is recorded with it: on a shared workspace every reader's queries land in the same table. |
| `theme_accent` | The workspace's one accent, as `#rrggbb` (near black when unset); light and dark follow the system. |
| `conductor_visual` | Workspace conductor shape and per-state motions, stored as validated JSON by Settings, Appearance. |
| `theme_font` | The reading font for prose in Document: `newsreader`, `literata`, `source-serif` or `inter`. |
| `appearance_v2` | Set once by the first open after the one-accent change, which cleared the earlier default accents; the retired keys `theme_background`, `theme_text`, `theme_mode`, `theme_dark_accent`, `theme_dark_background`, `theme_dark_text`, `theme_dark_auto` stay in the table unread. |
| `content_width`, `content_size` | The reading column's width and type size. |
| `logo`, `logo_adapt`, `logo_hidden`, `logo_luma`, `logo_coverage` | The workspace logo and how it adapts to the theme behind it. |
| `sidebar_collapse` | Whether the sidebar starts collapsed. |
| `tools` | The tools this workspace has on, `write,work,think` in that order; written once from the mounts when a workspace is first opened by this version, then by `tools` and by Settings. |
| `export_site_label`, `export_home_label`, `export_home_url`, `export_edit_base_url` | What a published site calls itself, where its home link goes, and where "suggest an edit" lands. See [[Publish to the web]]. |
| `session_secret`, `owner_password_hash` | Written by Hanji on first run and read on every login. Not for hand-editing; `HANJI_SESSION_SECRET` and `HANJI_OWNER_PASSWORD` override them. |

## The environment it reads

| Variable | Used by | For |
|---|---|---|
| `HANJI_DATA_DIR` | everything, `tracker-host` included | Where clones and the index live. Defaults to `~/.hanji`. |
| `HANJI_SESSION_SECRET` | the front | Signs login sessions. Optional - first run generates and stores one; env overrides. |
| `HANJI_OWNER_PASSWORD` | the front | The owner's password. Optional - the welcome screen stores one on first run; env overrides. |
| `HANJI_GITHUB_TOKEN` | propose | Lets `hanji_propose` open real pull requests on a clone mount; a local mount's proposal opens none. |
| `HANJI_GIT_AUTHOR_NAME`, `HANJI_GIT_AUTHOR_EMAIL` | writes | The name and email on the owner's commits. The name wins over the one set in Settings → People (Settings → General while the workspace has no people); with neither, commits read Hanji Owner. |
| `PORT` | the MCP server | The port it listens on. Defaults to 4101. |
| `HANJI_TAILSCALE_OWNER` | the front, Hanji Plan, Hanji Think | Turns on [[On your tailnet]] and names the owner's tailnet login. The login signs a request in only when its `Host` is the loopback address or the host of one of the three product addresses below. |
| `HANJI_WEBHOOK_SECRET` | the front | Turns on push-to-sync at `/api/webhook/git`; deliveries must carry its signature. |
| `HANJI_POLL_SECONDS` | the front | Sync every pages mount on this interval. The mechanism for tailnets and local mounts. |
| `HANJI_TRACKER_URL` | the front, Hanji Plan, Hanji Think | Plan's address, where `[[HNJ-12]]`, Cited by and a board's **Open** links open an issue. Defaults to `http://127.0.0.1:4102`. On a tailnet, set it to Plan's `tailscale serve` address; its host is then a `Host` that the tailnet login signs in. |
| `HANJI_KB_URL` | the front, Hanji Plan (its web app and its session host), Hanji Think | Where links to Document pages point. Defaults to `http://127.0.0.1:4100`. On a tailnet, its host is a `Host` that the tailnet login signs in. See [[On your tailnet]]. |
| `HANJI_THINK_URL` | the front, Hanji Plan, Hanji Think | Where links to Hanji Think boards point: `[[board:boards/crit#9f2c]]` in a page, and an issue's **From** link in Plan. Defaults to `http://127.0.0.1:4104`. On a tailnet, its host is a `Host` that the tailnet login signs in. Think on a tailnet needs the `tailscale serve` host named by this address or by one of the other two. |
| `HANJI_VERSION` | the front | Set by the build from the repository's package.json: the version in the workspace menu. |

For the agent side of this, read [[Agents]]. For what is coming, read [[Where we are]].

## Whiteboard development commands

The [[Think canvas]] has its own launcher, `node --import tsx packages/whiteboard/src/main.ts`, which reads `HANJI_DATA_DIR` like the rest. With no subcommand it serves every board of the workspace's think mounts on `--port` (4104) and, in a terminal, prints the owner's access link; `--demo` serves a throwaway workspace with one seeded board; `--repo` and `--state` are refused, except that `migrate-board` takes `--state`. `key --agent <handle>` prints an agent key, `key --revoke <handle>` ends one, `key --rotate-owner` replaces the owner's, and `key --owner-link` prints the owner's link again. `read`, `ops --file <request.json>` and `mcp` take `--board <mountPath>/<folder>` and require `WHITEBOARD_TOKEN`; MCP uses stdio. `migrate-board --board <mountPath>/<folder> --state <old state folder>` moves a board from the first launcher into a folder of its mount, with its journal. `WHITEBOARD_URL` is used only by the figure scripts and `scripts/whiteboard-browser.mjs`, and should be an access link to a fictional demo board. `pnpm tracker-host` runs the session host over `HANJI_DATA_DIR` alone: it opens every registered tracker, and `TRACKER_WORKSPACE` adds a folder that is not registered, which then leads, while Plan is on (with Plan off the host opens no tracker, says so at start, and an issue's or a project's session waits until Plan is on), and a workspace of pages or boards alone has a conductor too. A second host on the same data folder exits with code 3. The `tracker` CLI acts on `TRACKER_WORKSPACE`, else on the workspace above the current directory; `tracker --tracker <PREFIX> <verb>` names a registered tracker by its prefix (exact case; it wins over both), and a prefix no mount holds fails with the registered ones listed.
