---
title: Performance
order: 40
description: Paper-thin. The speed and size numbers, measured, and the ones still pending.
---

# Performance

Paper-thin is not only a metaphor about longevity. It is also a claim about weight. A knowledge base you read all day should be quick and light, and most are neither, because every read is a round-trip to someone's cloud and every page ships a small application to your browser.

Hanji is built the other way. Reads come off a local git clone and a small local index, not a network hop. Here are the numbers, measured on the dogfood instance, with the ones we have not earned yet marked as such. Honesty about limits applies to benchmarks too.

## The agent read path

This is the one that has to be fast, because agents read constantly. Measured against the running instance over 127.0.0.1, five runs each.

| Operation | Time | What it does |
|---|---|---|
| Fetch a page | ~2 ms | Return a page's Markdown by mount and path |
| Full-text search | ~2 to 3 ms | Query the FTS5 index across every mount you can read |

Single-digit milliseconds, because there is no cloud in the loop. The agent reads from a local clone and a local index, the same way it reads your code.

## Weight and dependencies

| Thing | Number |
|---|---|
| Bundled LLM SDKs | 0 |
| AI credits or metering | none, ever |
| Web app runtime dependencies | 24, all ProseMirror, remark-rehype, React, and Next |
| Test cases | 361 |
| Real Markdown files round-tripped byte-identical | 600+ |

The zero on the first row is the point. Hanji ships no model and meters nothing, so nothing about its cost or its weight grows because your team wrote more this month.

## Weight in the browser

Measured on a clean production build, served and loaded through a real request (the `pnpm smoke` boot test).

| Thing | Number |
|---|---|
| Shared JavaScript, first load | ~103 kB |
| Reading a page, first load | ~108 kB |
| A rendered page, over the wire | ~14 kB of HTML |
| Time to first byte | ~55 ms |

Reading sits just above the shared floor. The editor is a separate chunk that does not load until you press Edit, so the common case, reading, stays paper-thin, and the writing tools arrive only when you reach for them.

The index itself is worth one honest note. It is a cache. Whatever it costs in space, you can delete it and rebuild it from git, because the repository is the only thing that has to be trusted. Read [[Byte-fidelity]] for the engine underneath, and [[Why Hanji is different]] for why none of this is bundled.
