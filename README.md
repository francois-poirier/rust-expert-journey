# 🦀 Rust Expert Journey — Adjusted Plan  (12 months · 20 h/week)

**A sustainable transition from Senior Java Engineer to Rust Backend Engineer in 52 weeks: 4 hours a day, Monday to Friday — 1,040 total hours — driven by one clearly bounded project: a distributed task queue.**

[![Rust](https://img.shields.io/badge/language-Rust-orange?logo=rust)](https://www.rust-lang.org/)
[![Duration](https://img.shields.io/badge/duration-12%20months-blue)](#4-programme-at-a-glance)
[![Effort](https://img.shields.io/badge/effort-20%20hours%2Fweek-informational)](#5-study-method-4-hday)
[![Focus](https://img.shields.io/badge/focus-70%25%20backend%20%2F%2030%25%20systems%20programming-success)](#3-learning-allocation-7030)

## Table of contents

1. [Programme at a glance](#1-programme-at-a-glance)
2. [The driving project: jobline](#2-the-driving-project-jobline)
3. [Study method (4 h/day)](#3-study-method-4-hday)
4. [jobline design](#4-jobline-design)
5. [Project structure](#5-project-structure)
6. [Implementation: the six phases](#6-implementation-the-six-phases)
7. [Testing strategy and risks](#7-testing-strategy-and-risks)
8. [Weekly review and progress](#8-weekly-review-and-progress)
9. [Glossary](#9-glossary)

---

## 1. Programme at a glance

### 1.1 Why this plan was adjusted

The original plan (52 weeks × 30 h = 1,560 h) is too intense to sustain alongside a full-time job. This revision keeps the **same 12-month duration** and lowers the commitment to **20 h/week (4 h/day, Monday to Friday)**, giving **1,040 total hours (−33 %)**. Two changes make the programme realistic at this capacity:

1. **New driving project.** The Distributed Event Processing Platform (a Kafka-like log with custom consensus) was too ambitious for this budget. The replacement keeps genuine distributed-systems content but is bounded: **`jobline`, a distributed task queue** (section 2), selected from three candidates in section 2.3.
2. **Reduced scope per milestone.** Every milestone keeps its original weeks and ships a working capability, with explicit cuts; advanced topics move to optional stretch goals.

| Parameter | Original plan | Adjusted plan   |
|---|---|---|
| Daily effort (Mon–Fri) | 6 h | **4 h** |
| Weekly effort | 30 h | **20 h** |
| Duration | 52 weeks (12 months) | **52 weeks (unchanged)** |
| Total effort | 1,560 h | **1,040 h (−520 h, −33 %)** |
| Driving project | Event Processing Platform | **Distributed task queue (`jobline`)** |
| Phases / milestones | 6 / 25 | 6 / 25 (same week boundaries) |
| Hours per milestone | 60–90 h | **40–60 h** |
| Acceptance criteria | Full | Adjusted per milestone (section 6) |

**Adjusted career objective:** Rust Backend Engineer focused on data-intensive services. With 1,040 h the realistic outcome is demonstrable competence — production-grade Rust, async networking, durable storage, distributed delivery semantics and operations discipline — evidenced by one coherent, deployed project. Multi-coordinator consensus, sharding and similar advanced topics are documented as future work rather than claimed.

### 1.2 Learning allocation: 70/30

| Track | Share | Hours | Primary outcome |
|---|---:|---:|---|
| Distributed systems & high-performance backend | 70 % | 728 h | Design, build, test and operate `jobline` |
| Systems programming & low-level Rust | 30 % | 312 h | Memory, concurrency, file I/O, Linux, performance |
| **Total** | **100 %** | **1,040 h** | 12-month programme |

### 1.3 The programme in numbers

- **Duration:** 52 weeks (12 months).
- **Weekly effort:** 20 hours. **Daily effort:** 4 hours, Monday to Friday.
- **Total effort:** 1,040 hours.
- **Strategy:** one product, one repository, progressive milestones.

| Phase | Weeks | Hours | Milestones | Main focus |
|---|---:|---:|---:|---|
| 1. Rust Core & Ownership | 1–8 | 160 | 4 | Rust fundamentals; in-memory queue store |
| 2. Advanced Rust & Storage | 9–16 | 160 | 4 | Concurrency, WAL, snapshots, recovery |
| 3. Async Rust & Networking | 17–24 | 160 | 4 | Tokio, HTTP API, client, load testing |
| 4. Reliable Delivery & Fault Tolerance | 25–36 | 240 | 5 | Leases, retries, DLQ, failure drills |
| 5. Performance & Systems Programming | 37–44 | 160 | 4 | Profiling and one measured optimisation cycle |
| 6. Production Readiness | 45–52 | 160 | 4 | Observability, Docker, CI/CD, release |
| **Total** | **1–52** | **1,040** | **25** | **One product in evolution** |

---

## 2. The driving project: jobline

### 2.1 What it is

`jobline` is a distributed task queue, in the spirit of Amazon SQS or Sidekiq, built from scratch in Rust. Producers enqueue jobs over HTTP/JSON; a pool of independent worker processes polls and **leases** jobs, executes them, and acknowledges completion. A lease carries a visibility timeout: a worker that crashes mid-job simply loses the lease, and the coordinator makes the job visible again. Repeated failures move a job to a dead-letter queue (DLQ). All coordinator state survives restarts through a WAL plus periodic snapshots, recovered by replay.

```
+-----------+  HTTP/JSON   +-------------------------------+
| Producers |------------->|  Coordinator (Axum + Tokio)   |
|  (apps)   |<-------------|  Queues -- Leases -- DLQ      |
+-----------+              |  WAL -- Snapshot              |
                           +-------+---------------+-------+
                                   | poll / lease   | poll / lease
                                   v                v
                            +-------------+   +-------------+
                            |  Worker 1   |   |  Worker 2   |
                            +-------------+   +-------------+
```

The coordinator is a single, restartable process: its state lives in the WAL, so a crash is recovered by replay, and the resulting single point of failure is documented honestly instead of being solved with consensus. That one decision removes the most expensive research risk of the original project while keeping all the failure-handling work intact.

Delivery semantics: **at-least-once**. A job is delivered one or more times — never zero; the worker must be idempotent (see the glossary, section 9). The coordinator is single-node by design (multi-coordinator consensus is out of scope, stretch goal).

### 2.2 Why this project

- **Genuinely distributed systems.** Delivery semantics (at-least-once), leases and visibility timeouts, idempotency, retries with backoff, dead-letter handling, failure detection and backpressure form the core vocabulary of production distributed systems.
- **Engineering-grade, not research-grade.** The lease model is documented in real systems (SQS, Sidekiq), so there is no open-ended consensus research and every failure mode is known in advance.
- **Every milestone ships.** Each phase adds a visible capability to the same product; there are no throwaway exercises, and every week ends with a compiling repository and green tests.
- **Easy to explain.** The design maps one-to-one to SQS, Sidekiq or Celery, which makes interview conversations straightforward.

### 2.3 Project options compared

Three candidate driving projects were evaluated for the 1,040-hour budget:

| Criterion | A. Event Platform (original) | B. Task Queue — `jobline` | C. Key-Value Store — `kvline` |
|---|---|---|---|
| Distributed-systems core | Replication, ordering, custom consensus | Leases, at-least-once, retries, DLQ, failure detection | Primary/replica log shipping |
| Research risk | High — consensus design | Low — documented lease model | Low |
| Fit in 1,040 h | Exceeds budget | Fits, with margin | Fits, with margin |
| Interview defensibility | Hard to defend at depth | Strong — comparable to SQS/Sidekiq | Moderate |
| Verdict | Discarded (v1) | **Selected (v3)** | Discarded (v2) — not distributed enough |

The task queue keeps the distributed-systems substance while removing the two most expensive elements of the original project: custom consensus and cross-node log replication. The key-value store (v2) remains a valid fallback if queue semantics prove heavier than expected.

### 2.4 Scope boundaries

| In scope (built by week 52) | Out of scope → stretch goals |
|---|---|
| Enqueue / poll / ack / complete over HTTP/JSON | Multi-coordinator HA and consensus |
| Leases with visibility timeout and heartbeats | Priority queues and fair scheduling |
| At-least-once delivery with idempotency keys | Pub/sub fan-out and streaming |
| Retries with exponential backoff; dead-letter queue | TLS and authentication |
| WAL with CRC32, snapshots and crash recovery | Kubernetes deployment (compose is the target) |
| Worker-pool scaling; load tests to 500 connections | 1,000+ connections, io_uring / SIMD |

---

## 3. Study method (4 h/day)

Daily template, Monday to Friday:

| Activity | Hours/day | Hours/week |
|---|---:|---:|
| Reading & documentation | 0.5 | 2.5 |
| Exercises & code reading | 0.5 | 2.5 |
| Project implementation | 2.0 | 10.0 |
| Testing, debugging, profiling | 0.75 | 3.75 |
| Learning log & planning | 0.25 | 1.25 |
| **Total** | **4.0** | **20.0** |

Session structure:

1. **Block 1 (30 min) — Theory:** read about the active milestone topic (book or official documentation).
2. **Block 2 (30 min) — Exercises:** Rustlings, examples, or related code reading.
3. **Block 3 (2 h) — Project:** the main work block on the active `jobline` milestone.
4. **Block 4 (45 min) — Verification:** tests, debugging, benchmarks.
5. **Block 5 (15 min) — Log:** learning log and one concrete next step for tomorrow.

Pace rules:

- If a day shrinks to 2 h, protect the project block and cut theory — never cut testing at the end of a milestone.
- Close every week with the repository compiling and tests green.
- Do not move to the next milestone without meeting its adjusted acceptance criteria (section 6).
- At 4 h/day the margin is small: if a week is lost, recover only the essential items of the milestone, not the exact hours.

---

## 4. jobline design

This section is the technical reference for the project. Read it before writing code for each phase: it defines the architecture, the data model, the lease and WAL semantics, and the HTTP API that every milestone implements.

### 4.1 Guiding principles

1. **The WAL is the source of truth.** Every state mutation follows: append to WAL → apply to in-memory state → respond. If it is not in the WAL, it does not exist.
2. **Derived state is not logged.** Lease expirations, backoff and visibility are *computed* (timers + reads with `now()`), not written. This keeps the WAL to business events only.
3. **At-least-once, never at-most-once.** When in doubt, re-deliver. Idempotency is the worker's responsibility; the producer helps with idempotency keys.
4. **One coordinator, done well.** Single-node by design. The entire budget goes to lease semantics, durability and performance, not consensus.
5. **Every milestone compiles and passes tests.** The repo is never left broken between phases.

### 4.2 Components and architecture (C4 level 2)

```
                   ┌─────────────────────────────────────────────┐
   HTTP/JSON       │              jobline-coordinator            │
  ┌───────────┐    │  ┌─────────┐   ┌─────────────────────────┐  │
  │ Producer  │──▶│  │ HTTP    │   │      Core Engine        │  │
  │ (any)     │    │  │ Router  │──▶│  ┌──────────────┐      │  │
  └───────────┘    │  └────┬────┘    │  │ State machine│      │  │
  ┌───────────┐    │       │         │  │ (queues,     │      │  │
  │ Worker 1  │──▶│       │         │  │  leases,DLQ) │      │  │
  │ (client   │    │       ▼         │  └──────┬───────┘      │  │
  │  crate)   │    │  ┌─────────┐    │         │              │  │
  └───────────┘    │  │ proto   │    │  ┌──────▼───────┐      │  │
  ┌───────────┐    │  │ (types) │    │  │ Store (WAL,  │      │  │
  │ Worker N  │──▶│  └─────────┘    │  │  snapshot)   │──────┼──┼──▶ disk
  └───────────┘    │                 │  └──────────────┘      │  │
                   │                 │  ┌──────────────┐      │  │
                   │                 │  │ Lease mgr    │      │  │
                   │                 │  │ (timers)     │      │  │
                   │                 │  └──────────────┘      │  │
                   │                 └────────────────────────┘  │
                   └─────────────────────────────────────────────┘
```

- **Coordinator**: single process. Exposes HTTP/JSON. Holds all state in memory (Rust data structures) backed by the WAL.
- **Workers**: independent processes (not threads of the coordinator). Use the `jobline-client` crate. Poll → receive lease → execute → ack/heartbeat.
- **Producer**: any HTTP client; a client crate is available.

### 4.3 Data model

```rust
// jobline-proto
pub struct Job {
    pub id: JobId,                    // ULID (sortable, useful for snapshots)
    pub queue: QueueName,             // validated: [a-z0-9-]{1,64}
    pub payload: Bytes,               // opaque to the coordinator
    pub idempotency_key: Option<String>, // producer-side dedup
    pub attempts: u32,                // completed deliveries
    pub max_attempts: u32,            // default 5, configurable per queue
    pub enqueued_at: InstantSys,      // serializable SystemTime
    pub visible_at: InstantSys,       // computed visibility (backoff)
    pub lease: Option<Lease>,
}

pub struct Lease {
    pub id: LeaseId,                  // UUID v4, one per delivery
    pub worker: WorkerId,             // optional worker identifier
    pub deadline: InstantSys,         // now + visibility_timeout
    pub last_heartbeat: InstantSys,
}

pub enum JobState { Available, Leased, AwaitingRetry { at: InstantSys }, Dead }
```

In-memory structures of the coordinator:

```rust
// jobline-core
pub struct Queue {
    // queue of available jobs ordered by visible_at
    available: BinaryHeap<Reverse<(InstantSys, JobId)>>,
    // jobs currently leased
    leased: HashMap<LeaseId, JobId>,
    // id → Job index (single source of the full job)
    jobs: HashMap<JobId, Job>,
    dlq: VecDeque<JobId>,             // pointers; the Job stays in `jobs`
    dedup: HashMap<String, JobId>,     // idempotency_key → job
}

pub struct Engine {
    queues: HashMap<QueueName, Queue>,
    config: QueueConfig,              // visibility_timeout, backoff, max_attempts
    wal: WalWriter,
}
```

Decisions:

- **`BinaryHeap` ordered by `visible_at`** gives O(log n) enqueue and dequeue, and unifies available + awaiting-retry into a single structure.
- **Jobs and DLQ**: the DLQ stores `JobId`s; the full `Job` lives in `jobs` until purged. Moving to DLQ is therefore O(1) in the log (one event, not a copy of the payload).
- **Dedup by idempotency key**: `HashMap` in memory, *not* persisted (dedup is best-effort within process lifetime + whatever the snapshot captures; documented as best-effort cross-restart via snapshot).

### 4.4 Lease and visibility semantics

```
        enqueue               poll (lease T=30s)         ack
Available ───────▶ Available ───────────────▶ Leased ─────▶ Deleted
                      ▲                            │
                      │ lease expires (crash)      │ nack / failure
                      │ or heartbeat lost          ▼
                      └────────────── AwaitingRetry(at=now+backoff(attempts))
                                                         │
                                              attempts > max_attempts
                                                         ▼
                                                       Dead (DLQ)
```

Rules:

- **Visibility timeout** per queue (default 30 s), renewable via **heartbeat** (`POST /leases/{id}/heartbeat`, extends deadline).
- **Expiry**: lazy check on poll + periodic sweep by the lease manager (every ~1 s). If `now > deadline` and no ack: transition `Leased → AwaitingRetry` (if attempts remain) or `→ Dead`.
- **Explicit nack** with `requeue: bool` lets the worker return the job without waiting for the timeout (useful for deterministic errors).
- **Re-delivery**: a job whose lease expires returns to `Available` with `visible_at = now + backoff(attempts)`; exponential backoff with jitter, e.g. `min(cap, base * 2^attempts) * (1 ± 0.2)`.
- **Heartbeat**: the worker must call it if the job outlives the timeout. The coordinator extends the `deadline`; if the worker process is dead, heartbeats cease and the lease expires anyway.

### 4.5 WAL and recovery

#### 4.5.1 Log records

Business events only (no heartbeats or expirations):

```rust
pub enum LogRecord {
    JobEnqueued   { job: Box<Job> },              // full payload, once
    LeaseGranted  { job: JobId, lease: LeaseId },  // optional; see 4.5.3
    JobAcked      { job: JobId },
    JobReleased   { job: JobId, requeue: bool, attempts: u32 },
    JobRetried    { job: JobId, visible_at: InstantSys },
    JobDeadLettered { job: JobId, reason: String },
    JobPurged     { job: JobId },                  // admin delete from DLQ
}
```

Binary format of each record (not JSON — the WAL is internal):

```
[u32 LE: length][u32 LE: CRC32 of payload][u8: record_type][payload (bincode/postcard)]
```

- CRC32 (IEEE) over the payload protects against torn writes and bit rot.
- `length` allows *scan forward* after a mid-write cut: during replay, a record with invalid CRC or impossible length marks the logical end of the segment; the rest is discarded (tail truncation). **The process that was writing died mid-record: that is safe.**

#### 4.5.2 Fsync and batching

- Append → `writev` → `fdatasync` before responding to the client (real durability).
- To avoid destroying throughput: a write queue with **group commit** (appends are batched every ~1 ms or N records, whichever comes first) — this is what allows thousands of enqueues/s with fsync.
- Segments rotated by size (e.g. 256 MB): `wal/000001.log`, `000002.log`...

#### 4.5.3 Is `LeaseGranted` logged?

Decision: **no, by default** ("ephemeral leases" mode). On recovery, any `Leased` job is treated as `AwaitingRetry`/`Available` — conservative and correct. Future option: log `LeaseGranted` to avoid re-deliveries after a coordinator crash, at the cost of +1 fsync per poll. Document as a trade-off; implement the flag `durability_mode: Relaxed|Strict` but start in `Relaxed`.

#### 4.5.4 Snapshots

- Triggers: every 5 minutes **and** every 100k records since the last snapshot, whichever comes first; also snapshot on `SIGTERM` (clean shutdown).
- Format: bincode dump of the essential state (`queues.jobs`, `dedup`, counters). Header with `snapshot_seq` and `wal_offset` (how far it covers).
- On startup: read latest snapshot → replay WAL from `wal_offset` → ready.
- Compression optional (zstd) via feature flag; start without.
- Retention: keep the last 2 snapshots + WAL newer than the older of them; GC the rest.

#### 4.5.5 Recovery after kill -9

1. Open latest valid snapshot (validate CRC of the snapshot; if it fails, fall back to the previous one).
2. Replay WAL segments after it in order, applying valid records up to the first corrupt/truncated record.
3. Re-evaluate all `Leased`: `visible_at = now`, `attempts` unchanged (the worker did not fail — the coordinator did).
4. Serve traffic. The corrupt cut segment is renamed to `.corrupt` for inspection.

### 4.6 HTTP/JSON API (v1)

Base: `http://coordinator:7500/v1`

| Method | Route | Description |
|---|---|---|
| POST | `/queues/{q}/jobs` | Enqueue. Body: `{payload, idempotency_key?, delay_ms?}`. → `202 {job_id}` (or `200` with the existing job on dedup hit) |
| POST | `/queues/{q}/poll` | Body: `{worker_id?, wait_ms? (long-poll, max 20s), max_jobs?}`. → `200 {jobs:[{job_id, lease_id, payload, lease_deadline}]}` or `204` if empty |
| POST | `/leases/{lease_id}/ack` | → `204`. Idempotent: duplicate ack = 204. |
| POST | `/leases/{lease_id}/nack` | Body: `{requeue: true, reason?}`. Returns the job to the pool. |
| POST | `/leases/{lease_id}/heartbeat` | → `204`, extends deadline. 404/409 if the lease expired. |
| GET | `/queues/{q}/stats` | → depths, in-flight, dlq size, oldest job age |
| GET | `/queues/{q}/dlq` (admin) | List DLQ |
| POST | `/queues/{q}/dlq/{job_id}/requeue` (admin) | Resurrect a job from the DLQ |

Errors: JSON `{error: code, message}` with stable codes: `queue_not_found`, `job_not_found`, `lease_expired`, `payload_too_large` (cap, e.g. 1 MiB), `rate_limited`.

Long-poll: if the queue is empty, the coordinator holds the request until `wait_ms` or until a job arrives (notification via `tokio::sync::Notify` per queue). Natural backpressure: workers wait instead of aggressive polling.

### 4.7 Worker client (crate `jobline-client`)

```rust
let client = WorkerClient::connect("http://localhost:7500")?;
let mut lease = client.poll("emails", PollOpts::default()).await?;   // receives Job
// automatic heartbeat every timeout/3 while the lease lives
do_work(&lease.job).await?;
client.ack(&lease).await?;
```

- Heartbeat as an internal `tokio` task, cancelled on ack/nack.
- High-level `process_one(handler)`: poll loop → run closure → ack/nack with the closure's backoff. The "Sidekiq-like" API.

### 4.8 Failure modes (threat table)

| Failure | Detection | Response |
|---|---|---|
| Worker crashes mid-job | Lease deadline passes | Job becomes visible again (re-delivery) |
| Worker slow/hung | Heartbeats cease | Lease expires; job re-delivered (possible duplicate → idempotency) |
| Coordinator crashes (kill -9) | — | WAL+snapshot replay at startup; leased jobs re-evaluated |
| Torn write in WAL | Invalid CRC on replay | Truncate at last valid record |
| Disk full | fsync error | Reject enqueues with 503; alert; never fake success |
| Malicious/buggy worker: acks someone else's lease | Opaque, ephemeral LeaseId | Low risk; documented (no auth in v1) |
| Producer retries enqueue (network timeout) | Idempotency key | Dedup → same `job_id`, no duplicate |
| Nack-storm (worker in failure loop) | attempts grows | Exponential backoff → DLQ |

### 4.9 Backpressure and limits

- Cap of inflight jobs per queue (e.g. 100k) → enqueue returns `429/queue_full`. The producer must retry with backoff.
- Max payload 1 MiB.
- `max_jobs` per poll ≤ 10 (prevent one worker taking half the system).
- Connection: HTTP/1.1 keep-alive; reasonable per-IP connection limit (e.g. 512).

### 4.10 Observability (phase 6)

- Prometheus metrics: `jobline_enqueues_total`, `jobline_polls_total`, `jobline_acks_total`, `jobline_lease_expirations_total`, `jobline_dlq_entries_total`, `jobline_wal_bytes_written`, p50/p99 latency per endpoint, age of oldest job per queue.
- Structured logging (tracing): one span per request with `job_id`/`lease_id`.
- Health: `GET /healthz` (alive) and `GET /readyz` (recovery completed).

---

## 5. Project structure

This section defines the repository layout. Consult it when creating the repo and whenever reorganising code.

### 5.1 Cargo workspace

```
jobline/
├── Cargo.toml                  # workspace root, [workspace.members], unified version
├── README.md
├── docs/                       # documentation set (this master document)
├── crates/
│   ├── jobline-proto/          # shared types + serde
│   ├── jobline-core/           # engine: state machine, leases, backoff (NO I/O)
│   ├── jobline-store/          # WAL writer/reader, CRC32, snapshots, recovery
│   ├── jobline-coordinator/    # bin: HTTP server + engine wiring (tokio/axum)
│   ├── jobline-client/         # lib: producer + worker client
│   ├── joblinectl/             # bin: admin CLI
│   └── jobline-load/           # bin: load generator (phase 5)
├── examples/
│   └── demo_worker/            # example worker using jobline-client
├── tests/
│   ├── integration/            # end-to-end: coordinator + workers in process
│   └── failure_drills/         # kill -9, torn WAL, disk full
├── compose.yaml                # coordinator + prometheus + grafana
└── .github/workflows/ci.yml
```

`Cargo.toml` (workspace): resolver = "2", edition 2021, common lints (clippy pedantic on non-bin crates), version 0.1.0.

### 5.2 Crates in detail

#### jobline-proto — `crates/jobline-proto/`

```
src/
├── lib.rs            # re-exports everything
├── ids.rs            # JobId (ULID), LeaseId (UUIDv4), QueueName (validated newtype)
├── job.rs            # Job, Lease, JobState
├── api.rs            # EnqueueRequest/Response, PollRequest/Response, AckResponse, ErrorBody
└── codec.rs          # bincode helpers (feature "wal") — separate so the client doesn't pull in deps
```

- Dependencies only: `serde`, `serde_json`, `ulid`, `uuid`, `bytes` (+ `bincode`/`postcard` behind the `wal` feature flag).
- No `tokio`, no I/O. It is the contract between producer, coordinator and workers.

#### jobline-core — `crates/jobline-core/`

```
src/
├── lib.rs
├── engine.rs         # Engine: owns queues, applies commands, returns LogRecords to persist
├── queue.rs          # Queue: available heap, leased map, dlq, dedup
├── lease.rs          # LeaseManager: grant/expire/heartbeat, timer tick, reaper
├── backoff.rs        # BackoffPolicy trait + ExponentialBackoff {base, cap, jitter}
├── policy.rs         # QueueConfig: visibility_timeout, max_attempts, inflight_cap, payload_cap
└── error.rs          # EngineError (QueueFull, JobNotFound, LeaseExpired...)
```

- **Pure and testable**: `Engine` takes `&dyn Clock` (trait with `now()`) for deterministic tests, and emits `Vec<LogRecord>` for the coordinator to persist. Core knows nothing about WAL or HTTP → instant unit tests, easy property tests.
- Key design point: `Engine::apply(Command) -> Result<Vec<LogRecord>, EngineError>`.

#### jobline-store — `crates/jobline-store/`

```
src/
├── lib.rs
├── wal/
│   ├── writer.rs     # WalWriter: append + group commit + fdatasync + segment rotation
│   ├── reader.rs     # WalReader: iteration over segments, CRC validation, tail truncation
│   ├── record.rs     # LogRecord ↔ bytes (bincode) + CRC32 wrapper
│   └── segment.rs    # names, offsets, rotation/retention policy
├── snapshot/
│   ├── writer.rs     # SnapshotWriter: serializes EngineState, header {seq, wal_offset}, atomic fsync
│   ├── reader.rs     # recovery: snapshot + replay
│   └── gc.rs         # snapshot and old-WAL retention
└── crc32.rs          # CRC32 IEEE table (const fn, no external deps)
```

#### jobline-coordinator — `crates/jobline-coordinator/`

```
src/
├── main.rs           # CLI args (port, data dir), tracing init, startup
├── server.rs         # axum router, shared state
├── handlers/
│   ├── jobs.rs       # enqueue, poll (long-poll with Notify), stats
│   ├── leases.rs     # ack, nack, heartbeat
│   └── admin.rs      # dlq list/requeue, healthz/readyz
├── state.rs          # AppState: Engine + WalWriter + SnapshotScheduler + per-queue Notify
├── snapshotter.rs    # tokio task: timer + record threshold → snapshot
└── recovery.rs       # orchestrates: find snapshots → replay → build Engine
```

Enqueue flow: handler validates → `engine.apply(Enqueue)` → `wal.append(records).await` (group commit) → respond 202. If the WAL fails → 503, the change is **not** applied.

#### jobline-client — `crates/jobline-client/`

```
src/
├── lib.rs
├── producer.rs       # ProducerClient::enqueue(queue, payload, idempotency_key)
├── worker.rs         # WorkerClient: poll (with long-poll), ack, nack
├── heartbeat.rs      # tokio task renewing the lease every deadline/3
├── runner.rs         # run(handler): poll→handle→ack loop, with local circuit breaker
└── config.rs
```

Only `reqwest` (+ `tokio`, `serde` via proto). Published as a reusable library — external workers depend only on this.

#### joblinectl — `crates/joblinectl/`

Subcommands: `enqueue`, `inspect <job>`, `stats <queue>`, `dlq list <queue>`, `dlq requeue <queue> <job>`, `recover --dry-run` (inspects WAL/snapshots without serving).

#### jobline-load — `crates/jobline-load/` (phase 5)

Configurable generator: N producers × M jobs, K workers, ramp. Measures: enqueues/s, e2e latency (enqueue→ack), p99 poll latency, lease expiration rate. Exports CSV/JSON for the performance report.

### 5.3 Main dependencies

| Dep | Use | Alternative considered |
|---|---|---|
| tokio + axum | async runtime + HTTP | actix-web (more magic, worse for learning) |
| serde/serde_json | JSON API | — |
| bincode (or postcard) | WAL + snapshot | JSON (discarded: slow and verbose) |
| ulid | sortable JobId | UUIDv4 (not sortable) |
| tracing + tracing-subscriber | structured logging | log crate |
| metrics + metrics-exporter-prometheus | phase 6 | opentelemetry (overkill) |
| proptest | WAL/recovery property tests | quickcheck |
| tempfile | disk tests | — |
| reqwest | HTTP client | raw hyper (more control, more code) |

Rule: **new dependencies require justification in the PR**. This project is also a systems-programming exercise.

### 5.4 Conventions

- `Engine` and all lease logic: sync, no async. Async only at the boundary (handlers, WAL writer, heartbeat).
- Errors: `thiserror` in libs, `anyhow` only in bins.
- Tests: unit tests in each crate (`#[cfg(test)]`), integration/ and failure_drills/ against real binaries.
- CI: fmt → clippy -D warnings → tests (incl. failure drills on Linux) → release build.

---

## 6. Implementation: the six phases

Budget: 1,040 h over 52 weeks. Every phase ends with a compiling repository, green tests and a working demo — the repo is never left broken between phases.

Each phase delivers one visible capability to the same product:

| Phase | Capability delivered to `jobline` |
|---|---|
| 1. Rust Core & Ownership | In-memory queue store + CLI (enqueue/poll/ack) |
| 2. Advanced Rust & Storage | Durable coordinator state: WAL, snapshots, crash recovery |
| 3. Async Rust & Networking | HTTP service, worker client crate, load tests |
| 4. Reliable Delivery & Fault Tolerance | Leases, heartbeats, retries, DLQ, failure drills |
| 5. Performance & Systems Programming | Measured optimisation cycle, performance report |
| 6. Production Readiness | Observability, Docker, CI/CD, `v1.0.0-rc1` |

Every milestone keeps its original weeks; the "Scope adjustment" column states what is trimmed or deferred so the milestone fits 40–60 h. Where a design concept appears in the milestone tables, its full definition is in section 4 (design).

### 6.1 Phase 1 — Rust Core & Ownership (weeks 1–8 · 160 h)

**Goal:** an in-memory queue + CLI. No networking yet.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M1.1 Toolchain, Cargo, modules | 1–2 | 40 | Workspace + `Job`, `JobId`, `Queue`, `JobState` types | As original |
| M1.2 Ownership, borrowing | 3–4 | 40 | `InMemoryJobStore` with enqueue/poll/ack, no needless clones | As original |
| M1.3 Traits, generics, errors | 5–6 | 40 | `StoreError` (`thiserror`); `Store` trait abstraction | As original |
| M1.4 Tests, CLI, integration | 7–8 | 40 | CLI (`enqueue`/`poll`/`ack`) with `clap` | **≥ 20 tests** (was ≥ 30) |

**What you build**

- Workspace + `jobline-proto` (JobId = ULID, Job, QueueName validation with `newtype` + `TryFrom<&str>`).
- `jobline-core`: `Engine`, `Queue` with `BinaryHeap<Reverse<(InstantSys, JobId)>>`, pure `apply()` with `&dyn Clock`.
- `joblinectl` with `enqueue/poll/ack` subcommands against an in-memory Engine ("single-process demo" mode).
- Engine unit tests (FIFO delivery, ordering by `visible_at`, dedup by idempotency key).

**Rust skills trained here:** ownership and borrowing in the engine (the ID heap vs. the Job map — the "index + storage" pattern), `newtype`s for IDs, trait objects for the clock, `BinaryHeap`, iterators.

### 6.2 Phase 2 — Advanced Rust & Storage (weeks 9–16 · 160 h)

**Goal:** real durability. WAL + snapshots + crash recovery.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M2.1 Lifetimes, advanced traits | 9–10 | 40 | Job iterators; delayed jobs (`run_at`) sweep; `Cow`/`Bytes` reads | As original |
| M2.2 Concurrency & atomics | 11–12 | 40 | `ConcurrentJobStore`; 4-producer/4-consumer benchmark | Targeted `loom` test for one case (was 60 s continuous) |
| M2.3 Memory layout, file I/O | 13–14 | 40 | WAL record format (header + CRC32); `WalWriter`/`WalReader` | Minimal `unsafe` (CRC/bytes only); selective Rustonomicon |
| M2.4 Durable store & recovery | 15–16 | 40 | `PersistentStore`: WAL replay + snapshot; crash-recovery test | Simplified snapshot policy (no auto-compaction) |

**What you build**

- `jobline-store`: `WalWriter` with group commit (an `mpsc` channel + dedicated task that batches appends and calls `fdatasync`), segment rotation, CRC32 with a `const fn` table.
- `WalReader` with tail truncation + `SnapshotWriter/Reader` + GC + `recovery.rs` (snapshot → replay).
- Integration: an "embryonic" coordinator binary that applies commands and persists them (still no network: reads commands from stdin, or the CLI talks to disk).
- **Property tests (proptest)**: for any command sequence, `apply(seq)` == `recover(wal_of(seq))`. This is the project's star test.
- Initial failure drills: kill -9 the process between commands → correct recovery; torn write (truncate the WAL mid-record) → truncate and continue.

**Rust skills trained here:** minimal, justified `unsafe` (maybe mmap for the WAL — otherwise normal I/O with `writev`), `io::Write` + `File::sync_data`, error handling with enums, binary serialization, binary format design.

### 6.3 Phase 3 — Async Rust & Networking (weeks 17–24 · 160 h)

**Goal:** HTTP service + client crate.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M3.1 Futures, async/await, Tokio | 17–18 | 40 | Async service skeleton; WAL I/O via `spawn_blocking` + ADR-003 | As original |
| M3.2 Tasks, channels, shutdown | 19–20 | 40 | Connection pipeline with bounded channels; graceful SIGTERM drain | As original |
| M3.3 HTTP API with Axum | 21–22 | 40 | Enqueue/poll/ack endpoints + `/health`, `/ready`; tower limits | **HTTP/JSON only** — no custom TCP framing, no RESP protocol |
| M3.4 Client, backpressure, load | 23–24 | 40 | Worker client crate with pooling/retries; P50/P95/P99 | Load: **500 concurrent connections** (was 1,000) |

**What you build**

- `jobline-coordinator` with axum: enqueue/poll/ack/nack/heartbeat/stats endpoints.
- Long-poll with `tokio::sync::Notify` per queue + `tokio::time::timeout`.
- `jobline-client`: producer, worker, heartbeat task (`tokio::spawn` + cancellation on ack via `Drop`), high-level runner.
- Backpressure: inflight caps, `429 queue_full`.
- Load tests (the seed of `jobline-load`): start with 50–100 connections and ramp up to the M3.4 target of 500 concurrent connections.

**Rust skills trained here:** `async/await`, `Arc<AppState>` vs. fine-grained locking (the engine sits behind a `Mutex` or per-queue `RwLock` — measure), response streaming, graceful shutdown (SIGTERM → final snapshot).

### 6.4 Phase 4 — Reliable Delivery & Fault Tolerance (weeks 25–36 · 240 h)

**Goal:** leases, heartbeats, retries, DLQ + full failure drills.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M4.1 Delivery semantics, idempotency | 25–27 | 60 | At-least-once via leases (visibility timeout); idempotency keys; `semantics.md`; ADR-004 | As original |
| M4.2 Leases, heartbeats, crash detection | 28–30 | 60 | Lease renewal; worker heartbeat; expiry reclaim; visibility metrics | Replaces log shipping; documented lease model (SQS-style) |
| M4.3 Retries, backoff, dead-letter queue | 31–33 | 60 | Exponential backoff; DLQ with requeue; `retries.md`; ADR-005 | Replaces the consistency milestone; no replication guarantees needed |
| M4.4 Failure drills & fault injection | 34–35 | 40 | Coordinator restart from WAL; `toxiproxy` suite (worker crash mid-job, dropped acks) | **≥ 6 fault tests** (was ≥ 10) |
| M4.5 Resilience documentation | 36 | 20 | Failure-mode table, recovery runbook, README "Guarantees" (single-coordinator SPOF stated) | Compact, defensible version |

**What you build**

- Lease manager with periodic reaper (1 s tick, `tokio::time::interval`).
- Heartbeat endpoint + deadline renewal.
- Exponential backoff with jitter (trait `BackoffPolicy` so tests can use fixed backoff).
- DLQ + admin (list/requeue/purge) + per-queue `max_attempts`.
- **Automated failure drills** (integration tests):
  - kill -9 a worker mid-job → job re-delivered, attempts+1;
  - heartbeats cease → lease expires;
  - coordinator kill -9 → recovery, leased jobs re-evaluated;
  - nack-storm → reaches DLQ after max_attempts;
  - producer timeout + retry with the same idempotency key → no duplicate.
- Determinism in tests: `tokio::time::pause()` + injected `Clock`.

### 6.5 Phase 5 — Performance & Systems Programming (weeks 37–44 · 160 h)

**Goal:** a measured optimisation cycle + performance report.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M5.1 Benchmarks & profiling | 37–38 | 40 | Criterion suites + flamegraphs; top 3 hot functions | As original |
| M5.2 CPU, memory, allocations | 39–40 | 40 | `dhat` audit; `String`→`Bytes`; allocation reduction | Target: **≥ 30 %** fewer allocations (was ≥ 50 %) |
| M5.3 Cache, layout, contention | 41–42 | 40 | One optimisation cycle with before/after numbers | Queue-head lock contention; shard by queue only if profiling justifies it |
| M5.4 Linux I/O & report | 43–44 | 40 | fsync/fadvise options; cumulative performance report | SIMD, `mmap`, `io_uring`, deep unsafe audit → stretch goals |

**Working method: measure → hypothesis → change → re-measure (documented)**

- Baseline: enqueues/s, poll→ack e2e latency p50/p99, with 500 connections.
- Typical hypotheses: more aggressive group commit (1 ms batches), `Bytes` vs `String` for payloads, lock contention (shard the engine into N per-queue `Mutex`es), `io_uring` only if time remains (officially out of scope).

**Phase success criterion:** 500 sustained connections with e2e p99 under X ms (define X in week 1 of the phase from the baseline) and the report published in `docs/perf-report.md`.

### 6.6 Phase 6 — Production Readiness (weeks 45–52 · 160 h)

**Goal:** observability, Docker, CI/CD, `v1.0.0-rc1`.

**Milestones and adjusted scope**

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M6.1 Observability | 45–46 | 40 | `tracing` + Prometheus metrics; Jaeger traces; dashboard | As original |
| M6.2 Configuration, Docker | 47–48 | 40 | Multi-stage Dockerfile < 50 MB; compose: 1 coordinator + 2 workers | Kubernetes/StatefulSet → stretch goal |
| M6.3 CI/CD, soak testing | 49–50 | 40 | CI (fmt, clippy, test, audit); soak test | Soak: **30 min** (was 1 h) |
| M6.4 Docs, ADRs, release | 51–52 | 40 | Quickstart; ≥ 8 ADRs; tag `v1.0.0-rc1`; 15-min walkthrough | **≥ 8 ADRs** (was 10) |

**What you build**

- Prometheus metrics + Grafana dashboard (compose).
- End-to-end structured tracing with `job_id`/`lease_id` spans.
- healthz/readyz, graceful shutdown, disk-full handling (503 + alert, no data loss).
- Multi-stage Dockerfile (distroless builder), `compose.yaml` (coordinator + prometheus + grafana + demo worker), CI (fmt/clippy/test/failure-drills/build), release workflow with changelogs.
- `v1.0.0-rc1` + operations documentation (runbook: manual recovery, DLQ, disk expansion).

### 6.7 Stretch goals (outside the 1,040-hour budget)

Documented stretch goals, all out of scope until `v1.0.0-rc1`: multi-coordinator high availability with consensus, priority queues and pub/sub fan-out, TLS/auth, Kubernetes deployment, 1,000+ concurrent connections, advanced I/O (`io_uring`/`mmap`/SIMD). Keep them in a `STRETCH.md` file in the repository so scope creep has a documented home. If real progress allows, prioritise in this order: **coordinator HA → Kubernetes → advanced I/O**.

---

## 7. Testing strategy and risks

### 7.1 Testing strategy (transversal)

| Layer | Technique | Guarantees |
|---|---|---|
| Engine | unit + proptest (arbitrary command sequences) | queue/lease semantics |
| WAL | property: recover(write(seq)) == seq applied | durability and recovery |
| HTTP | integration tests with `tower::ServiceExt` | API contract |
| System | failure drills with real binaries + kill -9 | behavior under real failures |
| Load | jobline-load against a release build | phase 5 criteria |

Golden rule: **every bug found by a failure drill becomes a permanent test**.

### 7.2 Risks and mitigations

| Risk | Likelihood | Mitigation |
|---|---|---|
| Group commit complexity | medium | start with fsync-per-append (correct, slow); optimize in phase 5 |
| Engine lock contention | medium | per-queue sharding from phase 3; measure before optimizing |
| Long-poll + Notify race (job arrives after check) | high | double-check after registering the waiter; stress test |
| Slow recovery with a large WAL | low | frequent snapshots + benchmark in phase 2 |
| Scope creep (auth, TLS, priorities) | high | out of scope until v1.0.0-rc1; keep a `STRETCH.md` |

---

## 8. Weekly review and progress

### 8.1 Weekly review

Close each week by answering in the learning log:

1. What was learned and implemented?
2. What code was written without following a tutorial?
3. Which tests prove the expected behaviour?
4. Which technical problem consumed the most time?
5. Which design decision can be explained and defended?
6. What is pending, and does it affect the calendar?

### 8.2 Phase review (every 8 weeks)

At the end of each phase, hold one extra half-session to validate the phase criteria, update the tracker, and decide whether any stretch goal enters the plan or is deferred.

### 8.3 Progress tracker

| Phase | Weeks | Status | Start date | End date | Evidence |
|---|---|---|---|---|---|
| 1. Rust Core & Ownership | 1–8 | Not started | | | |
| 2. Advanced Rust & Storage | 9–16 | Not started | | | |
| 3. Async Rust & Networking | 17–24 | Not started | | | |
| 4. Reliable Delivery & Fault Tolerance | 25–36 | Not started | | | |
| 5. Performance & Systems Programming | 37–44 | Not started | | | |
| 6. Production Readiness | 45–52 | Not started | | | |

Status values: `Not started`, `In progress`, `Completed`, `Needs review`.

### 8.4 Milestone checklist

Mark a milestone `[x]` only when its adjusted acceptance criteria (section 6) are met.

**Phase 1 — Rust Core & Ownership (weeks 1–8)**

- [ ] M1.1 (weeks 1–2) — Workspace + `Job`, `JobId`, `Queue`, `JobState` types
- [ ] M1.2 (weeks 3–4) — `InMemoryJobStore` with enqueue/poll/ack, no needless clones
- [ ] M1.3 (weeks 5–6) — `StoreError` (`thiserror`); `Store` trait abstraction
- [ ] M1.4 (weeks 7–8) — CLI (`enqueue`/`poll`/`ack`) with `clap`; ≥ 20 tests

**Phase 2 — Advanced Rust & Storage (weeks 9–16)**

- [ ] M2.1 (weeks 9–10) — Job iterators; delayed jobs (`run_at`) sweep; `Cow`/`Bytes` reads
- [ ] M2.2 (weeks 11–12) — `ConcurrentJobStore`; 4-producer/4-consumer benchmark; targeted `loom` test
- [ ] M2.3 (weeks 13–14) — WAL record format (header + CRC32); `WalWriter`/`WalReader`
- [ ] M2.4 (weeks 15–16) — `PersistentStore`: WAL replay + snapshot; crash-recovery test

**Phase 3 — Async Rust & Networking (weeks 17–24)**

- [ ] M3.1 (weeks 17–18) — Async service skeleton; WAL I/O via `spawn_blocking`; ADR-003
- [ ] M3.2 (weeks 19–20) — Connection pipeline with bounded channels; graceful SIGTERM drain
- [ ] M3.3 (weeks 21–22) — Enqueue/poll/ack endpoints + `/health`, `/ready`; tower limits
- [ ] M3.4 (weeks 23–24) — Worker client crate with pooling/retries; P50/P95/P99; load to 500 connections

**Phase 4 — Reliable Delivery & Fault Tolerance (weeks 25–36)**

- [ ] M4.1 (weeks 25–27) — At-least-once via leases; idempotency keys; `semantics.md`; ADR-004
- [ ] M4.2 (weeks 28–30) — Lease renewal; worker heartbeat; expiry reclaim; visibility metrics
- [ ] M4.3 (weeks 31–33) — Exponential backoff; DLQ with requeue; `retries.md`; ADR-005
- [ ] M4.4 (weeks 34–35) — Coordinator restart from WAL; `toxiproxy` suite; ≥ 6 fault tests
- [ ] M4.5 (week 36) — Failure-mode table, recovery runbook, README "Guarantees" (SPOF stated)

**Phase 5 — Performance & Systems Programming (weeks 37–44)**

- [ ] M5.1 (weeks 37–38) — Criterion suites + flamegraphs; top 3 hot functions
- [ ] M5.2 (weeks 39–40) — `dhat` audit; `String`→`Bytes`; ≥ 30 % fewer allocations
- [ ] M5.3 (weeks 41–42) — One optimisation cycle with before/after numbers
- [ ] M5.4 (weeks 43–44) — fsync/fadvise options; cumulative performance report

**Phase 6 — Production Readiness (weeks 45–52)**

- [ ] M6.1 (weeks 45–46) — `tracing` + Prometheus metrics; Jaeger traces; dashboard
- [ ] M6.2 (weeks 47–48) — Multi-stage Dockerfile < 50 MB; compose: 1 coordinator + 2 workers
- [ ] M6.3 (weeks 49–50) — CI (fmt, clippy, test, audit); 30-minute soak test
- [ ] M6.4 (weeks 51–52) — Quickstart; ≥ 8 ADRs; tag `v1.0.0-rc1`; 15-min walkthrough

---

## 9. Glossary

| Term | Meaning |
|---|---|
| ADR (Architecture Decision Record) | Short document capturing one design decision, its context and its consequences; the plan targets ≥ 8 by release. |
| At-least-once delivery | Guarantee that a job is delivered one or more times — never zero; duplicates are possible and handled through idempotency. |
| Backoff (exponential, with jitter) | Retry delay that doubles per attempt and is capped: `min(cap, base * 2^attempts)`, randomised ±20 % so failing workers do not retry in lock-step. |
| Backpressure | Mechanism that slows producers/workers when the system is saturated: inflight caps → `429 queue_full`; long-poll makes workers wait instead of aggressive polling. |
| CRC32 | Checksum stored with each WAL record; detects corrupted or half-written data during replay. |
| DLQ (dead-letter queue) | Holding area for jobs that failed more than `max_attempts` times; admins can list or requeue them. |
| dhat | Rust heap profiler used to audit and reduce allocations (phase 5 target: ≥ 30 % fewer). |
| fdatasync / fsync | System calls that force file data (fdatasync skips some metadata) to physical disk; the coordinator syncs before acknowledging an enqueue. |
| Group commit | Batching many WAL appends (~1 ms window or N records) into a single `fdatasync`, so real durability does not destroy throughput. |
| Heartbeat | Periodic worker call that extends a lease's deadline; if heartbeats stop, the lease expires and the job is re-delivered. |
| Idempotency / idempotency key | Property that repeating an operation has no additional effect; producers send a key so a retried enqueue returns the same `job_id` instead of creating a duplicate. |
| kill -9 | Forceful process kill (SIGKILL) with no cleanup — exactly the scenario the WAL recovery path must survive. |
| Lease / visibility timeout | Time-boxed ownership of a delivered job (default 30 s per queue): if the worker does not ack within it, the job becomes visible again for another worker. |
| Long-poll | Poll request the server holds open (up to `wait_ms`, max 20 s) until a job arrives or the wait expires, instead of returning empty immediately. |
| loom | Rust library for exhaustive testing of concurrent code; used here for one targeted concurrency test (M2.2). |
| proptest | Property-testing library: generates arbitrary command sequences and asserts invariants such as `apply(seq) == recover(wal_of(seq))`. |
| SIGTERM / graceful shutdown | Termination signal the coordinator handles by draining connections and writing a final snapshot. |
| SPOF (single point of failure) | Component whose failure stops the whole system; jobline's single coordinator is a documented, accepted SPOF. |
| Soak test | Long-duration run under sustained load to surface leaks and slow degradation; 30 minutes before release (M6.3). |
| Tail truncation | Recovery rule that discards WAL bytes after the last valid record when a torn write is found. |
| Torn write | A write cut off midway by a crash or power loss; detected via length/CRC checks and handled by truncation. |
| toxiproxy | TCP proxy that injects network faults; used by the phase-4 failure-drill suite. |
| ULID | Sortable unique identifier used for `JobId` (UUIDv4 is not sortable, which hurts snapshot ordering). |
| WAL (write-ahead log) | Append-only file recording every state mutation before it is applied; replaying it rebuilds the coordinator's in-memory state after a crash. |
| Worker pool | Set of independent worker processes (not threads) that poll, lease, execute and ack jobs. |
| writev | "Scatter/gather" write system call used to append a record's parts in one operation. |

---
**Guiding principle (unchanged):** *Build, measure, break, recover, and explain.* The goal is to demonstrate that you can design, implement, test and operate reliable distributed software in Rust — around a project that genuinely fits 1,040 hours.
