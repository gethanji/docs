---
title: Why Hanji is different
order: 30
description: The choices behind Hanji’s files, agent access and human surfaces.
---

# Why Hanji is different

Hanji approaches the workspace as research into human–agent interaction. The aim is to make context, contributions and decisions understandable enough for people to exercise judgment. The choices below support that work in today's products.


Hanji began with a practical need: people should be able to read and improve
knowledge that coding agents already use, without losing the files or the access
boundary. That idea now spans a canvas, a planning surface and a document reader.

## Keep the work portable

Document’s content is Markdown in repositories you control. A small page edit
should leave a small, readable diff. Plan keeps issues and projects in files.
Think uses a live journal and automatic Markdown/Git checkpoints so a canvas can
respond smoothly while accepted changes remain durable.

Portability does not mean every piece of state is in Git. Accounts, grants,
comments, sessions and uncheckpointed canvas changes need their durable stores
backed up too. The search index is a rebuildable cache; those stores are not.

## Give people the right surface

Ideas do not always start as a finished page or a fully specified task. Think
leaves space to arrange and connect them. Plan gives a next step structure.
Document keeps the context worth returning to. Shared appearance and navigation
keep those changes of surface familiar.

## Treat agents as scoped participants

The conductor is a standing conversation across the three products. It brings
context into a request and acts through the rights and scopes it has. Some
changes can be applied directly, some ask for approval, and some arrive as a
proposal for a person to review. Visible activity and supported Undo help make
those actions understandable. [[The conductor]] explains the precise behavior.

Hanji supplies no model account and meters no product feature by tokens. Provider
subscriptions and authentication remain with the tools you connect.

## Say what the boundary protects

Search, links, agent reads and publication must respect the configured access
model. The product’s checks do not hide files from someone who already has direct
repository access. [[Permissions]] documents the current checks and limits.

These are Hanji’s design choices, not a claim that no other tool shares them.
The current desktop is still an invited preview. [[Is Hanji for you?]] describes
that fit, and [[Where we are]] separates what exists from future direction.
