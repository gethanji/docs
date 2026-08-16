---
title: What Hanji is
order: 20
description: How Hanji turns the repos you already have into one permissioned knowledge base.
---

# What Hanji is

Hanji turns the git repositories a team already has into one shared, permissioned knowledge base with two faces. One for people, one for agents. Nothing moves and nothing is copied into a new store. Hanji reads what is already there and serves it well.

## Mounts

You point Hanji at git repositories, or at folders inside them, and each one becomes a **mount** in a single tree. A repo of engineering docs, a folder of product specs, a private repo of meeting notes, all of it lands in one place to read and search, while each repository stays exactly where it is. Hanji never becomes the owner of your content. Git remains the source of truth, and the index Hanji builds on top is a cache it can throw away and rebuild at any time. See [[Mounts]] for how to add one.

## For people

A quiet reading and writing front. Pages are Markdown, rendered with care on a warm sheet meant for long reading. Editing is deliberate. You enter edit mode, make your changes, and press Update, and that Update is a single commit with a full history behind it. Git stays out of sight unless you go looking for it. There is a full editor under that calm surface, with tables, task lists, images, and embeds, and it never rewrites a line you did not touch. Read [[The two faces]] for how the front is built, and [[Byte-fidelity]] for why your history stays clean.

## For agents

Coding agents read the knowledge the way they read code, as files in a repository, at the speed of a local clone. Where a hosted surface helps, a small MCP server offers scoped search and reading, plus a write path that proposes changes as branches rather than editing pages in place. An agent can suggest. A person decides. That one rule fixes the failure mode everyone with a bundled AI has hit, the model that confidently edited the wrong page. Here it opens a branch, and you merge it or you do not.

## For the sensitive parts

Some folders hold decisions and notes that should not be open to everyone. Every read Hanji serves, search and links and the agent tools alike, runs through a single permission check, so a reader only ever sees what they are allowed to see. A search will not leak a title. A wikilink will not resolve to a page you cannot open. And we are honest about where that check is strict today and where it is still getting finer. Read [[Permissions]] for exactly what is enforced now.

## What it will never do

No bundled model. No per-token credits. No format you cannot walk away from. If you leave, you already have everything, because it was always just your files in your git. Read [[Why Hanji is different]] for the longer version of that promise.
