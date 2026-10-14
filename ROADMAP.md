# Redis Mastery Roadmap: 6 weeks, about 1-2 hours a day

> Visual version: [diagrams/00-learning-roadmap.excalidraw](diagrams/00-learning-roadmap.excalidraw)

## The daily loop

Every study day follows the same loop. Don't skip the **predict** step: that's where the learning happens.

```
1. Read     docs/hinglish/<topic>.md            intuition first (15 min)
2. Run      npm run lesson <n> -- --step         watch Redis do it (20 min)
3. Revise   docs/hinglish/lesson-notes/NN_*.md   match YOUR output with the note (5 min)
4. Draw     diagrams/<nn>-*.excalidraw           open it, then redraw from memory (10 min)
5. Deepen   docs/system-design/<topic>.md        the rigorous version (20 min)
6. Measure  npm run bench <n>                    PREDICT first, then measure (10 min)
7. Build    npm run project <n>                  read code + its NOTES.hinglish.md, do 1 "Level up" (30 min)
8. Write    notes/day-NN.md                      what surprised you? in your own Hinglish (5 min)
```

Legend: **L** = lesson (`npm run lesson N`), **B** = benchmark (`npm run bench N`),
**P** = project (`npm run project N`), **SD** = `docs/system-design/`, **HI** = `docs/hinglish/`,
**D** = `diagrams/`, **IN** = `docs/insights/`.

---

## Day 0: Setup (1 hour)

- [ ] Start Docker Desktop, then `docker compose up -d`
- [ ] `npm install` (Node 20+; installs ioredis, TypeScript, tsx)
- [ ] `npm run lab` lists everything · `npm run lesson 1` runs your first lesson
- [ ] Open `D/00-learning-roadmap` in Excalidraw
- [ ] Create `notes/day-00.md` from the template in [notes/README.md](notes/README.md)
- [ ] Optional: `npm run up ui` and open RedisInsight at http://localhost:5540 (add DB `redis-lab:6379`)

---

## Week 1: Foundations. *Goal: think in data structures and round trips.*

| Day | Focus | Do |
|---|---|---|
| 1 | Keys, RESP, the event loop | HI 01 · **L01** · D 01, 02 · SD 01 (first half) |
| 2 | Strings and atomic counters | HI 02 (strings) · **L02** (watch the race-condition demo) |
| 3 | Lists as queues | **L03** · **B01** pipelining (predict the speedup first!) |
| 4 | Hashes and sets | **L04**, **L05** · D 03 · SD 02 (encodings) |
| 5 | First project | **P06** URL shortener: read every file, do 2 "Level up" tasks |
| 6 | Review | Redraw D 01-03 from memory. Write a one-page "which data type when" cheat sheet |
| 7 | Rest / catch up | |

**Checkpoint (answer out loud, in Hinglish and in English):**
- Why is single-threaded Redis fast, and what would make it slow?
- Why is `INCR` safe under concurrency when `GET` + `SET` isn't?
- What does pipelining save, and what *doesn't* it give you?
- When does a hash stop being a listpack, and why should you care?

---

## Week 2: Advanced types. *Goal: pick the right structure for a product feature.*

| Day | Focus | Do |
|---|---|---|
| 8 | Sorted sets | **L06** · **P03** leaderboard (100k players, measure rank lookups) |
| 9 | Bitmaps, HyperLogLog, Geo | **L07** · **B03** memory layouts |
| 10 | Streams | HI 11 · **L08** · D 13 |
| 11 | Pub/Sub and keyspace events | **L09** · D 14 · SD 11 |
| 12 | Sessions and serialization | **P07** session store · **B02** JSON vs hash vs pickle |
| 13 | Review | HI 13 (basics section) · design a "real-time analytics dashboard" on paper (DAU, uniques, top pages) |
| 14 | Rest / catch up | |

**Checkpoint:**
- Leaderboard with ties: how do you make "earlier achiever ranks higher" work?
- Count unique visitors for 100M users per day: exact vs HLL, memory for each?
- Pub/Sub vs Streams: what happens to a message when the consumer is offline?

---

## Week 3: Patterns and atomicity. *Goal: build correct things under concurrency.*

| Day | Focus | Do |
|---|---|---|
| 15 | Transactions, WATCH, Lua, Functions | **L10** · **B05** (see the naive approach oversell!) |
| 16 | Round trips and clients | **L11** · **B07** concurrency · re-run B01 and compare autopipelining vs explicit pipelines |
| 17 | Caching | HI 03, 04 · SD 03, 04 · D 04, 05, 06 · **P01** cache-aside (stampede demo) |
| 18 | Rate limiting | HI 09 · SD 09 · D 11 · **P02** five algorithms |
| 19 | Distributed locks | HI 10 · SD 10 · D 12 · **P04** (reproduce the unsafe-release bug) |
| 20 | Review | Write an RFC: "Introducing a Redis cache for the product catalog" using the template in IN `senior-engineer-mindset.md` |
| 21 | Rest / catch up | |

**Checkpoint:**
- MULTI doesn't roll back. So what *does* it guarantee?
- When do you choose Lua over WATCH? Over Functions?
- Why delete the cache key on write instead of updating it?
- Explain the fencing token problem with a GC pause, using a drawing.

---

## Week 4: Internals. *Goal: predict memory, latency, and durability before you deploy.*

| Day | Focus | Do |
|---|---|---|
| 22 | Memory and encodings | **L12** · re-run **B03** · SD 02 (internals half) |
| 23 | Expiry and eviction | HI 05 · **L13** · D 07 · SD 05 |
| 24 | Persistence | HI 06 · **L14** · D 08 · SD 06 · do the `docker kill` durability experiment the lesson prints |
| 25 | Blocking and raw throughput | **B04** big keys · **B06** redis-benchmark · SD 01 (second half) |
| 26 | Numbers and anti-patterns | IN `numbers-to-know.md`, `anti-patterns.md` · do the capacity-estimation exercises |
| 27 | Review | Explain RDB vs AOF trade-offs in 2 minutes · redraw D 07, 08 |
| 28 | Rest / catch up | |

**Checkpoint:**
- Your dataset is 20 GB. How much RAM do you provision, and why?
- `allkeys-lru` vs `volatile-lru` vs `noeviction`: pick one for a cache, a queue, and a session store.
- What exactly can you lose with `appendfsync everysec`? With RDB only?
- Why does `UNLINK` exist?

---

## Week 5: Distributed Redis. *Goal: reason about failure, consistency, and sharding.*

| Day | Focus | Do |
|---|---|---|
| 29 | Replication and Sentinel | `npm run up replication` · HI 07 · **L15** · D 09 · SD 07 |
| 30 | Failure drill | `docker stop repl-master` → watch `docker logs -f sentinel-1` → measure how long writes fail → `docker start repl-master` (it comes back as a replica!) |
| 31 | Cluster | `npm run up cluster` · HI 08 · **L16** · D 10 · SD 08 |
| 32 | Durable job queue | **P05** job queue on Streams (crash a worker, watch recovery) |
| 33 | Real-time | **P08** realtime chat (try `npx tsx cli.ts` in 2 terminals) |
| 34 | Review | Design a notification system (Streams + Pub/Sub + rate limiting) on one page |
| 35 | Rest / catch up | |

**Checkpoint:**
- Can a write acknowledged by the master be lost? When? What does `WAIT` change?
- What's quorum vs majority in Sentinel?
- Why must related keys share a `{hash tag}` in Cluster, and what's the danger?
- MOVED vs ASK: which one updates the client's slot map?

---

## Week 6: Production and mastery. *Goal: operate it, debug it, defend a design.*

| Day | Focus | Do |
|---|---|---|
| 36 | Observability and security | **L17** · D 16 · IN `debugging-runbook.md` |
| 37 | Production architecture | SD 12 · D 15 · IN `production-checklist.md` · HI 12 |
| 38 | Capstone (day 1) | **P09** flash sale: read README, do capacity estimation, design keys |
| 39 | Capstone (day 2) | Implement `flash-sale.ts` until `npx tsx check.ts` passes |
| 40 | Capstone (day 3) | Level ups: hot-key bucketing, reaper, load test, write a postmortem of a bug you hit |
| 41 | Design rounds | SD 13 case studies · HI 13 interview prep · mock: explain 3 designs in 10 min each |
| 42 | Retrospective | Write "Redis in production: my one-pager" in `notes/` and compare with week 1 notes |

**Final checkpoint (you're done when you can):**
- [ ] Debug "Redis is slow" using only `INFO`, `SLOWLOG`, `LATENCY`, `CLIENT LIST`
- [ ] Size a Redis deployment for a given workload, with headroom reasoning
- [ ] Choose persistence, eviction and topology for cache vs queue vs session store
- [ ] Implement cache-aside with stampede protection, a rate limiter, a lock with fencing, and a stream consumer with retries/DLQ from scratch
- [ ] Explain Redlock's critique and when a Redis lock is enough
- [ ] Defend a Redis design in a system design interview

---

## Progress tracker

**Lessons:** L01 [ ] · L02 [ ] · L03 [ ] · L04 [ ] · L05 [ ] · L06 [ ] · L07 [ ] · L08 [ ] · L09 [ ] · L10 [ ] · L11 [ ] · L12 [ ] · L13 [ ] · L14 [ ] · L15 [ ] · L16 [ ] · L17 [ ]

**Benchmarks (prediction written?):** B01 [ ] · B02 [ ] · B03 [ ] · B04 [ ] · B05 [ ] · B06 [ ] · B07 [ ]

**Projects (+ 1 level-up each):** P01 [ ] · P02 [ ] · P03 [ ] · P04 [ ] · P05 [ ] · P06 [ ] · P07 [ ] · P08 [ ] · P09 capstone [ ]

**Diagrams redrawn from memory:** 01 [ ] · 04 [ ] · 06 [ ] · 07 [ ] · 08 [ ] · 09 [ ] · 10 [ ] · 12 [ ] · 13 [ ]

---

## Other tracks

**2-week fast track** (already comfortable with basic Redis):
L01, L06, L08 → B01, B04, B05 → L10-L14 → P01, P02, P04 → L15, L16 → P05 → L17 → IN all → P09.

**Interview-prep track (1 week):** HI 13 every day · SD 03, 04, 07, 08, 09, 10, 13 · diagrams 04, 06, 09, 10, 11, 12, 17 · P02, P04 code walkthroughs · IN `numbers-to-know.md`.

## Beyond week 6

- Read the source: `src/ae.c` (event loop), `src/server.c` → `processCommand()`, `src/t_zset.c` (skiplist), `src/dict.c` (incremental rehash), `src/evict.c`
- Redis 8 Query Engine (secondary indexes, vector search) and vector sets
- Run the same lessons against **Valkey** (`image: valkey/valkey`) and compare
- Managed Redis (ElastiCache / MemoryDB / Azure Cache / Redis Cloud): read their failover and backup docs
- Write a blog post explaining one concept in Hinglish. Teaching is the final exam.
