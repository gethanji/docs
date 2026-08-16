---
title: The two faces
order: 10
description: One knowledge base, a human face and an agent face, over the same git.
---

# The two faces

Most tools are built for one reader and then bolt on the other. A wiki built for people adds an AI chat box. An agent tool built for machines adds a rough editor and hopes the humans cope. Hanji starts from the fact that both read the same knowledge, and gives each the surface it actually wants, over the same git underneath.

## The human face

A person should never have to think about git to write down what they know.

So the front hides it completely. You open a page and it reads like a good magazine, warm paper and a serif set for long reading. You click Edit, the page becomes editable in place, you change what you came to change, and you press Update. Behind that one button, Hanji writes a single commit with a clean message and a full history. No staging, no branch names, no merge dialog. And if you change nothing, nothing is committed, because there was nothing to record.

Under the calm surface is a real editor. Tables with the controls you expect, task lists you can check off, a slash menu to insert, images that upload into the repo itself, and embeds for the tools you already use. It stays out of your way, and it never rewrites a line you did not touch. See [[Byte-fidelity]] for why that last part matters more than it sounds.

## The agent face

An agent should not have to pretend to be a browser to read your docs.

Agents already live in git, so the first face for them is git itself. They clone, they read files, they search, all at local speed, with no API throttle and no hosted round-trip. Where a live surface helps, a small MCP server gives them scoped search and reading through the same permission check a person gets. And when an agent wants to contribute, it does not reach into a page and overwrite it. It proposes a change as a branch, and where GitHub is connected, as a pull request. A person reviews it in the front and merges. The agent suggests, you decide, and your history never fills up with edits nobody approved.

## The same git underneath

The point of two faces is that there is one thing behind them. A person edits a page and an agent sees the new bytes on its next read. An agent proposes a branch and a person reviews it on the same warm page they read everything else on. Nobody is working on a copy. It is one knowledge base, and it was your git the whole time.

Read [[What Hanji is]] for the overview, and [[Using Hanji]] when you want to actually drive it.
