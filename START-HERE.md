# START HERE: your clickable learning path

Ye page poore repo ka **map** hai. Upar se neeche chalo, har link pe click karo, har checkbox tick karo.
27 stations, 6 weeks. Har station pe same rhythm hai: **padho → code dekho → run karo → revise → draw → measure → build**.

---

## How to use this page in VS Code

| Kaam | Kaise |
|---|---|
| Is page ko clickable banao | Is file pe `Ctrl+Shift+V` (Markdown Preview). Ab links pe click karo, files khul jayengi. |
| Side-by-side rakho | `Ctrl+K V`: left mein ye guide, right mein jo file khuli hai |
| Terminal kholo | `` Ctrl+` `` and run the commands written below |
| `.excalidraw` diagrams kholo | Install the VS Code extension **Excalidraw** (`pomdtr.excalidraw-editor`), then click the link |
| Checkbox tick karo | Editor mode mein `- [ ]` ko `- [x]` kar do (ya preview mein click karo) |
| Wapas is page pe aao | `Alt+←` (go back) ya `Ctrl+P` → `START-HERE` |

**Legend:** **Padho** = read · **Code** = open the source and skim it · **Run** = run it in the terminal · **Revise** = Hinglish revision note
· **Draw** = diagram · **Deep** = rigorous English doc · **Measure** = benchmark · **Build** = project · **Check** = answer this out loud

> Golden rule: **run karne se pehle predict karo.** "Mujhe lagta hai ye 10x fast hoga". Phir run karo. Jo farak aaye, wahi asli learning hai.
> Apne notes `notes/day-NN.md` mein likho ([template](notes/README.md)).

---

## Progress map

| Week | Stations |
|---|---|
| Setup | [00](#station-00-setup-and-tour) |
| 1: Foundations | [01](#station-01-what-redis-is-and-why-it-is-fast) · [02](#station-02-strings-and-atomic-counters) · [03](#station-03-lists-and-queues) · [04](#station-04-hashes-and-sets) · [05](#station-05-project-url-shortener) |
| 2: Advanced types | [06](#station-06-sorted-sets-and-leaderboard) · [07](#station-07-bitmaps-hyperloglog-geo) · [08](#station-08-streams) · [09](#station-09-pubsub-and-keyspace-events) · [10](#station-10-project-session-store) |
| 3: Patterns | [11](#station-11-transactions-and-lua) · [12](#station-12-round-trips-and-pipelining) · [13](#station-13-caching-and-cache-failures) · [14](#station-14-rate-limiting) · [15](#station-15-distributed-locks) |
| 4: Internals | [16](#station-16-memory-and-encodings) · [17](#station-17-expiry-and-eviction) · [18](#station-18-persistence) · [19](#station-19-blocking-and-raw-throughput) |
| 5: Distributed | [20](#station-20-replication-and-sentinel) · [21](#station-21-cluster-and-sharding) · [22](#station-22-project-job-queue-on-streams) · [23](#station-23-project-realtime-chat) |
| 6: Production | [24](#station-24-observability-and-security) · [25](#station-25-production-thinking) · [26](#station-26-capstone-flash-sale) · [27](#station-27-interview-prep) |

---

## Station 00: Setup and tour
`Day 0 · ~60 min` · Goal: lab chalu karo aur samjho ki repo kaise bana hai.

- [ ] **Padho** → [README.md](README.md) (repo kya hai, commands, Docker topologies)
- [ ] **Padho** → [ROADMAP.md](ROADMAP.md) (6-week plan; ye page usi ka clickable version hai)
- [ ] **Run** → `npm install` then `npm run up` then `npm run lab` (sab lessons ki list dikhegi)
- [ ] **Code** → [docker-compose.yml](docker-compose.yml) (3 topologies: standalone, replication+sentinel, cluster)
- [ ] **Code** → [config/redis.conf](config/redis.conf) (har setting ke upar comment: lab vs production)
- [ ] **Code** → [lib/conn.ts](lib/conn.ts) (`getClient({ trace: true })`, which prints every command like redis-cli)
- [ ] **Code** → [lib/ui.ts](lib/ui.ts) and [run.ts](run.ts) (optional: lessons ka output kaise banta hai)
- [ ] **Draw** → [diagrams/00-learning-roadmap.excalidraw](diagrams/00-learning-roadmap.excalidraw)
- [ ] **Padho** → [docs/hinglish/README.md](docs/hinglish/README.md) (Hinglish track kaise use karna hai)
- [ ] **Check:** `npm run status` mein `redis-lab` "healthy" dikh raha hai?

---

# Week 1: Foundations

## Station 01: What Redis is and why it is fast
`Day 1 · ~90 min` · Goal: single-threaded event loop + RESP protocol samjho.

- [ ] **Padho** → [Hinglish 01: Redis kya hai aur itna fast kyun](docs/hinglish/01-redis-kya-hai-aur-itna-fast-kyun.md)
- [ ] **Code** → [lessons/01_basics_and_resp.ts](lessons/01_basics_and_resp.ts)
- [ ] **Run** → `npm run lesson 1 -- --step`
- [ ] **Revise** → [lesson note 01](docs/hinglish/lesson-notes/01_basics_and_resp.md)
- [ ] **Draw** → [01-event-loop-architecture](diagrams/01-event-loop-architecture.excalidraw) and [02-command-lifecycle](diagrams/02-command-lifecycle.excalidraw)
- [ ] **Deep** → [System design 01: architecture and event loop](docs/system-design/01-architecture-and-event-loop.md)
- [ ] **Check:** Ek thread hokar bhi Redis fast kyun hai? Aur kaunsi commands sabko block kar deti hain?

## Station 02: Strings and atomic counters
`Day 2 · ~60 min` · Goal: race condition dekho aur `INCR` kyun safe hai, samjho.

- [ ] **Padho** → [Hinglish 02: data structures](docs/hinglish/02-data-structures-aasan-bhasha-mein.md) (Strings wala section)
- [ ] **Code** → [lessons/02_strings_and_counters.ts](lessons/02_strings_and_counters.ts)
- [ ] **Run** → `npm run lesson 2 -- --step` (lost updates ka number dhyan se dekhna)
- [ ] **Revise** → [lesson note 02](docs/hinglish/lesson-notes/02_strings_and_counters.md)
- [ ] **Check:** `GET` + `SET` se counter kyun galat hota hai, aur `INCR` se kyun nahi?

## Station 03: Lists and queues
`Day 3 · ~75 min` · Goal: queue banana, aur "reliable" queue ka matlab.

- [ ] **Padho** → [Hinglish 02](docs/hinglish/02-data-structures-aasan-bhasha-mein.md) (Lists wala section)
- [ ] **Code** → [lessons/03_lists_and_queues.ts](lessons/03_lists_and_queues.ts)
- [ ] **Run** → `npm run lesson 3 -- --step`
- [ ] **Revise** → [lesson note 03](docs/hinglish/lesson-notes/03_lists_and_queues.md)
- [ ] **Measure** → [benchmarks/bench_01_roundtrip_vs_pipeline.ts](benchmarks/bench_01_roundtrip_vs_pipeline.ts) → `npm run bench 1` (pehle guess likho: pipeline kitna fast hoga?)
- [ ] **Padho** → [benchmarks/README.md](benchmarks/README.md) (benchmark ka output kaise padhna hai)
- [ ] **Check:** `BRPOP` vs `BLMOVE`: worker crash hone pe kya hota hai?

## Station 04: Hashes and sets
`Day 4 · ~75 min` · Goal: encodings (listpack vs hashtable) aur memory ka connection.

- [ ] **Code** → [lessons/04_hashes.ts](lessons/04_hashes.ts) → **Run** `npm run lesson 4 -- --step`
- [ ] **Revise** → [lesson note 04](docs/hinglish/lesson-notes/04_hashes.md)
- [ ] **Code** → [lessons/05_sets.ts](lessons/05_sets.ts) → **Run** `npm run lesson 5 -- --step`
- [ ] **Revise** → [lesson note 05](docs/hinglish/lesson-notes/05_sets.md)
- [ ] **Draw** → [03-data-structure-encodings](diagrams/03-data-structure-encodings.excalidraw)
- [ ] **Deep** → [System design 02: data structures and encodings](docs/system-design/02-data-structures-and-encodings.md)
- [ ] **Check:** Hash 128 fields ke baad "badal" kyun jaata hai? Isse memory pe kya farak padta hai?

## Station 05: Project: URL shortener
`Day 5 · ~90 min` · Goal: pehla real project. INCR ids, HyperLogLog analytics, HTTP server.

- [ ] **Padho** → [projects/06_url_shortener/README.md](projects/06_url_shortener/README.md)
- [ ] **Code** (is order mein) → [shortener.ts](projects/06_url_shortener/shortener.ts) → [server.ts](projects/06_url_shortener/server.ts) → [demo.ts](projects/06_url_shortener/demo.ts)
- [ ] **Run** → `npm run project 6`
- [ ] **Revise** → [NOTES.hinglish.md](projects/06_url_shortener/NOTES.hinglish.md) (code ka file-by-file walkthrough)
- [ ] **Try** → `cd projects/06_url_shortener` then `npx tsx server.ts`, and in another terminal: `curl -X POST localhost:8088/shorten -d "{\"url\":\"https://redis.io\"}"`
- [ ] **Build:** README ke "Level up" mein se 2 tasks karo
- [ ] **Week 1 review:** Redraw diagrams 01-03 from memory. Write a one-page "kaunsa data type kab" cheat-sheet.

---

# Week 2: Advanced types

## Station 06: Sorted sets and leaderboard
`Day 8 · ~90 min` · Goal: skiplist, rank O(log N), real leaderboard.

- [ ] **Code** → [lessons/06_sorted_sets.ts](lessons/06_sorted_sets.ts) → **Run** `npm run lesson 6 -- --step`
- [ ] **Revise** → [lesson note 06](docs/hinglish/lesson-notes/06_sorted_sets.md)
- [ ] **Build** → [projects/03_leaderboard/README.md](projects/03_leaderboard/README.md) → [leaderboard.ts](projects/03_leaderboard/leaderboard.ts) → [demo.ts](projects/03_leaderboard/demo.ts)
- [ ] **Run** → `npm run project 3`
- [ ] **Revise** → [NOTES.hinglish.md](projects/03_leaderboard/NOTES.hinglish.md)
- [ ] **Check:** Same score pe "pehle aane wala upar" kaise karoge?

## Station 07: Bitmaps, HyperLogLog, Geo
`Day 9 · ~75 min` · Goal: crore users ko KBs mein count karna.

- [ ] **Code** → [lessons/07_bitmaps_hll_geo.ts](lessons/07_bitmaps_hll_geo.ts) → **Run** `npm run lesson 7 -- --step`
- [ ] **Revise** → [lesson note 07](docs/hinglish/lesson-notes/07_bitmaps_hll_geo.md)
- [ ] **Measure** → [bench_03_memory_layouts.ts](benchmarks/bench_03_memory_layouts.ts) → `npm run bench 3`
- [ ] **Check:** 1M unique visitors: SET vs HyperLogLog, memory kitni aur error kitna?

## Station 08: Streams
`Day 10 · ~90 min` · Goal: durable log, consumer groups, ack, crash recovery.

- [ ] **Padho** → [Hinglish 11: Pub/Sub vs Streams](docs/hinglish/11-pubsub-vs-streams.md)
- [ ] **Code** → [lessons/08_streams.ts](lessons/08_streams.ts) → **Run** `npm run lesson 8 -- --step`
- [ ] **Revise** → [lesson note 08](docs/hinglish/lesson-notes/08_streams.md)
- [ ] **Draw** → [13-streams-consumer-groups](diagrams/13-streams-consumer-groups.excalidraw)
- [ ] **Check:** PEL kya hai? Consumer crash ho jaye toh message kaise wapas milta hai?

## Station 09: Pub/Sub and keyspace events
`Day 11 · ~60 min` · Goal: fire-and-forget messaging aur uski limits.

- [ ] **Code** → [lessons/09_pubsub_and_keyspace_events.ts](lessons/09_pubsub_and_keyspace_events.ts) → **Run** `npm run lesson 9 -- --step`
- [ ] **Revise** → [lesson note 09](docs/hinglish/lesson-notes/09_pubsub_and_keyspace_events.md)
- [ ] **Draw** → [14-pubsub-vs-streams](diagrams/14-pubsub-vs-streams.excalidraw)
- [ ] **Deep** → [System design 11: messaging, Pub/Sub and Streams](docs/system-design/11-messaging-pubsub-and-streams.md)
- [ ] **Check:** Subscriber offline tha, message aaya. Ab kya hoga?

## Station 10: Project: session store
`Day 12 · ~90 min` · Goal: sliding expiry, "logout from all devices", device limit.

- [ ] **Padho** → [projects/07_session_store/README.md](projects/07_session_store/README.md)
- [ ] **Code** → [sessions.ts](projects/07_session_store/sessions.ts) → [middleware.ts](projects/07_session_store/middleware.ts) → [demo.ts](projects/07_session_store/demo.ts)
- [ ] **Run** → `npm run project 7`
- [ ] **Revise** → [NOTES.hinglish.md](projects/07_session_store/NOTES.hinglish.md)
- [ ] **Measure** → [bench_02_serialization_formats.ts](benchmarks/bench_02_serialization_formats.ts) → `npm run bench 2` (JSON vs HASH)
- [ ] **Week 2 review:** [Hinglish 13: interview prep](docs/hinglish/13-interview-ki-taiyari.md), the basics + data structures questions

---

# Week 3: Patterns and atomicity

## Station 11: Transactions and Lua
`Day 15 · ~90 min` · Goal: MULTI, WATCH, Lua, Functions. Kab kya use karna hai.

- [ ] **Code** → [lessons/10_transactions_and_lua.ts](lessons/10_transactions_and_lua.ts) → **Run** `npm run lesson 10 -- --step`
- [ ] **Revise** → [lesson note 10](docs/hinglish/lesson-notes/10_transactions_and_lua.md)
- [ ] **Measure** → [bench_05_atomicity_strategies.ts](benchmarks/bench_05_atomicity_strategies.ts) → `npm run bench 5` (naive approach ka oversell dekho!)
- [ ] **Check:** MULTI rollback nahi karta. Toh guarantee kya deta hai?

## Station 12: Round trips and pipelining
`Day 16 · ~60 min` · Goal: network round trip hi asli bottleneck hai.

- [ ] **Code** → [lessons/11_pipelining_and_round_trips.ts](lessons/11_pipelining_and_round_trips.ts) → **Run** `npm run lesson 11 -- --step`
- [ ] **Revise** → [lesson note 11](docs/hinglish/lesson-notes/11_pipelining_and_round_trips.md)
- [ ] **Measure** → [bench_07_client_concurrency.ts](benchmarks/bench_07_client_concurrency.ts) → `npm run bench 7`
- [ ] **Check:** Pipeline vs MULTI vs autopipelining: teeno mein farak?

## Station 13: Caching and cache failures
`Day 17 · ~2 hours` · Goal: cache-aside, stampede, penetration, avalanche.

- [ ] **Padho** → [Hinglish 03: caching patterns](docs/hinglish/03-caching-patterns.md) → [Hinglish 04: cache ki bimariyan](docs/hinglish/04-cache-ki-bimariyan.md)
- [ ] **Draw** → [04-cache-aside-flow](diagrams/04-cache-aside-flow.excalidraw) → [05-write-strategies](diagrams/05-write-strategies.excalidraw) → [06-cache-stampede-protection](diagrams/06-cache-stampede-protection.excalidraw)
- [ ] **Deep** → [SD 03: caching patterns](docs/system-design/03-caching-patterns.md) → [SD 04: cache failure modes](docs/system-design/04-cache-failure-modes.md)
- [ ] **Build** → [projects/01_cache_aside/README.md](projects/01_cache_aside/README.md) → [slow-db.ts](projects/01_cache_aside/slow-db.ts) → [cache.ts](projects/01_cache_aside/cache.ts) → [demo.ts](projects/01_cache_aside/demo.ts)
- [ ] **Run** → `npm run project 1` (stampede: 50 DB queries vs 1 with the lock)
- [ ] **Revise** → [NOTES.hinglish.md](projects/01_cache_aside/NOTES.hinglish.md)
- [ ] **Check:** Write pe cache ko update kyun nahi, DELETE kyun karte hain?

## Station 14: Rate limiting
`Day 18 · ~90 min` · Goal: 5 algorithms aur unke trade-offs.

- [ ] **Padho** → [Hinglish 09: rate limiting](docs/hinglish/09-rate-limiting.md)
- [ ] **Draw** → [11-rate-limiter-algorithms](diagrams/11-rate-limiter-algorithms.excalidraw)
- [ ] **Deep** → [SD 09: rate limiting](docs/system-design/09-rate-limiting.md)
- [ ] **Build** → [projects/02_rate_limiter/README.md](projects/02_rate_limiter/README.md) → [limiters.ts](projects/02_rate_limiter/limiters.ts) → [middleware.ts](projects/02_rate_limiter/middleware.ts) → [demo.ts](projects/02_rate_limiter/demo.ts)
- [ ] **Run** → `npm run project 2`
- [ ] **Revise** → [NOTES.hinglish.md](projects/02_rate_limiter/NOTES.hinglish.md)
- [ ] **Check:** Fixed window mein boundary pe 2x traffic kaise ghus jaata hai?

## Station 15: Distributed locks
`Day 19 · ~90 min` · Goal: safe release, fencing token, GC-pause wala bug.

- [ ] **Padho** → [Hinglish 10: distributed locks](docs/hinglish/10-distributed-locks.md)
- [ ] **Draw** → [12-distributed-lock-fencing](diagrams/12-distributed-lock-fencing.excalidraw)
- [ ] **Deep** → [SD 10: distributed locks](docs/system-design/10-distributed-locks.md)
- [ ] **Build** → [projects/04_distributed_lock/README.md](projects/04_distributed_lock/README.md) → [lock.ts](projects/04_distributed_lock/lock.ts) → [fenced-store.ts](projects/04_distributed_lock/fenced-store.ts) → [demo.ts](projects/04_distributed_lock/demo.ts)
- [ ] **Run** → `npm run project 4`
- [ ] **Revise** → [NOTES.hinglish.md](projects/04_distributed_lock/NOTES.hinglish.md)
- [ ] **Week 3 review:** RFC likho, "Product catalog ke liye Redis cache". Template: [senior-engineer-mindset.md](docs/insights/senior-engineer-mindset.md)

---

# Week 4: Internals

## Station 16: Memory and encodings
`Day 22 · ~75 min` · Goal: har key ka "tax" aur hash-bucketing trick.

- [ ] **Code** → [lessons/12_memory_and_encodings.ts](lessons/12_memory_and_encodings.ts) → **Run** `npm run lesson 12 -- --step`
- [ ] **Revise** → [lesson note 12](docs/hinglish/lesson-notes/12_memory_and_encodings.md)
- [ ] **Measure** → `npm run bench 3` again. Ab numbers ka matlab samajh aayega.
- [ ] **Check:** 10 lakh chhoti keys vs hash buckets: memory kitni bachi?

## Station 17: Expiry and eviction
`Day 23 · ~75 min` · Goal: keys kaise marti hain (TTL) aur kaise nikali jaati hain (memory full).

- [ ] **Padho** → [Hinglish 05: expiry aur eviction](docs/hinglish/05-expiry-aur-eviction.md)
- [ ] **Code** → [lessons/13_expiry_and_eviction.ts](lessons/13_expiry_and_eviction.ts) → **Run** `npm run lesson 13 -- --step`
- [ ] **Revise** → [lesson note 13](docs/hinglish/lesson-notes/13_expiry_and_eviction.md)
- [ ] **Draw** → [07-expiry-and-eviction](diagrams/07-expiry-and-eviction.excalidraw)
- [ ] **Deep** → [SD 05: expiry and eviction](docs/system-design/05-expiry-and-eviction.md)
- [ ] **Check:** Cache, queue aur session store ke liye kaunsi `maxmemory-policy`?

## Station 18: Persistence
`Day 24 · ~90 min` · Goal: RDB vs AOF, fork, copy-on-write, kitna data loss ho sakta hai.

- [ ] **Padho** → [Hinglish 06: persistence](docs/hinglish/06-persistence-rdb-aof.md)
- [ ] **Code** → [lessons/14_persistence.ts](lessons/14_persistence.ts) → **Run** `npm run lesson 14 -- --step`
- [ ] **Revise** → [lesson note 14](docs/hinglish/lesson-notes/14_persistence.md)
- [ ] **Draw** → [08-persistence-rdb-aof](diagrams/08-persistence-rdb-aof.excalidraw)
- [ ] **Deep** → [SD 06: persistence](docs/system-design/06-persistence-rdb-aof.md)
- [ ] **Try:** lesson jo "durability experiment" print karta hai (`docker kill` → `docker start`), woh khud karo
- [ ] **Check:** `appendfsync everysec` mein crash pe kitna data ja sakta hai?

## Station 19: Blocking and raw throughput
`Day 25-26 · ~2 hours` · Goal: ek slow command sabko kaise rok deta hai, aur zaroori numbers.

- [ ] **Measure** → [bench_04_big_keys_blocking.ts](benchmarks/bench_04_big_keys_blocking.ts) → `npm run bench 4` (DEL vs UNLINK)
- [ ] **Measure** → [bench_06_redis_benchmark_cli.ts](benchmarks/bench_06_redis_benchmark_cli.ts) → `npm run bench 6` (official C tool)
- [ ] **Padho** → [insights: numbers to know](docs/insights/numbers-to-know.md)
- [ ] **Padho** → [insights: anti-patterns](docs/insights/anti-patterns.md)
- [ ] **Week 4 review:** Redraw diagrams 07 and 08. RDB vs AOF 2 minute mein samjhao.

---

# Week 5: Distributed Redis

## Station 20: Replication and Sentinel
`Day 29-30 · ~2 hours` · Goal: async replication, failover, split brain.

- [ ] **Run** → `npm run up replication` (master + 2 replicas + 3 sentinels)
- [ ] **Padho** → [Hinglish 07: replication aur sentinel](docs/hinglish/07-replication-aur-sentinel.md)
- [ ] **Code** → [lessons/15_replication_and_sentinel.ts](lessons/15_replication_and_sentinel.ts) → **Run** `npm run lesson 15 -- --step` (live failover hoga!)
- [ ] **Revise** → [lesson note 15](docs/hinglish/lesson-notes/15_replication_and_sentinel.md)
- [ ] **Draw** → [09-replication-sentinel-failover](diagrams/09-replication-sentinel-failover.excalidraw)
- [ ] **Deep** → [SD 07: replication and sentinel](docs/system-design/07-replication-and-sentinel.md)
- [ ] **Drill:** `docker stop repl-master`, watch `docker logs -f sentinel-1`, then `docker start repl-master`. Ab woh replica ban gaya!
- [ ] **Check:** Master ne "OK" bola, phir bhi write lose ho sakti hai. Kab?

## Station 21: Cluster and sharding
`Day 31 · ~90 min` · Goal: 16384 slots, hash tags, MOVED vs ASK.

- [ ] **Run** → `npm run up cluster`
- [ ] **Padho** → [Hinglish 08: cluster aur sharding](docs/hinglish/08-cluster-aur-sharding.md)
- [ ] **Code** → [lessons/16_cluster.ts](lessons/16_cluster.ts) → **Run** `npm run lesson 16 -- --step`
- [ ] **Revise** → [lesson note 16](docs/hinglish/lesson-notes/16_cluster.md)
- [ ] **Draw** → [10-cluster-hash-slots](diagrams/10-cluster-hash-slots.excalidraw)
- [ ] **Deep** → [SD 08: cluster and sharding](docs/system-design/08-cluster-and-sharding.md)
- [ ] **Try** → `npm run cli 7001` and `CLUSTER KEYSLOT {user:42}:cart` (khud slots dekho)
- [ ] **Check:** `{user:42}` hash tag kyun chahiye, aur `{global}` kyun khatarnak hai?

## Station 22: Project: job queue on Streams
`Day 32 · ~2 hours` · Goal: production-grade queue: retries, DLQ, crash recovery.

- [ ] **Padho** → [projects/05_job_queue_streams/README.md](projects/05_job_queue_streams/README.md)
- [ ] **Code** → [job-queue.ts](projects/05_job_queue_streams/job-queue.ts) → [tasks.ts](projects/05_job_queue_streams/tasks.ts) → [producer.ts](projects/05_job_queue_streams/producer.ts) → [worker.ts](projects/05_job_queue_streams/worker.ts) → [demo.ts](projects/05_job_queue_streams/demo.ts)
- [ ] **Run** → `npm run project 5` (accounting exact balance hona chahiye)
- [ ] **Try:** 2 terminals mein `cd projects/05_job_queue_streams` then `npx tsx worker.ts --name w1` / `--name w2`, and a third: `npx tsx producer.ts`
- [ ] **Revise** → [NOTES.hinglish.md](projects/05_job_queue_streams/NOTES.hinglish.md)

## Station 23: Project: realtime chat
`Day 33 · ~90 min` · Goal: Pub/Sub (live) + Streams (history), dono saath mein.

- [ ] **Padho** → [projects/08_realtime_chat/README.md](projects/08_realtime_chat/README.md)
- [ ] **Code** → [chat.ts](projects/08_realtime_chat/chat.ts) → [demo.ts](projects/08_realtime_chat/demo.ts) → [cli.ts](projects/08_realtime_chat/cli.ts)
- [ ] **Run** → `npm run project 8`
- [ ] **Try:** 2 terminals mein `cd projects/08_realtime_chat` then `npx tsx cli.ts --room general --user alice` (and `--user bob`)
- [ ] **Revise** → [NOTES.hinglish.md](projects/08_realtime_chat/NOTES.hinglish.md)
- [ ] **Week 5 review:** Notification system design karo (Streams + Pub/Sub + rate limit), 1 page.

---

# Week 6: Production and mastery

## Station 24: Observability and security
`Day 36 · ~90 min` · Goal: "Redis slow hai" ko debug karna, ACL se lock down karna.

- [ ] **Code** → [lessons/17_observability_and_security.ts](lessons/17_observability_and_security.ts) → **Run** `npm run lesson 17 -- --step`
- [ ] **Revise** → [lesson note 17](docs/hinglish/lesson-notes/17_observability_and_security.md)
- [ ] **Draw** → [16-latency-debugging-runbook](diagrams/16-latency-debugging-runbook.excalidraw)
- [ ] **Padho** → [insights: debugging runbook](docs/insights/debugging-runbook.md)
- [ ] **Check:** Latency alert aaya. Pehli 5 commands kaunsi chalaoge?

## Station 25: Production thinking
`Day 37 · ~2 hours` · Goal: senior engineer ki tarah sochna.

- [ ] **Padho** → [Hinglish 12: production mein Redis](docs/hinglish/12-production-mein-redis.md)
- [ ] **Draw** → [15-production-architecture](diagrams/15-production-architecture.excalidraw)
- [ ] **Deep** → [SD 12: production operations](docs/system-design/12-production-operations.md)
- [ ] **Padho** → [insights: production checklist](docs/insights/production-checklist.md)
- [ ] **Padho** → [insights: senior engineer mindset](docs/insights/senior-engineer-mindset.md)
- [ ] **Padho** → [insights: industry use cases](docs/insights/industry-use-cases.md)
- [ ] **Padho** → [insights: Redis vs alternatives](docs/insights/redis-vs-alternatives.md) (Valkey, Dragonfly, Kafka, ioredis vs node-redis)

## Station 26: Capstone: flash sale
`Day 38-40 · 3 days` · Goal: **khud banao.** 10,000 units, 1M users, no oversell.

- [ ] **Padho** → [projects/09_capstone_flash_sale/README.md](projects/09_capstone_flash_sale/README.md) (requirements + milestones)
- [ ] **Draw** → [17-flash-sale-capstone](diagrams/17-flash-sale-capstone.excalidraw)
- [ ] **Padho** → [NOTES.hinglish.md](projects/09_capstone_flash_sale/NOTES.hinglish.md) (sirf hints)
- [ ] **Code (yahan likhna hai)** → [flash-sale.ts](projects/09_capstone_flash_sale/flash-sale.ts)
- [ ] **Check** → [check.ts](projects/09_capstone_flash_sale/check.ts): `cd projects/09_capstone_flash_sale` then `npx tsx check.ts` (25 invariants, sab PASS hone chahiye)
- [ ] **Atak gaye?** → [solution-hints.md](projects/09_capstone_flash_sale/solution-hints.md) (ek-ek hint kholo, poora nahi)

## Station 27: Interview prep
`Day 41-42` · Goal: kisi bhi Redis design ko defend kar sako.

- [ ] **Deep** → [SD 13: design case studies](docs/system-design/13-design-case-studies.md) (6 mini system designs)
- [ ] **Padho** → [Hinglish 13: interview ki taiyari](docs/hinglish/13-interview-ki-taiyari.md) (50 questions + design script)
- [ ] **Mock:** 3 designs 10-10 minute mein bolke samjhao (leaderboard, rate limiter, flash sale)
- [ ] **Final:** `notes/` mein "Redis in production: my one-pager" likho, aur week 1 ke notes se compare karo

---

## Quick reference

| Kya chahiye | Kahan |
|---|---|
| Saare Hinglish concept docs | [docs/hinglish/README.md](docs/hinglish/README.md) |
| Saare lesson revision notes | [docs/hinglish/lesson-notes/README.md](docs/hinglish/lesson-notes/README.md) |
| Saare system-design docs | [docs/system-design/README.md](docs/system-design/README.md) |
| Saare insights | [docs/insights/README.md](docs/insights/README.md) |
| Saare diagrams (index) | [diagrams/README.md](diagrams/README.md) |
| Benchmarks kaise padhein | [benchmarks/README.md](benchmarks/README.md) |
| Day-by-day plan + checkpoints | [ROADMAP.md](ROADMAP.md) |
| Kuch toot gaya? | [README.md#troubleshooting](README.md#troubleshooting) |

**Commands yaad rakho:** `npm run lab` (list) · `npm run lesson N -- --step` · `npm run bench N` · `npm run project N` · `npm run cli` · `npm run up [replication|cluster]` · `npm run down`
