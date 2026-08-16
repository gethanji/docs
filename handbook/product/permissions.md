---
title: Permissions
order: 20
description: The two-plane model, and exactly what it enforces today.
---

# Permissions

Permissions are where most knowledge tools get honest too late. Hanji tries to get honest early, which means telling you plainly what the lock does today and what it does not do yet.

## The model

Two planes. One is an open human front, where a team reads and writes in the clear. The other is the set of sensitive areas that must stay closed, the meeting notes and the decisions and anything you would not tack to a shared wall. The whole design rests on one rule: **every surface honors the same check**. Search, wikilinks, page reads, and the agent tools all ask the same question before they show you anything, so there is no back door where a locked title slips out through search, or a link resolves to a page you were never allowed to open.

That is not a claim to make lightly, so here is how it actually works.

## One read path

Every read Hanji serves runs through a single module. The scope check, which mounts may this reader see, is compiled in exactly one place and nowhere else. Search filters to your readable mounts before it runs. A page read returns nothing for a mount you cannot see. Wikilink resolution uses the same filter. This is the boring, load-bearing part. There is one door, and it is the same door for people and for agents.

A locked page and a page that does not exist look identical to a reader who lacks access. Both come back as not found. You cannot probe the shape of what you are not allowed to see.

## Scopes today

Access is granted per mount, in two levels.

| Scope | The reader can | The reader cannot |
|---|---|---|
| none | see nothing of the mount, not even that it exists | read, search, or link into it |
| `read` | read, search, and follow links inside the mount | write, or propose changes |
| `read+propose` | everything `read` can, plus propose changes as a branch or PR | merge, or write directly |

The owner sees every mount. An agent gets a bearer token whose scopes take the same shape as a person's grants - a whole mount, a folder, or exactly one page - so an agent that helps with the engineering docs never sees the folder of sensitive notes, even when both live in the same repository. See [[Agents]] for how that works in practice.

## Visibility belongs to the content

The model reads the way Notion and Confluence taught everyone to think. Content carries a general access level - **Everyone can read**, **Everyone can suggest**, or **Restricted** - set on a mount, a folder, or a single page, flowing down to everything beneath. Open content is simply open: no per-person bookkeeping. Restricted content is dark by default, and the deepest rule covering a page wins, so one restricted folder inside an open mount goes dark for everyone.

People are the exceptions. A person granted a path - a mount, a folder, or exactly one page - punches through a restriction at or below their grant. That is the 1:1 note: a restricted folder, two names, and nobody else sees it in the tree, in search, or through a wikilink. A locked page still looks identical to a page that does not exist.

Sharing lives where the content lives: a **Share** control on every page, and on every mount and folder in the sidebar, showing the general access (and where it is inherited from), the people with specific access and through which grant, and a one-line way to add someone with read or suggest. Settings keeps the overview: a Mount access tab for general access per mount, and a People tab for everyone's grants in one place. Revoking takes effect on the person's next request. The rule and grant tables are the implementation; the page is the interface.

<figure><img src="assets/share-popover.png" alt="The Share popover on a mount: general access set to Everyone can read, and one person's grant listed beneath" class="w-75"><figcaption>Share, where the content lives: the general rule on top, the exceptions by name.</figcaption></figure>

One deliberate exception: **agents never inherit "everyone"**. A token sees exactly the scopes it was minted with, whatever the content rules say - opening a mount to your team never opens it to a bot.

## People, same scopes

The same two levels now apply to humans. The owner adds a person in Settings with a name, a password, and per-mount scopes: `read` makes a **viewer**, `read+propose` makes a **contributor** whose Update reads Propose and lands in the owner's review card. A user never commits to base, never restructures the tree, and never sees review or settings - those stay the owner's. In user terms: viewers read, contributors suggest, the owner decides. One read path, one review ceremony, whatever species the writer is.

## What is honest about it

The unit of permission is now the path, down to one file, for people and agent tokens alike, and it holds on the same single read path as everything else. What remains honest to say: product privacy is not storage privacy - the bytes are plaintext in a git repo, so the instance owner and the git host can always read them. A 1:1 note is hidden from every other principal; hiding it from the owner would take cryptography, not permissions. The two-plane model is real and enforced on one honest read path, and [[Where we are]] tracks the rest without rounding up.
