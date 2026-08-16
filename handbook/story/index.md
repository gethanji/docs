---
title: The story
order: 60
description: The manifesto, and the why behind every decision.
---


# The story

*Paper-thin documentation tool.*

Most knowledge tools are built for the moment you write in them. Hanji is built for every moment after.

We went looking for a place to keep what a team knows, and we kept finding the same shape. A store you cannot read without the app that made it. A model bundled in and metered by the token. A wall between your writing and the agents that, more and more, do the reading. In every case the knowledge was the hostage, and the software was holding it.

So we built the reverse.

Hanji keeps what a team knows as plain Markdown, in a git repository you already own. People read and write through a quiet, well-set web front. Coding agents read and contribute through the same git and a small protocol, at the speed of a local clone. Nothing is bundled. Nothing is metered. Nothing is locked away where only one program can reach it.

That is the whole product in one move. Git is the store, and everything else is a face on top of it.

## Two faces

One face is for people. Pages are Markdown, rendered with care, on a warm sheet meant for long reading. You enter edit mode, make a change, and press Update. That update is a single commit with a full history behind it, and git never once shows itself unless you go looking for it.

The other face is for agents. They already live in git, so Hanji meets them there, plus a small MCP server for scoped search and reading. When an agent wants to write, it does not edit your page. It proposes a change as a branch, and a person decides. An agent can suggest. You merge.

## A lock on the sensitive folders

Some folders hold meeting notes and decisions that are not for everyone. Every read Hanji serves, search and links and the agent tools alike, runs through one permission check, so a reader only ever sees what they are allowed to see. We are honest about what that lock protects today and what it does not. A promise we cannot keep is worse than no promise at all.

## Paper-thin

The name is also a claim about weight. Reads come off a local clone and a small local index, not a round-trip to someone's cloud, so the front stays quick and ships almost nothing to the browser. It is meant to get out of the way. See [[Performance]] for the numbers.

## Why "Hanji"

한지 is Korean paper that has carried writing for a thousand years. The name is a promise about longevity: software comes and goes, and the paper should outlast the app. Read [[The name]] for where it comes from, [[What Hanji is]] for how it works, [[Why Hanji is different]] for what it refuses to do, and [[Mission and vision]] for where it is going.

> Your agents already live in git. Hanji gives the humans a front and the sensitive folders a lock.

---

The rest of the story, in reading order:

1. [[The name]] - 한지, the paper this is named for.
2. [[Why Hanji is different]], how it lands differently from other tools.
3. [[Is Hanji for you?]], an honest look at fit, with better options where they exist.
4. [[Mission and vision]], why it exists and where it is going.
5. [[Principles]], the beliefs that break the ties.
6. [[The brand]] - the visual language and the voice, for anyone writing as Hanji.
