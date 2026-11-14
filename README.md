# 🦀 Rust Expert Journey — Adjusted Plan v3 (12 months · 20 h/week)

**A sustainable transition from Senior Java Engineer to Rust Backend Engineer in 52 weeks: 4 hours a day, Monday to Friday — 1,040 total hours — driven by one clearly bounded project: a distributed task queue.**

[![Rust](https://img.shields.io/badge/language-Rust-orange?logo=rust)](https://www.rust-lang.org/)
[![Duration](https://img.shields.io/badge/duration-12%20months-blue)](#4-programme-at-a-glance)
[![Effort](https://img.shields.io/badge/effort-20%20hours%2Fweek-informational)](#5-study-method-4-hday)
[![Focus](https://img.shields.io/badge/focus-70%25%20backend%20%2F%2030%25%20systems%20programming-success)](#3-learning-allocation-7030)

## Table of contents

1. [Plan adjustment summary](#1-plan-adjustment-summary)
2. [The driving project: `jobline`, a distributed task queue](#2-the-driving-project-jobline-a-distributed-task-queue)
3. [Learning allocation: 70/30](#3-learning-allocation-7030)
4. [Programme at a glance](#4-programme-at-a-glance)
5. [Study method (4 h/day)](#5-study-method-4-hday)
6. [Phases and milestones with adjusted scope](#6-phases-and-milestones-with-adjusted-scope)
7. [Weekly review](#7-weekly-review)
8. [Progress tracker](#8-progress-tracker)

---

## 1. Plan adjustment summary

The original plan (52 weeks × 30 h = 1,560 h) is too intense to sustain alongside a full-time job. This revision keeps the **same 12-month duration** and lowers the commitment to **20 h/week (4 h/day, Monday to Friday)**, giving **1,040 total hours (−33 %)**. Two changes make the programme realistic at this capacity:

1. **New driving project.** The Distributed Event Processing Platform (a Kafka-like log with custom consensus) was too ambitious for this budget. The replacement keeps genuine distributed-systems content but is bounded: **`jobline`, a distributed task queue** (section 2), selected from three candidates in section 2.3.
2. **Reduced scope per milestone.** Every milestone keeps its original weeks and ships a working capability, with explicit cuts; advanced topics move to optional stretch goals.

| Parameter | Original plan | Adjusted plan (v3) |
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

## 2. The driving project: `jobline`, a distributed task queue

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

### 2.5 How the phases map to the product

| Phase | Capability delivered to `jobline` |
|---|---|
| 1. Rust Core & Ownership | In-memory queue store + CLI (enqueue/poll/ack) |
| 2. Advanced Rust & Storage | Durable coordinator state: WAL, snapshots, crash recovery |
| 3. Async Rust & Networking | HTTP service, worker client crate, load tests |
| 4. Reliable Delivery & Fault Tolerance | Leases, heartbeats, retries, DLQ, failure drills |
| 5. Performance & Systems Programming | Measured optimisation cycle, performance report |
| 6. Production Readiness | Observability, Docker, CI/CD, `v1.0.0-rc1` |

## 3. Learning allocation: 70/30

| Track | Share | Hours | Primary outcome |
|---|---:|---:|---|
| Distributed systems & high-performance backend | 70 % | 728 h | Design, build, test and operate `jobline` |
| Systems programming & low-level Rust | 30 % | 312 h | Memory, concurrency, file I/O, Linux, performance |
| **Total** | **100 %** | **1,040 h** | 12-month programme |

## 4. Programme at a glance

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

## 5. Study method (4 h/day)

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

## 6. Phases and milestones with adjusted scope

Every milestone keeps its original weeks; the "Scope adjustment" column states what is trimmed or deferred so the milestone fits 40–60 h.

### Phase 1 — Rust Core & Ownership (weeks 1–8 · 160 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M1.1 Toolchain, Cargo, modules | 1–2 | 40 | Workspace + `Job`, `JobId`, `Queue`, `JobState` types | As original |
| M1.2 Ownership, borrowing | 3–4 | 40 | `InMemoryJobStore` with enqueue/poll/ack, no needless clones | As original |
| M1.3 Traits, generics, errors | 5–6 | 40 | `StoreError` (`thiserror`); `Store` trait abstraction | As original |
| M1.4 Tests, CLI, integration | 7–8 | 40 | CLI (`enqueue`/`poll`/`ack`) with `clap` | **≥ 20 tests** (was ≥ 30) |

### Phase 2 — Advanced Rust & Storage (weeks 9–16 · 160 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M2.1 Lifetimes, advanced traits | 9–10 | 40 | Job iterators; delayed jobs (`run_at`) sweep; `Cow`/`Bytes` reads | As original |
| M2.2 Concurrency & atomics | 11–12 | 40 | `ConcurrentJobStore`; 4-producer/4-consumer benchmark | Targeted `loom` test for one case (was 60 s continuous) |
| M2.3 Memory layout, file I/O | 13–14 | 40 | WAL record format (header + CRC32); `WalWriter`/`WalReader` | Minimal `unsafe` (CRC/bytes only); selective Rustonomicon |
| M2.4 Durable store & recovery | 15–16 | 40 | `PersistentStore`: WAL replay + snapshot; crash-recovery test | Simplified snapshot policy (no auto-compaction) |

### Phase 3 — Async Rust & Networking (weeks 17–24 · 160 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M3.1 Futures, async/await, Tokio | 17–18 | 40 | Async service skeleton; WAL I/O via `spawn_blocking` + ADR-003 | As original |
| M3.2 Tasks, channels, shutdown | 19–20 | 40 | Connection pipeline with bounded channels; graceful SIGTERM drain | As original |
| M3.3 HTTP API with Axum | 21–22 | 40 | Enqueue/poll/ack endpoints + `/health`, `/ready`; tower limits | **HTTP/JSON only** — no custom TCP framing, no RESP protocol |
| M3.4 Client, backpressure, load | 23–24 | 40 | Worker client crate with pooling/retries; P50/P95/P99 | Load: **500 concurrent connections** (was 1,000) |

### Phase 4 — Reliable Delivery & Fault Tolerance (weeks 25–36 · 240 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M4.1 Delivery semantics, idempotency | 25–27 | 60 | At-least-once via leases (visibility timeout); idempotency keys; `semantics.md`; ADR-004 | As original |
| M4.2 Leases, heartbeats, crash detection | 28–30 | 60 | Lease renewal; worker heartbeat; expiry reclaim; visibility metrics | Replaces log shipping; documented lease model (SQS-style) |
| M4.3 Retries, backoff, dead-letter queue | 31–33 | 60 | Exponential backoff; DLQ with requeue; `retries.md`; ADR-005 | Replaces the consistency milestone; no replication guarantees needed |
| M4.4 Failure drills & fault injection | 34–35 | 40 | Coordinator restart from WAL; `toxiproxy` suite (worker crash mid-job, dropped acks) | **≥ 6 fault tests** (was ≥ 10) |
| M4.5 Resilience documentation | 36 | 20 | Failure-mode table, recovery runbook, README "Guarantees" (single-coordinator SPOF stated) | Compact, defensible version |

### Phase 5 — Performance & Systems Programming (weeks 37–44 · 160 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M5.1 Benchmarks & profiling | 37–38 | 40 | Criterion suites + flamegraphs; top 3 hot functions | As original |
| M5.2 CPU, memory, allocations | 39–40 | 40 | `dhat` audit; `String`→`Bytes`; allocation reduction | Target: **≥ 30 %** fewer allocations (was ≥ 50 %) |
| M5.3 Cache, layout, contention | 41–42 | 40 | One optimisation cycle with before/after numbers | Queue-head lock contention; shard by queue only if profiling justifies it |
| M5.4 Linux I/O & report | 43–44 | 40 | fsync/fadvise options; cumulative performance report | SIMD, `mmap`, `io_uring`, deep unsafe audit → stretch goals |

### Phase 6 — Production Readiness (weeks 45–52 · 160 h)

| Milestone | Weeks | Hours | Deliverable | Scope adjustment |
|---|---|---:|---|---|
| M6.1 Observability | 45–46 | 40 | `tracing` + Prometheus metrics; Jaeger traces; dashboard | As original |
| M6.2 Configuration, Docker | 47–48 | 40 | Multi-stage Dockerfile < 50 MB; compose: 1 coordinator + 2 workers | Kubernetes/StatefulSet → stretch goal |
| M6.3 CI/CD, soak testing | 49–50 | 40 | CI (fmt, clippy, test, audit); soak test | Soak: **30 min** (was 1 h) |
| M6.4 Docs, ADRs, release | 51–52 | 40 | Quickstart; ≥ 8 ADRs; tag `v1.0.0-rc1`; 15-min walkthrough | **≥ 8 ADRs** (was 10) |

**Documented stretch goals (outside the 1,040 h budget):** multi-coordinator high availability with consensus, priority queues and pub/sub fan-out, TLS/auth, Kubernetes deployment, 1,000+ concurrent connections, advanced I/O (`io_uring`/`mmap`/SIMD). If real progress allows, prioritise in this order: coordinator HA → Kubernetes → advanced I/O.

## 7. Weekly review

Close each week by answering in the learning log:

1. What was learned and implemented?
2. What code was written without following a tutorial?
3. Which tests prove the expected behaviour?
4. Which technical problem consumed the most time?
5. Which design decision can be explained and defended?
6. What is pending, and does it affect the calendar?

Every 8 weeks (end of phase), hold one extra half-session to validate the phase criteria, update the tracker, and decide whether any stretch goal enters the plan or is deferred.

## 8. Progress tracker

| Phase | Weeks | Status | Start date | End date | Evidence |
|---|---|---|---|---|---|
| 1. Rust Core & Ownership | 1–8 | Not started | | | |
| 2. Advanced Rust & Storage | 9–16 | Not started | | | |
| 3. Async Rust & Networking | 17–24 | Not started | | | |
| 4. Reliable Delivery & Fault Tolerance | 25–36 | Not started | | | |
| 5. Performance & Systems Programming | 37–44 | Not started | | | |
| 6. Production Readiness | 45–52 | Not started | | | |

Status values: `Not started`, `In progress`, `Completed`, `Needs review`. The detailed M1.1–M6.4 tracker remains valid with the adjusted acceptance criteria in section 6.

---

**Guiding principle (unchanged):** *Build, measure, break, recover, and explain.* The goal is to demonstrate that you can design, implement, test and operate reliable distributed software in Rust — now around a project that genuinely fits 1,040 hours.
