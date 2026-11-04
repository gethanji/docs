---
title: Performance
order: 40
description: Paper-thin. The speed and size numbers, measured, and the ones still pending.
---

# Performance

Paper-thin is not only a metaphor about longevity. It is also a claim about weight. A knowledge base you read all day should be quick and light, and most are neither, because every read is a round-trip to someone's cloud and every page ships a small application to your browser.

Hanji is built the other way. Reads come off a local git clone and a small local index, not a network hop. Here are the numbers, measured on the instance we live in daily, with the ones we have not earned yet marked as such. Honesty about limits applies to benchmarks too.

These are recorded measurements of the document/search plane on the stated
corpus, not a benchmark of the complete desktop app, canvas rendering or every
first load. Their setup matters; do not read them as a promise for another machine.

## The agent read path

This is the one that has to be fast, because agents read constantly, measured on the 34,726-page corpus (corpus A). A page fetch and the title tier are measured over 127.0.0.1 against the running instance, five runs each. The four search rows below are p50s from `packages/core/bench/probe.ts`, one timed call per corpus A bench case (42 cases), run in process rather than over the wire.

| Operation | Time | What it does |
|---|---|---|
| Fetch a page | ~3 ms | Return a page's Markdown by mount and path |
| Palette, 8 rows | 126.5 ms | Query the FTS5 index across every pages mount you can read |
| Palette, scoped to one mount | 45.1 ms | The same query for a reader who holds part of the tree |
| Results, 20 rows | 107.1 ms | The same query, paged past the palette's eight |
| Results, scoped to one mount | 43.3 ms | The same query for a reader who holds part of the tree |
| Title suggestions as you type | ~11 ms | The two-character prefix the palette fires from |

A page fetch and the title tier stay single-digit to low tens of milliseconds. A full search over this 34,726-page corpus runs about a tenth of a second unscoped and tens of milliseconds for a reader who holds part of the tree; on the 959-page corpus in the "Fifth run, 2026-09-19" sub-section below, every one of these paths stays under 25 ms.

## Indexing at scale

The first sync is the expensive moment: every page is parsed, stored, and
full-text indexed. Measured on a real corpus, on the same machine as above.

| Operation | Time | What it does |
|---|---|---|
| First sync, 34,726 pages | ~4.4 min | Clone-to-searchable, the same pipeline at stress-test size |
| Re-derive an existing index at version 2 | ~3.2 min | The one-time upgrade, run from Settings with progress |

The cost stays near-linear because index writes run in batched transactions
and the full-text rows are addressed by rowid; an earlier build paid a scan
of the whole growing index per page, which turned the 35,000-page sync into
half an hour. Re-syncs only pull what changed in git first, and the indexing
pass yields to the event loop as it runs, so a big sync never freezes the
instance serving it.

A full rebuild of the 34,726-page corpus lands at 866 MB after optimize and
VACUUM, up from 818 MB at version 1, measured on the same corpus before the
format change. The difference is the sections index: five full-text tables
where the previous format carried one, and that is plainly what bought
passage-level landing, jumping to the paragraph a query matched instead of
just the page.

## What the search numbers were measured on

The p50s on this page come from a hand-ranked bench of real queries against
real content, held outside this repository: corpus A is 34,726 pages with 36
targeted judged queries (42 cases total), corpus B is 959 pages with 55
targeted judged queries (82 cases total). Both tables above are corpus A end
to end, so no row compares a small corpus against a large one. Corpus B's
numbers, which pass every budget the spec sets, are in the "Fifth run,
2026-09-19" sub-section below rather than mixed in here. The ranking
constants that decide what ranks first were swept against both sets once and
frozen; see `packages/core/bench/RESULTS.md` for the sweep.

## Weight and dependencies

| Thing | Number |
|---|---|
| Bundled LLM SDKs | 0 |
| AI credits or metering | none, ever |
| Web app runtime dependencies | 24, all ProseMirror, remark-rehype, React, and Next |
| Test cases | 1070 |
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

## Search, measured 2026-09-18

The search engine was rebuilt from the retrieval layer up. These are the
numbers it ships with, measured against two real corpora on one laptop: a
34,726-page corpus and a 959-page one. The budgets come from the spec, and
not all of them are met yet. They are written down anyway, because a budget
you quietly drop is not a budget.

The dated sections that follow supersede this one: its query timings and its
gate verdicts were all true when measured and have all since changed. Read the
last dated section for where things stand, and this one for where they started.

### Building the index

| Corpus | Pages | Re-derive the whole index | Integrity findings |
|---|---|---|---|
| corpus A | 34,726 | 193 s | 0 |
| corpus B | 959 | 9 s | 0 |

That is a full re-derivation from stored page content, not a git sync, which
is the expensive path an index-format change takes.

| Corpus | index.db before compaction | after optimize and VACUUM | Budget |
|---|---|---|---|
| corpus A | 873,287,680 bytes (873 MB) | 852,533,248 bytes (853 MB) | 1.0 GB |
| corpus B | 82,677,760 bytes (83 MB) | 43,728,896 bytes (44 MB) | none |

Corpus A sits inside the 1.0 GB budget with about 15 percent to spare, and it
carries five full-text tables where the previous index carried one. Corpus B
halves on compaction; corpus A barely moves, because a re-derivation that
large reuses the pages the dropped tables freed instead of growing the file.

### Query latency

p50 and p95 over every judged query in each set, warm index, one process.

| Path | Corpus A p50 | Corpus A p95 | Corpus B p50 | Corpus B p95 | Budget |
|---|---|---|---|---|---|
| Palette, 8 rows | 264.4 ms | 371.9 ms | 18.3 ms | 37.4 ms | 40 ms |
| Palette, scoped to one mount | 64.5 ms | 100.4 ms | 12.0 ms | 26.0 ms | 40 ms |
| Results, 20 rows | 271.0 ms | 385.3 ms | 17.0 ms | 33.7 ms | 80 ms |
| Results, scoped to one mount | 63.7 ms | 101.1 ms | 12.8 ms | 28.6 ms | 80 ms |
| Exact counts | 468.3 ms | 663.6 ms | 18.1 ms | 37.3 ms | 150 ms |
| Exact counts, one term excluded | 560.5 ms | 777.1 ms | 18.2 ms | 37.2 ms | not budgeted |

Corpus B is comfortably inside every budget. Corpus A is over four of the five
budgeted paths, by 1.6x on the scoped palette and by as much as 6.6x on the
unscoped one. The shape of the miss is the useful part: restricting a reader
to one mount makes the same query four times faster, so the cost tracks how
much of the corpus a query can reach and not the permission filter. At 35,000
pages the retrieval pass is still proportional to the number of rows that
match, which is the next thing to fix.

There is a CI tripwire on a generated 5,000-page fixture, budgeted at 25 ms at
p50. Run on its own it holds for a two-word English query (15 to 17 ms) and for
1- and 2-character title prefixes (about 4 ms), and misses on a two-group
Korean query (35 to 38 ms), which fetches roughly twice the rows because the
Korean path adds two more candidate pools. It is not wired into CI yet for that
reason, and because timings taken inside a parallel test suite measure the
suite's other workers as much as the engine.

### Relevance

Scored against judged sets: for each query, the pages a person typing it meant,
best first. MRR is the reciprocal rank of the best target, hit@k the share of
queries with a target in the top k.

| Set | n | MRR | hit@1 | hit@5 | hit@20 | No results |
|---|---|---|---|---|---|---|
| corpus A, previous engine | 36 | 0.277 | 22.2% | 33.3% | 47.2% | not recorded |
| corpus A, this engine | 36 | 0.462 | 38.9% | 55.6% | 63.9% | 0.0% |
| corpus B, this engine | 55 | 0.566 | 41.8% | 80.0% | 87.3% | 0.0% |

On the same corpus and the same 36 judged queries, the rebuild moved MRR up 67
percent and nearly doubled hit@1. Latency at p50 went the other way by about
15 percent, from 234.9 ms to 271.0 ms: the engine now reads section text,
scores passages and decides presence in JavaScript, and that is what the
relevance was bought with. Both corpora now return something for every judged
query. The previous engine's zero-result rate was not recorded, so that last
column improves on nothing; it is a floor to hold, not a win to claim.

Counted over every query including the ones with no judged target, which can
never be hit, corpus A reads 0.396 MRR and corpus B 0.379. Those are the
honest denominators for "how often does search help at all"; the table above is
the one that compares to the older measurement.

### Second run, same day: two redundant reads removed

Two numbers were being fetched twice per search. The corpus row counts, which
are the IDF denominators, are now read once per open index instead of twice per
query, and each term's per-table index count is read once per search instead of
twice. Separately, the scorer now reads the per-section term presence that the
earlier phase already decided, instead of deciding it again. None of the three
changes what a search returns; the relevance block below is identical to the one
above, to three decimals, which is how they were checked.

| Path | Corpus A p50 | was | Corpus B p50 | was | Budget |
|---|---|---|---|---|---|
| Palette, 8 rows | 224.6 ms | 264.4 | 17.2 ms | 18.3 | 40 ms |
| Palette, scoped to one mount | **38.5 ms** | 64.5 | 10.7 ms | 12.0 | 40 ms |
| Results, 20 rows | 192.5 ms | 271.0 | 14.0 ms | 17.0 | 80 ms |
| Results, scoped to one mount | 36.8 ms | 63.7 | 10.5 ms | 12.8 | 80 ms |
| Exact counts | 379.5 ms | 468.3 | 14.7 ms | 18.1 | 150 ms |
| Exact counts, one term excluded | 475.1 ms | 560.5 | 15.0 ms | 18.2 | not budgeted |

The scoped palette path moves inside its budget, so corpus A now meets two of
the five, and every path on both corpora is faster. The three still outside are
the unscoped ones, by 5.6x, 2.4x and 2.5x. They are dominated by one index read
whose cost tracks how many rows a query matches rather than how many it returns,
and fixing that is a schema question rather than a tuning one.

Relevance, unchanged by all three:

| Set | n | MRR | hit@1 | hit@5 | hit@20 | No results |
|---|---|---|---|---|---|---|
| corpus A | 36 | 0.462 | 38.9% | 55.6% | 63.9% | 0.0% |
| corpus B | 55 | 0.566 | 41.8% | 80.0% | 87.3% | 0.0% |

The CI tripwire on the 5,000-page fixture also improved: the two-word English
query now reads 13.6 ms and the title prefixes 3.7 to 3.9 ms, all inside the
25 ms budget, and the two-group Korean query 27.4 to 28.5 ms, down from 32 to 38
but still over it. The tripwire is still not wired into CI for that one case.

### Third run, same day: the candidate pools stop sorting by date

The two indexes that find Korean and other non-spaced text are candidate
generators: they carry no relevance score, so whatever order their cut takes is
arbitrary. It took the page's edit date, which is the one order that cannot ride
an index, so a common Korean term matched 20,630 sections and all of them were
sorted to return 208. They now take the cut in index order instead, which costs
nothing to produce, and that one statement went from 63.3 ms to 0.6 ms.

| Path | Corpus A p50 | was | and before that | Corpus B p50 | Budget |
|---|---|---|---|---|---|
| Palette, 8 rows | 142.5 ms | 224.6 | 264.4 | 17.1 ms | 40 ms |
| Palette, scoped to one mount | **37.1 ms** | 38.5 | 64.5 | 10.4 ms | 40 ms |
| Results, 20 rows | 116.5 ms | 192.5 | 271.0 | 13.1 ms | 80 ms |
| Results, scoped to one mount | **35.1 ms** | 36.8 | 63.7 | 10.5 ms | 80 ms |
| Exact counts | 316.0 ms | 379.5 | 468.3 | 15.3 ms | 150 ms |
| Exact counts, one term excluded | 408.9 ms | 475.1 | 560.5 | 14.8 ms | not budgeted |

Across the three runs the palette path is 46 percent faster, the results path 57
percent, and the exact counts 33 percent, on the large corpus. Corpus B passes
every budget. Corpus A still misses the three unscoped paths, now by 3.6x, 1.5x
and 2.1x rather than 6.6x, 3.4x and 3.1x.

Relevance, unchanged again:

| Set | n | MRR | hit@1 | hit@5 | hit@20 | No results |
|---|---|---|---|---|---|---|
| corpus A | 36 | 0.462 | 38.9% | 55.6% | 63.9% | 0.0% |
| corpus B | 55 | 0.566 | 41.8% | 80.0% | 87.3% | 0.0% |

Worth one honest note: identical scores do not mean identical results. The new
cut changes which candidates survive on 13 of the 42 judged queries, 50 pages in
all. Every one of them was a page no judge had named, so no target moved, which
is the evidence that the old order was not buying relevance. It is not evidence
that the cut never matters.

The latency tripwire now passes, and it is wired into a gate rather than into
CI: `pnpm test:gates` runs the suite and then the tripwire. There is no CI
workflow in this repository at all, so nothing runs either of them on a push;
the gate is what a person or an agent runs before a commit. The tripwire lives
in its own file and runs alone, with file parallelism off, as `pnpm --filter
@hanji/core test:latency`; it is kept out of the default test run on purpose,
because a timing assertion sharing a machine with the rest of the suite
measures the suite. On the
5,000-page fixture, three runs: a two-word query 13.7 to 14.3 ms unscoped and
12.5 to 13.0 scoped, a two-group Korean query 23.9 to 24.7 ms, and 1- and
2-character title prefixes 3.7 ms. Every budget here follows one rule: about
1.5x the case's own isolated p50, rounded up to the next 5 ms. The title
prefixes keep the 25 ms that gives them, clearing it by about seven times
over. The two-word cases and the Korean case now sit at 30 ms and 40 ms
instead, once the fourth run's constant-depth pooling raised their own
measurements to 19.9 ms and 26.8 ms: at the old 25 ms and 35 ms they would
clear by only a few percent, and a tripwire with that little headroom flakes
on a busy machine long before it catches a real regression. What the Korean
budget defends against is a regression of 40 percent or more on that shape.

### Fourth run, 2026-09-18: one candidate set per query

The pool depth was the offset plus the page size, so page 2 pooled deeper than
page 1 and could retrieve a candidate ranking above a row page 1 had already
served. Walking a 700-page fixture twenty rows at a time served 96 pages twice
and never reached 306 of the 700. Any request that pages, or asks for more rows
than the palette's eight, now pools at the cap, so every page of one query is a
slice of one candidate set. The palette keeps the shallow pool: it never pages,
and its budget is the tight one.

That is retrieval depth bought with time. The row that says so is the same run
with the old depth, measured minutes apart on the same machine, because the
numbers below are about 10 percent slower than the third run's across the board
including on the palette, which this change cannot touch.

| Path | Corpus A p50 | same run, old depth | Corpus B p50 | same run, old depth | Budget |
|---|---|---|---|---|---|
| Palette, 8 rows | 159.3 ms | 149.5 | 15.4 ms | 16.2 | 40 ms |
| Palette, scoped to one mount | 56.9 ms | 51.6 | 9.5 ms | 9.9 | 40 ms |
| Results, 20 rows | 136.6 ms | 126.7 | 22.5 ms | 12.1 | 80 ms |
| Results, scoped to one mount | 54.5 ms | 50.5 | 15.1 ms | 9.4 | 80 ms |
| Exact counts | 367.5 ms | 342.9 | 24.8 ms | 14.3 | 150 ms |

On corpus B the cost is plain: the results path went from 12.1 ms to 22.5 ms and
the exact counts from 14.3 to 24.8, because on a 959-page corpus a 500-row pool
is a large share of everything there is to fetch. On corpus A the results path
moved 7.8 percent, which is what the palette moved in the same pair of runs
without being touched, so at that size the change is at the machine's noise
floor: the pool is bounded by how many rows match, not by the cap. Corpus B
stays inside every budget. Corpus A misses four this time rather than three: the
three unscoped paths, and the scoped palette, which read 37.1 ms in the third run
and reads 51.6 ms in this run's old-depth control as well, so what moved it is
the machine and not the change.

Relevance did NOT stay identical this time, and that is the point of the change:

| Set | n | MRR | hit@1 | hit@5 | hit@20 | No results |
|---|---|---|---|---|---|---|
| corpus A | 36 | **0.472** | 38.9% | **58.3%** | **69.4%** | 0.0% |
| corpus B | 55 | 0.566 | 41.8% | 80.0% | 87.3% | 0.0% |

Corpus A read 0.462 MRR, 55.6 percent hit@5 and 63.9 percent hit@20 for three
runs; what moved them is judged targets the shallow pool cut before anything
could rank them. Over every query including the untargeted ones, corpus A reads
0.405 against 0.396 and corpus B 0.380 against 0.379. Corpus B's judged block is
identical to three decimals, which is the check the previous three runs were held
to: its 959 pages were almost always inside the shallow pool already. The
old-depth control run reproduced all six of the previous numbers exactly, which
is how this change was attributed rather than assumed.

The latency tripwire on the 5,000-page fixture still passes every case at its
budget. The same constant paging depth that moved the benchmark table above
also moved the tripwire's own two-word gate cases, from about 13 ms to about
20 ms.

### Fifth run, 2026-09-19: rebuild and exact-pass numbers re-taken

A `hanji rebuild` over the same 34,726-page corpus, git tree included, took
263 s: about 70 s longer than the 193 s `reindexAll` measured earlier for the
same page count, because `rebuild` re-reads and re-parses every file from the
mounted tree first and `reindexAll` only re-derives the index from content
already stored in `pages`.

Corpus A's palette, results path and exact pass, against the spec's budgets:

| Path | Corpus A p50 | Budget | Meets it |
|---|---|---|---|
| Palette, 8 rows | 126.5 ms | 40 ms | no |
| Palette, scoped to one mount | 45.1 ms | 40 ms | no |
| Results, 20 rows | 107.1 ms | 80 ms | no |
| Results, scoped to one mount | 43.3 ms | 80 ms | yes |
| Exact counts | 285.2 ms | 150 ms | no |

Corpus B, 959 pages, every path:

| Path | Corpus B p50 | Budget | Meets it |
|---|---|---|---|
| Palette, 8 rows | 14.9 ms | 40 ms | yes |
| Palette, scoped to one mount | 8.7 ms | 40 ms | yes |
| Results, 20 rows | 20.9 ms | 80 ms | yes |
| Results, scoped to one mount | 15.1 ms | 80 ms | yes |
| Exact counts | 22.2 ms | 150 ms | yes |

Corpus B meets every budget the spec sets. Corpus A meets one of the five
(the scoped results path); the other four miss by 3.2x, 1.1x, 1.3x and 1.9x
(palette, scoped palette, results, the exact pass).

The exact pass on corpus A is the widest miss here, at roughly 1.9x its
150 ms budget. There is no "500+" fallback on the results page for this: the
exact pass always runs inline, and the count line reads "N results, M match
every term" when the two differ, or a plain "N results" when they agree. A
list-first variant, rendering the list before the counts resolve, is the
recorded post-launch option for hiding that latency. The exact pass's
transient heap at 35,000 pages is the other recorded residual: the unbounded
pass holds
every matching row to compute `total` and `totalAny`, and that has not been
profiled or bounded yet.

See `packages/core/bench/RESULTS.md` for the ranking sweep these numbers were
measured against. That sweep moved corpus A from 0.472 to 0.490 MRR and
corpus B from 0.566 to 0.579.

### Acceptance checks

| Check | Result |
|---|---|
| A large attachment manifest does not move a title-exact match | pass |
| A term matching mid-word ranks below one matching at a word start | pass, over the returned page of 20 |
| A glued mixed-script term does not widen to pages holding its halves apart | pass |

The index itself is worth one honest note. It is a cache. Whatever it costs in space, you can delete it and rebuild it from git, because the repository is the only thing that has to be trusted. Read [[Byte-fidelity]] for the engine underneath, and [[Why Hanji is different]] for why none of this is bundled.
