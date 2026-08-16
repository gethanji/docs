---
title: Quick start
order: 5
description: A running Hanji in five minutes, on the Markdown you already have.
---

# Quick start

You have repositories full of Markdown - notes, docs, decisions that deserve
better than a file tree. In five minutes they become one quiet, readable
place: yours to write, your team's to read, your agents' to work in.

*Pre-release note: the code ships soon as one clean release. This page is
exactly what day one looks like.*

## The five minutes

```bash
# 1. get it running
git clone https://github.com/gethanji/hanji && cd hanji
pnpm install

# 2. tell it who you are
export HANJI_DATA_DIR=~/.hanji
export HANJI_SESSION_SECRET=$(openssl rand -hex 32)
export HANJI_OWNER_PASSWORD=choose-one

# 3. point it at a repo you own
pnpm hanji add-mount notes git@github.com:you/notes.git notes
pnpm hanji sync

# 4. open the front
pnpm dev:web   # http://localhost:4100
```

Sign in, and your notes are there, rendered on warm paper. Click **Edit**,
change something, press **Update** - you just made a clean git commit without
thinking about git once. That is the whole trick, and it never gets more
complicated than that.

## Where to next

Pick the shape that matches your life:

- **A team on your tailnet** - share one instance where the network signs
  people in and nobody types a password. [[On your tailnet]].
- **Your coding agents** - give them scoped reads and let their edits arrive
  as proposals you review. [[Agents]].
- **A public site** - one command turns everything you've opened into a
  website, `llms.txt` included. [[Publish to the web]].
- **Just curious how it holds together?** [[Mounts]], then
  [[Reading and writing]] - the two pages that explain the daily feel.
