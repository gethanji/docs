---
title: Quick start
order: 5
description: From the invited desktop preview to your first canvas, issue or page.
---

# Quick start

The quickest path is the invited Mac desktop preview. [Request access](https://hanji.ink/start/)
if you do not have a copy. Public binary downloads are not available yet, and
source access is shared separately with preview participants.

## With the desktop preview

1. Move Hanji.app to Applications and open it.
2. Choose Create your workspace, give it a name, and select Think, Plan and/or Document.
3. Choose Start fresh to let Hanji prepare folders, or bring existing content.
4. Choose an owner password, review, and create the workspace.

[[Desktop workspace]] explains each stage, recovery, and the workspace shell.

![The workspace setup](assets/onboarding-workspace.png)

## Make the first thing

- **Think:** create a canvas, add a sticky, and connect it to another idea.
- **Plan:** create an issue; a clear title is enough to start.
- **Document:** open the welcome page, edit a sentence and save it.

Your files stay local. The conductor is optional: configure your own agent later
through Settings. See [[The conductor]] for provider setup, context and approval.

## With an existing source checkout

Development dependencies and a working Git installation are required. The
SQLite native module must match the Node runtime used to run the checkout.
Follow that checkout’s prerequisites, then:

```bash
pnpm install
pnpm dev:web   # → http://localhost:4100
```

The first visit uses the same guided setup. [[Install Hanji]] covers explicit
configuration for a source checkout or server. These commands assume access to
the source; they are not a public download link.

## Where next

[[Think canvas]], [[Objectives and key results]], [[Reading and writing]] and
[[Workspace insights]] cover the daily tools. [[Mounts]] adds existing content,
and [[On your tailnet]] shares it with people you authorize.
