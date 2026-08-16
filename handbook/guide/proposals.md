---
title: Proposals
order: 45
description: Agents propose. You review the diff and merge, right in the front.
---

# Proposals

This is the page where Hanji stops being a wiki and becomes the thing it was built to be. An agent never edits your pages in place. It proposes a change, the proposal lands in the front, and you decide. The whole loop runs over git, and you never have to see that.

## The loop

1. An agent with `read+propose` scope calls `hanji_propose` (see [[Agents]]). Behind the scenes that becomes a `hanji/*` branch on the mount's remote, carrying one commit with the agent's change.
2. A card appears in the sidebar, under the search box: how many proposals are waiting, and **Review**. It exists only while something waits; at zero the sidebar stays pure content. The count is cheap to compute, and it refreshes whenever anything talks to the remote.
3. Open the proposal. You get the title the agent gave it, who proposed it, which page it touches, and a line diff: green for what it adds, red for what it removes.
4. Press **Merge** or **Reject**. That is the entire ceremony.

<figure><img src="assets/proposal-review.png" alt="A proposal open for review: title, proposer, the line diff, and Reject and Merge buttons"><figcaption>An agent's proposal, waiting. The diff is the whole conversation.</figcaption></figure>

<figure><img src="assets/merge.gif" alt="Opening a pending proposal, pressing Merge, and landing on the merged page with the agent's byline"><figcaption>Merge lands it: the page updates, and the byline says who really wrote it.</figcaption></figure>

## People propose too

Agents are not the only ones held to review. Mount a repository with `--suggest` and the human front follows the same rule: the editor works exactly as everywhere else, but **Update becomes Propose**, and pressing it opens a branch instead of committing. The proposal lands in the same card, gets the same diff, and waits for the same Merge. One review ceremony for every writer, whatever species they are.

## What Merge does

Merge lands the proposal on the page: a merge commit on the base branch, the proposal branch deleted, the page and the search index updated immediately. The agent's own commit survives in history with its name on it, so `git blame` a year from now still tells the truth about who wrote what.

## What Reject does

Reject deletes the branch. Nothing else changes, nothing lingers, and the page never knew it was threatened.

## When the page moved underneath

If someone edited the page after the agent proposed, the two versions may no longer merge cleanly. Hanji checks before you do anything: the proposal opens with a warning and Merge is disabled. There is no conflict-resolution screen, on purpose. The honest move is to ask the agent to re-propose from the current page, or reject. A merge you have to untangle by hand defeats the point of reviewing calmly.

## The honest notes

- The review surface is owner-only. Agents can propose all day; only you merge.
- A proposal is a branch on the mount's remote, nothing more. There is no separate proposal database to back up or migrate. Walk away and the proposals walk with the repo.
- Local mounts (a working directory on disk) have no proposal surface. Proposals need a remote to hold branches, so they live on clone mounts.
- One page per proposal in this version, which is exactly what `hanji_propose` produces.

Read [[Agents]] for minting the token that can propose, and [[Permissions]] for the scopes behind it.
