---
title: Why Hanji is different
order: 70
description: What Hanji refuses to do, and why that is the point.
---

# Why Hanji is different

Open five knowledge tools and four of them share the same shape. The knowledge lives in a database only their app can read, intelligence comes bundled and metered so the more your team writes the more you pay, permissions are an afterthought bolted to the side, and leaving is a migration project because your writing was never really yours to carry.

Hanji is built against that shape, on purpose. Here is where it lands differently.

| | The common shape | Hanji |
|---|---|---|
| Where knowledge lives | A proprietary store, read through the app | Plain Markdown in a git repo you already own |
| Intelligence | A bundled model, metered by the token | Bring the agent you already trust. None bundled, nothing metered |
| Who reads it | People, with agents bolted on as a chat box | People and agents, both first-class, both scoped |
| Permissions | One flat space, or an afterthought | Two planes: an open human front, locked sensitive folders |
| Edit history | Opaque blocks, a diff you cannot read | Byte-fidelity: a one-word edit is a one-line change |
| Leaving | An export, a migration, a loss | Nothing to do. It was always your files in your git |

None of these axes is novel on its own. Plenty of tools keep files in git. A few take permissions seriously. The point is holding four things at once, git-backed storage, agent-native access, a polished human front, and real permissions, and refusing to trade any of them away for a feature. As of today, nothing else holds the whole set. That is the gap Hanji was built to sit in.

## Git is the store, not a backup

Most tools that "support git" treat it as an export target. The truth lives in their database, and git gets a copy when you remember to sync. Hanji reverses that. The repository is the source of truth, and the index is a cache we can throw away and rebuild at any time. That single choice is what makes leaving free. There is no export, because there was never anything to export from.

## Agents are readers, not a chat box

More than half of documentation today is read by a machine, not a person. Most tools answer that by adding a chat box that reads everything and replies in a blur. Hanji treats an agent the way it treats a person: a reader with an identity and a scope. It reads through git at the speed of a local clone, and when it wants to write, it proposes a branch. A person still decides. Your knowledge base does not quietly get rewritten by a model at 3am.

## The lock is honest about its limits

A two-plane model is simple to say and hard to do. The human front is open and inviting, the sensitive folders are locked, and every surface honors the lock. Search will not leak a title. A wikilink will not resolve to a page you cannot open. The agent tools see exactly what you see, nothing more. And where the lock has a limit, we say so plainly, because a promise we cannot keep is worse than none.

No bundled model. No credits, ever. That line is a promise, and every choice above is what keeps it.

Read [[Principles]] for the beliefs underneath these choices, [[Is Hanji for you?]] for an honest look at where something else fits better, and [[What Hanji is]] for how they fit together.
