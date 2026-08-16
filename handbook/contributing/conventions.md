---
title: Conventions
order: 20
description: How the code and the commits are kept.
---

# Conventions

Nothing exotic here. The conventions exist so the codebase stays boring, and boring is the goal.

## Code

- **Strict TypeScript**, everywhere, with no exceptions carved out to dodge a type.
- **Small modules with one job.** The parts of Hanji have clear seams on purpose. A file that grows into two responsibilities gets split.
- **The security-critical paths are single-sourced.** Permissions live in one module. Byte-fidelity lives in one engine. You do not add a second way to do either, because a second way is a second thing to get wrong.
- **No new dependency for what a few lines can do**, and never a dependency that pulls in a model or a meter.

## Commits

- **DCO sign-off** on every commit, `git commit -s`.
- **Clean, human history.** A commit message says what changed and why, in a person's words. No machine trailers, no noise. The point of byte-fidelity is a readable history, and the commits are held to the same bar.
- **One change per commit.** A one-word fix is a one-line diff, and the commit that carries it should be just as legible.

## Documentation

- **Docs ship with the feature.** A behavior change and its handbook pages land in the same series: the changelog says what changed, [[Where we are]] stays true, and the affected guides get swept for claims the change made false. This handbook is the product documenting itself, so a stale page is a product bug.
- **A mechanical floor is enforced by the test suite**: every environment variable the product reads is documented, every CLI command is in the [[Command reference]], every guide page is in the reading order. When that test fails, the docs are wrong, not the test.

## Reviews

Changes get reviewed, and the reviews are adversarial where they need to be, on the permission and byte-fidelity paths especially. A finding on those paths is not a nit. It is the product. Read [[Open core]] for the license this all ships under.
