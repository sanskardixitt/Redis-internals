# Redis Internals Lab

> **New here? Open [START-HERE.md](START-HERE.md)** and press `Ctrl+Shift+V`. It's a clickable path through every file, in learning order.

Learn Redis the way senior engineers know it: **run a file, watch Redis respond, measure, then build.**
Node.js + TypeScript, real Redis 8 in Docker (standalone, replication + Sentinel, and a 6-node cluster),
Excalidraw diagrams, and every concept explained twice: rigorous English and intuitive Hinglish.

```
$ npm run lesson 3

[4] Reliable queue: BLMOVE into a processing list, ack, recover
----------------------------------------------------------------------------------------
  redis> BLMOVE l03:q:sms l03:q:sms:processing RIGHT LEFT 1
  "{"id":"otp-1","to":"987650001"}"
  redis> SET l03:q:lease:otp-1 worker-A EX 1
  OK
  redis> LREM l03:q:sms:processing 1 "{\"id\":\"otp-1\",\"to\":\"987650001\"}"
  (integer) 1
  ...
  +-- INSIGHT -------------------------------------------------------------------------
  | otp-2 was delivered twice: reliable queues give AT-LEAST-ONCE delivery, never
  | exactly-once. The consumer must be idempotent (e.g. SET sent:otp-2 NX before
  | sending the SMS). BLMOVE/LMOVE (formerly BRPOPLPUSH) makes the hand-off atomic:
  | the job is always in exactly one list, even if the worker dies mid-command.
  +-------------------------------------------------------------------------------------
```

## What's inside

| | Count | Where | Run it |
|---|---|---|---|
| Lessons: guided, traced, measured | 17 | [lessons/](lessons/) | `npm run lesson 1` |
| Benchmarks: predict, measure, explain | 7 | [benchmarks/](benchmarks/) | `npm run bench 1` |
| Mini projects: industrial patterns | 8 + capstone | [projects/](projects/) | `npm run project 1` |
| Excalidraw diagrams | 18 | [diagrams/](diagrams/) | open in excalidraw.com / VS Code |
| System design docs (English) | 13 | [docs/system-design/](docs/system-design/) | read |
| Concept explainers (Hinglish) | 13 | [docs/hinglish/](docs/hinglish/) | read |
| Hinglish revision notes: every lesson + project | 17 + 9 | [docs/hinglish/lesson-notes/](docs/hinglish/lesson-notes/README.md), `projects/*/NOTES.hinglish.md` | read after running |
| Industry insights | 8 | [docs/insights/](docs/insights/) | read |
| **6-week plan** | 42 days | [ROADMAP.md](ROADMAP.md) | follow it |

## Quick start

Prerequisites: **Node.js 20+** and **Docker Desktop** (running).

```bash
npm install                 # ioredis + TypeScript + tsx
npm run up                  # Redis 8 on localhost:6379 (docker compose up -d)
npm run lab                 # list every lesson, benchmark and project
npm run lesson 1            # your first lesson
npm run lesson 1 -- --step  # same, pausing after each concept (recommended)
```

Then open [ROADMAP.md](ROADMAP.md) and [diagrams/00-learning-roadmap.excalidraw](diagrams/00-learning-roadmap.excalidraw).

## Commands

| Command | What it does |
|---|---|
| `npm run lab` | list everything with one-line summaries |
| `npm run lesson N [-- --step]` | run `lessons/NN_*.ts` (step mode waits for Enter between concepts) |
| `npm run bench N [-- --quick]` | run `benchmarks/bench_NN_*.ts`; results saved to `benchmarks/results/` |
| `npm run project N [-- --quick]` | run `projects/NN_*/demo.ts` |
| `npm run smoke` | run every lesson + project demo that needs only standalone Redis |
| `npm run up [replication\|cluster\|ui]` | start a Docker topology (see below) |
| `npm run down` | stop all lab containers |
| `npm run status` | show lab containers |
| `npm run cli [port]` | open `redis-cli` inside the right container (6379, 6380-6382, 7001-7006) |
| `npm run diagrams` | regenerate `diagrams/*.excalidraw` from code |
| `npm run typecheck` | `tsc --noEmit` over the whole repo |

Any file can also be run directly: `npx tsx lessons/06_sorted_sets.ts`.

## Docker topologies

| Profile | Command | Nodes (host ports) | Used by |
|---|---|---|---|
| default | `npm run up` | `redis-lab` :6379 (config: [config/redis.conf](config/redis.conf)) | lessons 01-14, 17, all benchmarks and projects |
| replication | `npm run up replication` | master :6380, replicas :6381 :6382, sentinels :26379-26381 (`mymaster`, quorum 2) | lesson 15 |
| cluster | `npm run up cluster` | 6 nodes :7001-7006 (3 primaries + 3 replicas), auto-initialised | lesson 16 |
| ui | `npm run up ui` | RedisInsight at http://localhost:5540 (add DB host `redis-lab`, port 6379) | exploring |

> **A real production gotcha, demonstrated here.** Docker Desktop can't route from your host to container IPs.
> Sentinel and Cluster tell clients "the master is `172.28.1.11:6381`", which your laptop can't reach. Every
> node therefore listens on a unique port mapped 1:1, and the client rewrites addresses with ioredis's
> `natMap` ([lib/conn.ts](lib/conn.ts)). It's the same problem `cluster-announce-ip` / `replica-announce-ip`
> solve behind NAT and in Kubernetes.

## Repository map

```
redis-internals/
├── README.md · ROADMAP.md          start here, then follow the 6-week plan
├── run.ts                          the runner behind every npm script
├── lib/                            shared helpers
│   ├── conn.ts                     getClient({trace}), cluster/sentinel clients, cleanup, main()
│   ├── ui.ts                       transcript printer: section / explain / insight / prod / warn
│   └── bench.ts                    measure(), percentiles, report(), save()
├── lessons/01_..17_*.ts            17 runnable lessons (basics -> internals -> distributed -> ops)
├── benchmarks/bench_01..07_*.ts    7 benchmarks + README (methodology, predict-first worksheet)
├── projects/
│   ├── 01_cache_aside/             cache-aside, stampede protection (mutex + XFetch), negative caching
│   ├── 02_rate_limiter/            fixed / sliding log / sliding counter / token bucket / GCRA
│   ├── 03_leaderboard/             sorted sets, tie-breaks, around-me, daily/weekly unions
│   ├── 04_distributed_lock/        safe release, watchdog, fencing tokens, the GC-pause bug
│   ├── 05_job_queue_streams/       consumer groups, retries, XAUTOCLAIM, DLQ, idempotency
│   ├── 06_url_shortener/           base62 ids, click analytics with HLL, node:http server
│   ├── 07_session_store/           sliding expiry, logout-all-devices, device limits
│   ├── 08_realtime_chat/           Pub/Sub for live + Streams for history, presence
│   └── 09_capstone_flash_sale/     build it yourself; check.ts verifies the invariants
├── diagrams/*.excalidraw           18 senior-engineer-style diagrams (+ README index)
├── docs/
│   ├── system-design/              13 deep dives (English)
│   ├── hinglish/                   13 intuition-first explainers (Hinglish) + interview prep
│   └── insights/                   use cases, anti-patterns, runbooks, numbers, checklists
├── tools/                          Excalidraw generator (diagrams as code)
├── config/redis.conf               lab config (deliberately annotated: lab vs production)
├── docker-compose.yml              standalone + replication/sentinel + cluster + RedisInsight
└── notes/                          your learning journal (template inside)
```

## How each piece is meant to be used

**Lessons** print a redis-cli-style transcript of every command they send (`getClient({ trace: true })`),
then measure something real (latency, `MEMORY USAGE`, `OBJECT ENCODING`, INFO deltas), then explain the
internals (**INSIGHT**) and how production systems use it (**IN PRODUCTION**). Each ends with "Your turn"
exercises and an interview angle.

**Benchmarks** print a hypothesis first. Write your prediction in `notes/` *before* you look. The gap
between prediction and result is where the learning is.

**Projects** are small but production-shaped: typed, reusable modules (`cache.ts`, `limiters.ts`, `lock.ts`,
`job-queue.ts`...) plus a `demo.ts` that proves the behaviour under concurrency and failure. Every README
has a key-design table, trade-offs, "Level up" tasks, and a 2-minute interview explanation.

**Diagrams** come with zones, numbered steps, red failure paths, and yellow callouts carrying the
numbers. Redraw them from memory at the end of each week.

**Hinglish notes** are for revision. After a lesson, read its note in [docs/hinglish/lesson-notes/](docs/hinglish/lesson-notes/README.md)
(what to look for in the output, cheat-sheet, interview one-liner). After a project, read its `NOTES.hinglish.md` for a
file-by-file walkthrough of the code.

**Docs** come in two voices: [Hinglish](docs/hinglish/) for intuition ("andar kya hota hai"), and
[system design](docs/system-design/) for rigor, trade-offs, and interview questions.
[Insights](docs/insights/) collects what usually only comes from running Redis in production.

## Lessons at a glance

| # | Lesson | # | Lesson |
|---|---|---|---|
| 01 | Keys, RESP on the wire, SCAN vs KEYS | 10 | MULTI/EXEC, WATCH, Lua, Functions |
| 02 | Strings, atomic counters, race conditions | 11 | Round trips, pipelining, autopipelining |
| 03 | Lists, blocking pops, reliable queues | 12 | Memory, encodings, the hash-bucketing trick |
| 04 | Hashes, listpack → hashtable, field TTLs | 13 | Expiry (lazy + active), eviction policies |
| 05 | Sets, intset, set algebra | 14 | Persistence: RDB, AOF, fork, durability |
| 06 | Sorted sets, skiplists, delayed queues | 15 | Replication, Sentinel, live failover |
| 07 | Bitmaps, HyperLogLog, Geo | 16 | Cluster: slots, hash tags, MOVED/CROSSSLOT |
| 08 | Streams, consumer groups, XAUTOCLAIM | 17 | Observability (SLOWLOG, LATENCY), ACL security |
| 09 | Pub/Sub, keyspace notifications | | |

## Conventions

- Every runnable file wraps its body in `await main(async () => { ... })`, which prints errors nicely and always closes connections.
- Each file only touches keys under its own prefix (`l03:`, `bench:01:`, `proj:05:`) and cleans up with `SCAN` + `UNLINK`. Nothing ever runs `FLUSHALL`.
- Any `CONFIG SET` a lesson makes is restored in a `finally` block.
- The client is [ioredis](https://github.com/redis/ioredis), chosen for *learning*: its API mirrors raw Redis commands (`r.zadd(key, score, member)`), so the code reads like `redis-cli`, and it's what BullMQ is built on. Be aware that ioredis is now in **maintenance mode**, and Redis recommends [node-redis](https://github.com/redis/node-redis) for new projects (it supports newer features like hash-field expiration natively). Concepts transfer 1:1. [docs/insights/redis-vs-alternatives.md](docs/insights/redis-vs-alternatives.md) has an ioredis → node-redis mapping table.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Cannot reach Redis at 127.0.0.1:6379` | Start Docker Desktop, then `npm run up`. Check with `npm run status`. |
| `failed to connect to the docker API` | Docker Desktop isn't running. On Windows: `docker desktop start`. |
| Port 6379 already in use | Another Redis is running locally. Stop it, or set `REDIS_PORT` and change the mapping in `docker-compose.yml`. |
| Lesson 15/16 exits with "Start it with: npm run up replication/cluster" | Start that profile; the first cluster start takes ~10 s to initialise. |
| Lesson 15 after many runs | Sentinel moves the master on every run; that's expected, and the lesson discovers it. |
| Want a clean slate | `npm run down` then `docker volume rm redis-lab_redis-data` |
| Colours look broken | Use Windows Terminal / VS Code terminal, or set `NO_COLOR=1`. |

---

Built as a learning lab. The lab config is intentionally unsafe (no auth, DEBUG enabled) so lessons can
poke at internals. Never copy it to production; [docs/insights/production-checklist.md](docs/insights/production-checklist.md) shows what to use instead.
