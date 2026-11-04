---
title: The core
order: 20
description: "@hanji/core: mounts, git, the index, search, permissions, propose, and history."
---

# The core

`@hanji/core` is the rail everything runs on. If the serializer is the paper, core is the press. It takes your git repositories and turns them into something you can read, search, and permission.

## What it holds

- **Mounts and git.** Core clones each pages mount, keeps it current, and writes through it. Plan and think mounts belong to Plan and Think. Git is driven as a subprocess, the same git you already have, so there is no reimplementation of version control to trust.
- **The index.** A SQLite database with full-text search over pages and over their sections, CJK bigram indexes beside both, and a table of wikilinks, split in two: a config database for mounts and tokens, and an index database for pages. It also keeps one fingerprint per mount standing for every non-Markdown file in it, which is how a changed image becomes visible both as a change and as a new asset URL. The index is a cache, rebuildable at any time from the clones, and a schema change re-derives it once from the pages it already holds.
- **Permissions.** One module, and one rule: every read the app serves flows through it, and the scope check is compiled in exactly one place. Search, page reads, and link resolution all pass through the same door. A page you cannot see is indistinguishable from one that does not exist. Read [[Permissions]].
- **Tailnet sign-in.** `resolveTailscalePrincipal` turns the login that `tailscale serve` sends into the owner or a person, `tailnetHostAllowed` says whether the request's `Host` is the loopback address, which covers requests made on this machine, or the host of a product address, which is the tailnet name `tailscale serve` passes, and `tailscaleHeaderVerified` asks tailscaled to confirm the login when `HANJI_TAILSCALE_WHOIS` is set. Document, Plan and Think all call them, so a person signs in to each the same way. Read [[On your tailnet]].
- **Writes.** `saveFile` commits directly, with read-your-own-writes, guarded by a per-mount lock so two writes cannot race - and the lock holds across processes, so the web front, the MCP server, and the CLI sharing one data directory cannot wedge a clone between them. `proposeSave` instead opens a branch, and where GitHub is connected, a pull request.
- **History.** Because the store is git, a page's past is just its commits. Core reads them into a list of revisions, and can hand back any past version.

## Why SQLite, and why a subprocess git

Both are the boring, durable option. SQLite is a file, not a server, so the index has no daemon to run and nothing to operate. Driving the real `git` binary means Hanji inherits whatever your git already does, credentials and remotes and all, instead of reimplementing a fraction of it and getting the edge cases wrong.

Read [[The MCP server]] for how agents reach this, and [[The web front]] for how people do.
