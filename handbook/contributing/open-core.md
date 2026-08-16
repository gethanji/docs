---
title: Open core
order: 30
description: The license, the open-core model, and the promise about what stays free.
---

# Open core

Hanji is open core, and the model is chosen deliberately, because the license is a promise you cannot take back later without burning the people who trusted it.

## The license today

The whole project is **AGPL-3.0-only**. That is a strong copyleft license. You can run it, read it, change it, and self-host it freely, and if you offer it as a service, your changes stay open too. It is the terminal license, picked on purpose. There is no plan to relicense, because relicensing is a permanent tax on the trust of everyone who showed up early.

## The seam

Over time, a set of features for administrators will live behind a license key, the way the open-core projects of this era do it. The rule that decides which side of the seam a feature lands on is fixed.

| Free, forever | Paid |
|---|---|
| The complete single-team knowledge base | Single sign-on |
| The editor, history, search | Per-folder granular permissions |
| Bring-your-own-agent, the MCP surface | Audit and enforcement |
| Import and export, the API | Priority support |

The line is simple: never gate what an individual needs to write. A student, a solo maintainer, a small team gets the whole writing experience for nothing, forever. The paid tier is for the things an administrator wants when the team gets big, and only those.

## Where it is today

The paid seam is a plan, not yet code. What ships now is the AGPL core. When the license key and the administrator features arrive, they arrive behind the seam described above, and this page will stop describing a plan and start describing a product. Read [[Mission and vision]] for the why, and the [[Where we are]] for the when.
