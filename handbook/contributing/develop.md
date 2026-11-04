---
title: Developing
order: 10
description: Running the project, and the tests that guard it.
---

# Developing

The project is a pnpm workspace: shared packages, web products and a desktop shell, with TypeScript and Rust. Here is how to run it and how to keep it honest.

## Get set up

- [ ] Obtain an authorized source checkout
- [ ] `pnpm install` (this builds the native SQLite driver, so you need a C toolchain)
- [ ] Set the environment from [[Install Hanji]]
- [ ] `pnpm dev:web` and `pnpm dev:mcp` to run the two surfaces
- [ ] `pnpm typecheck` and `pnpm test` before you commit
- [ ] `pnpm test:gates` when you touched the search engine: the suite, then the latency tripwire on its own

## The tests that matter

Run the full suite with `pnpm test`. It covers serialization, authorization, real Git operations, product APIs, canvas behavior and the desktop shell. Two guards are worth calling out.

**The corpus gate.** The serializer's promise is guarded by round-tripping real Markdown files and asserting byte-identity. It is off by default and turns on with `KB_CORPUS_DIR`. It must stay green, and it never gets weakened to make a change pass.

```bash
KB_CORPUS_DIR=/path/to/markdown pnpm vitest run packages/serializer/test/corpus.test.ts
```

**The permission invariant.** Every read flows through one module, and the tests probe it with hostile inputs to make sure a scope can never leak. If you touch reads, you keep them on that one path. Read [[The core]].

**The search bench.** Relevance and latency are measured against judged query
sets that live outside the repo, under `HANJI_BENCH_DIR`: one
`judged-<name>.json` per corpus, each naming the Hanji data dir it is judged
against. The probe prints MRR, hit@1/5/20, the zero-result rate and p50/p95
latency for every set. `HANJI_BENCH_EXCLUDE` holds a term the corpus uses
often (` -foo`, leading space and all), which adds the excluded-term timing
line; without it that line is skipped.

```bash
HANJI_BENCH_DIR=/path/to/bench pnpm exec tsx packages/core/bench/probe.ts
```

## A known gap

The type checker and the unit suite never open the database through the Next app, so a class of runtime binding bug can ship green. A boot smoke test is on the [[Where we are]]. Until it lands, run the front and load a page before you trust a change to the web build.

Read [[Conventions]] for how the code is kept.
