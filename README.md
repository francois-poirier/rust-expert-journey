# 🦀 52-Week Rust Mastery Roadmap

**Goal:** Become a Senior Rust Engineer, proficient in distributed, fault-tolerant systems, cloud deployment, diagnostics, and large-scale Rust architecture.  
*Approx. 2 h/day · ~10–12 h/week · ~12 months*

---

## Table of Contents

- [Phase 1 — Rust Core Mastery (Weeks 1–8)](#phase-1---rust-core-mastery-weeks-1-8)
- [Phase 2 — Async Rust & Web Backends (Weeks 9–16)](#phase-2---async-rust--web-backends-weeks-9-16)
- [Phase 3 — Concurrency & Messaging (Weeks 17–24)](#phase-3---concurrency--messaging-weeks-17-24)
- [Phase 4 — Distributed Systems (Weeks 25–32)](#phase-4---distributed-systems-weeks-25-32)
- [Phase 5 — Rust in Cloud & Containers (Weeks 33–40)](#phase-5---rust-in-cloud--containers-weeks-33-40)
- [Phase 6 — Agents, Performance & Capstone (Weeks 41–52)](#phase-6---agents-performance--capstone-weeks-41-52)

---

# Phase 1 — Rust Core Mastery (Weeks 1–8)

**Outcome:** Build a strong foundation in Rust’s ownership, types, traits, generics, modules, error handling, and testing.

### Weeks 1–2 — Ownership, Borrowing, Lifetimes
**Study**
- *The Rust Programming Language* (TRPL) — ch.1–4 (Ownership & References, Slices, Lifetimes)
- *Rust by Example* — Ownership, References & Borrowing, Lifetimes

**Exercises**
- Rustlings: `variables`, `functions`, `ownership`, `references`, `borrow`, `slices`, `lifetimes`

**Mini-Project**
- CLI config parser

### Weeks 3–4 — Enums, Pattern Matching, Error Handling
**Study**
- TRPL — ch.6 Enums & Pattern Matching, ch.9 Error Handling
- Rust by Example — Enums, Pattern Matching, Option & Result, error propagation

**Exercises**
- Rustlings: `enums`, `options`, `error_handling`
- Parse user input → `Result` → custom error types

### Weeks 5–6 — Traits, Generics, Modules
**Study**
- TRPL — ch.10 Generics, Traits, and Lifetimes, ch.7 Modules and Crates
- Rust by Example — Traits, Generics, Modules, Visibility

**Exercises**
- Rustlings: `traits`, `generics`, `modules`
- Define and implement custom traits

### Weeks 7–8 — Testing, Logging, CLI Integration
**Study**
- TRPL — ch.11 Testing, ch.12 I/O Project
- Logging crates: `log`, `env_logger`, `tracing`

**Exercises**
- Rustlings: `tests`, `iterators`
- Unit and integration tests for previous mini-projects

**Mini-Project**
- Log Analyzer CLI with tests and logging

---

# Phase 2 — Async Rust & Web Backends (Weeks 9–16)

**Outcome:** Build async web services, REST APIs, interact with databases, and produce production-grade code.

### Weeks 9–10 — Async/Await & Tokio Runtime
**Study**
- *Async Rust Book* — Futures, Async/Await, Executors, Pinning, Cancellation
- Tokio docs — Tasks, Spawning, Timing utilities

**Exercises**
- Rustlings: `async-await`, `futures`, `async-fn`
- Spawn tasks, use `join!`, `select!`, timeouts

**Mini-Project**
- Concurrent Downloader CLI

### Weeks 11–12 — HTTP Servers with Axum
**Study**
- Axum Guide — Routing, Extractors, Responses, JSON handling, Middleware
- Tower & Hyper basics

**Exercises**
- Minimal JSON endpoint with state sharing
- Actor-based worker pipeline

**Project**
- TaskHub REST service (CRUD) — in-memory store

### Weeks 13–14 — Databases & Observability
**Study**
- SQLx — Async queries, pool management, migrations
- Tracing crate — structured logging, spans
- Config: `dotenvy`, `config` crates

**Exercises**
- Async SQLx queries and migrations
- Tracing spans around DB calls and handlers

**Tasks**
- Integrate Postgres persistence
- Structured logs and metrics instrumentation

### Weeks 15–16 — Testing + Dockerization
**Study**
- TRPL I/O & error patterns, Rust testing
- Docker Docs & community guides

**Exercises**
- Integration tests with `reqwest`
- Dockerfile multi-stage build + Docker Compose

**Tasks**
- Containerize services, set up DB
- Verify integration and containerized behavior

---

# Phase 3 — Concurrency & Messaging (Weeks 17–24)

**Outcome:** Master concurrency, async streams, actor models, and message-based distributed systems.

### Weeks 17–18 — Concurrency Primitives
**Study**
- TRPL ch.16 “Fearless Concurrency”, Rust by Example: Threads, Mutex, RwLock, Channels

**Exercises**
- Rustlings: `threads`, `threads2`, `mutex`, `sync`
- Thread-pool implementation
- Atomic counters

**Mini-Project**
- Parallel word-count CLI with benchmark

### Weeks 19–20 — Async Channels & Streams
**Study**
- Async Rust Book — Streams, Channels, Select, Async I/O
- Tokio: `mpsc`, `broadcast`, `watch`, `tokio_stream`

**Exercises**
- Convert sync iterator to Stream
- Producer → Stream → Consumer pipeline

**Project**
- Concurrent web crawler

### Weeks 21–22 — Actor Model in Rust
**Study**
- Actor frameworks: xtra, Ractor, Actix
- Actor model concepts: supervision trees, mailbox semantics

**Exercises**
- Minimal actor with message handling and state
- Supervision tree implementation

**Project**
- Actor-based crawler

### Weeks 23–24 — Messaging & Queue Integration
**Study**
- Kafka/NATS official docs, Rust crates `rdkafka`, `async-nats`

**Exercises**
- Rust producer & consumer with retries
- Connect actor-based workers to broker

**Project**
- Multi-node message pipeline demo

---

# Phase 4 — Distributed Systems (Weeks 25–32)

**Outcome:** Build fault-tolerant, replicated, consistent distributed systems.

### Weeks 25–26 — Distributed Systems Fundamentals
**Study**
- DDIA by Kleppmann: ch.1,2,5,8,9
- Distributed Systems for Fun and Profit: ch.1–6

**Exercises**
- Sketch replication topologies in Rust enums
- In-memory replicated key-value store

**Tasks**
- CLI to simulate N nodes, replication, and delays

### Weeks 27–28 — Implementing Raft in Rust
**Study**
- Raft paper — leader election, log replication, safety
- Optional: `async-raft` crate docs

**Exercises**
- Randomized election timeouts, in-memory log, AppendEntries RPC
- Leader election demo

**Project**
- RustRaft 3-node in-memory cluster

### Weeks 29–30 — Persistent Distributed Key-Value Store
**Study**
- `sled` & `rocksdb` docs, Axum REST API, snapshotting design

**Exercises**
- Persistent log storage, snapshotting, recovery

**Project**
- RaftiKV: RESTful KV store backed by Raft + persistence

### Weeks 31–32 — Fault Injection, Metrics & Chaos Testing
**Study**
- Chaos Engineering book, Prometheus, tracing + OpenTelemetry

**Exercises**
- Network simulation harness
- Metrics instrumentation & Grafana dashboards

**Deliverables**
- Chaos-tested RaftiKV cluster with metrics

---

# Phase 5 — Rust in Cloud & Containers (Weeks 33–40)

**Outcome:** Deploy, monitor, and maintain Rust services in containers and orchestration platforms.

### Weeks 33–34 — Dockerizing Rust Services
**Study**
- Dockerfile best practices, multi-stage builds

**Exercises**
- Dockerfile for RaftiKV & RustRaft
- Optimize image size

**Tasks**
- Compose file for multi-node deployment

### Weeks 35–36 — Kubernetes for Rust Services
**Study**
- Kubernetes Up & Running: Pods, Deployments, ConfigMaps, Probes

**Exercises**
- Write manifests for Deployment/Service/ConfigMap
- Test scaling & failover

### Weeks 37–38 — Observability, Logging & Monitoring
**Study**
- Prometheus, Grafana, tracing & OpenTelemetry

**Exercises**
- Instrument metrics & distributed tracing
- Grafana dashboards for service metrics

### Weeks 39–40 — CI/CD Pipeline
**Study**
- GitHub Actions, caching, secrets, build matrices

**Exercises**
- Workflow: lint, test, build, push Docker, deploy

---

# Phase 6 — Agents, Performance & Capstone (Weeks 41–52)

**Outcome:** Build a full-scale distributed system showcasing actors, performance tuning, fault tolerance, and deployment.

### Weeks 41–42 — Actor-Based Systems
- Implement agents reacting to events, message handling, fault isolation

### Weeks 43–44 — Multi-Agent Communication
- Extend agents across nodes using NATS or gRPC
- Retry logic, leader election among agents

### Weeks 45–46 — Performance & Profiling
- Benchmark hot paths, profiling with `criterion`, `tokio-console`, flamegraphs
- Optimize locks, allocations, scheduling

### Weeks 47–48 — Fault Tolerance & Snapshots
- Snapshot & log compaction
- Test crash recovery and consistency

### Weeks 49–51 — Capstone: Distributed Event-Sourced Ledger
- Event sourcing + CQRS, append-only storage, REST API, messaging, projections
- Dockerized deployment & optional Kubernetes

### Week 52 — Polish, Document & Showcase
- Clean codebase, finalize tests, write architecture docs & blog post
- Publish on GitHub as a portfolio project

---

# Final Notes
- Commit everything to a public Git repository
- Document architecture as you go
- Automate CI/CD early
- Refactor and review after each phase  
