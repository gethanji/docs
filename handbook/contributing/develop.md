---
title: Developing
order: 10
description: Running the project, and the tests that guard it.
---

# Developing

The project is a pnpm workspace: four packages and one web app, strict TypeScript throughout. Here is how to run it and how to keep it honest.

## Get set up

- [x] Clone the repo
- [ ] `pnpm install` (this builds the native SQLite driver, so you need a C toolchain)
- [ ] Set the environment from [[Install Hanji]]
- [ ] `pnpm dev:web` and `pnpm dev:mcp` to run the two surfaces
- [ ] `pnpm typecheck` and `pnpm test` before you commit

## The tests that matter

Most of the suite is ordinary unit tests, a little over 200 of them, run with `pnpm test`. Two guards are worth calling out.

**The corpus gate.** The serializer's promise is guarded by round-tripping real Markdown files and asserting byte-identity. It is off by default and turns on with `KB_CORPUS_DIR`. It must stay green, and it never gets weakened to make a change pass.

```bash
KB_CORPUS_DIR=/path/to/markdown pnpm vitest run packages/serializer/test/corpus.test.ts
```

**The permission invariant.** Every read flows through one module, and the tests probe it with hostile inputs to make sure a scope can never leak. If you touch reads, you keep them on that one path. Read [[The core]].

## A known gap

The type checker and the unit suite never open the database through the Next app, so a class of runtime binding bug can ship green. A boot smoke test is on the [[Where we are]]. Until it lands, run the front and load a page before you trust a change to the web build.

Read [[Conventions]] for how the code is kept.
