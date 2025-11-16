# 🦀 26-Week Rust Mastery Roadmap

**Goal:** Distributed, Fault-Tolerant Systems
*Approx. 2 hours/day · 10–12 hours/week · ~6 months*

---

## Table of Contents

* [Phase 1 — Rust Core Mastery (Weeks 1–4)](#phase-1---rust-core-mastery-weeks-1-4)

    * [Week 1 — Ownership, Borrowing, Lifetimes](#week-1---ownership-borrowing-lifetimes)
    * [Week 2 — Enums, Pattern Matching, Error Handling](#week-2---enums-pattern-matching-error-handling)
    * [Week 3 — Traits, Generics, Modules](#week-3---traits-generics-modules)
    * [Week 4 — Testing, Logging, CLI Integration](#week-4---testing-logging-cli-integration)
* [Phase 2 — Async Rust & Web Backends (Weeks 5–8)](#phase-2---async-rust--web-backends-weeks-5-8)

    * [Week 5 — Async/Await & Tokio Runtime](#week-5---asyncawait--tokio-runtime)
    * [Week 6 — HTTP Servers with axum](#week-6---http-servers-with-axum)
    * [Week 7 — Databases & Observability](#week-7---databases--observability)
    * [Week 8 — Testing and Dockerization](#week-8---testing-and-dockerization)
* [Phase 3 — Concurrency & Messaging (Weeks 9–12)](#phase-3---concurrency--messaging-weeks-9-12)

    * [Week 9 — Concurrency Primitives](#week-9---concurrency-primitives)
    * [Week 10 — Async Channels & Streams](#week-10---async-channels--streams)
    * [Week 11 — Actor Model in Rust](#week-11---actor-model-in-rust)
    * [Week 12 — Messaging & Queue Integration](#week-12---messaging--queue-integration)
* [Phase 4 — Distributed Systems (Weeks 13–16)](#phase-4---distributed-systems-weeks-13-16)

    * [Week 13 — Distributed Systems Theory](#week-13---distributed-systems-theory)
    * [Week 14 — Raft Algorithm Deep Dive](#week-14---raft-algorithm-deep-dive)
    * [Week 15 — Build “RaftiDB”](#week-15---build-raftidb)
    * [Week 16 — Fault Injection & Metrics](#week-16---fault-injection--metrics)
* [Phase 5 — Cloud & Containerization (Weeks 17–20)](#phase-5---cloud--containerization-weeks-17-20)

    * [Week 17 — Docker & Kubernetes Basics](#week-17---docker--kubernetes-basics)
    * [Week 18 — AWS & EKS](#week-18---aws--eks)
    * [Week 19 — Observability](#week-19---observability)
    * [Week 20 — CI/CD Automation](#week-20---cicd-automation)
* [Phase 6 — Agents, Performance & Final Project (Weeks 21–26)](#phase-6---agents-performance--final-project-weeks-21-26)

    * [Week 21 — Actor-Based Agents](#week-21---actor-based-agents)
    * [Week 22 — Multi-Agent Communication](#week-22---multi-agent-communication)
    * [Week 23 — Performance & Profiling](#week-23---performance--profiling)
    * [Week 24 — Fault Tolerance & Recovery](#week-24---fault-tolerance--recovery)
    * [Week 25 — Final Project: Autonomous Swarm Simulation](#week-25---final-project-autonomous-swarm-simulation)
    * [Week 26 — Polish & Showcase](#week-26---polish--showcase)

---

## Phase 1 — Rust Core Mastery (Weeks 1–4)

**Outcome:** Write idiomatic, memory-safe Rust confidently.

### Week 1 — Ownership, Borrowing, Lifetimes

**Book references**

* *The Rust Programming Language* (The Book) — Ch. [1](https://doc.rust-lang.org/stable/book/ch01-00-getting-started.html), [2](https://doc.rust-lang.org/stable/book/ch02-00-guessing-game-tutorial.html), [3](https://doc.rust-lang.org/stable/book/ch03-00-common-programming-concepts.html), [4 (Ownership)](https://doc.rust-lang.org/stable/book/ch04-00-understanding-ownership.html).
* *Rust for Rustaceans* — Ch. 1 & 3 (paid).
* Jon Gjengset — *Crust of Rust: Ownership & Borrowing* (video): [https://www.youtube.com/watch?v=8O0Nt9qY_vo](https://www.youtube.com/watch?v=8O0Nt9qY_vo)

**Tasks**

* [ ] Rustlings exercises 1–4 ([https://github.com/rust-lang/rustlings](https://github.com/rust-lang/rustlings))
* [ ] Read *The Book* ch. 1–4 (links above)
* [ ] Watch Gjengset ownership video
* [ ] Mini project: **Config Parser** (use `serde` to parse a `.toml` config → validate → print)

**Deliverable**

* CLI that loads, validates, and prints parsed config (GitHub repo).

---

### Week 2 — Enums, Pattern Matching, Error Handling

**Book references**

* *The Book* — Ch. [6 (Enums & Pattern Matching)](https://doc.rust-lang.org/stable/book/ch06-00-enums.html), [9 (Error Handling)](https://doc.rust-lang.org/stable/book/ch09-00-error-handling.html).
* *Rust for Rustaceans* — Ch. 4 (Error Handling).

**Tasks**

* [ ] Learn `enum`, `match`, `if let`, `while let` patterns
* [ ] Practice `Result` / `Option` patterns, `?` operator
* [ ] Use `thiserror` and `anyhow` for custom errors
* [ ] Extend Config Parser to return rich errors and recovery messages

**Deliverable**

* Error-tolerant CLI with clear error messages and tests.

---

### Week 3 — Traits, Generics, Modules

**Book references**

* *The Book* — Ch. [10 (Generics & Traits)](https://doc.rust-lang.org/stable/book/ch10-00-generics.html), [7 (Modules/Crates)](https://doc.rust-lang.org/stable/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html).
* *Rust for Rustaceans* — Ch. 2 (Types & Traits).

**Tasks**

* [ ] Implement traits, trait bounds, and generic types
* [ ] Refactor projects into modules and crates (library + binary)
* [ ] Project: **Math Toolkit** crate (vectors, basic linear algebra, trait implementations)

**Deliverable**

* Published GitHub repo / crate (or GitHub repo demonstrating crate layout).

---

### Week 4 — Testing, Logging, CLI Integration

**Book references**

* *The Book* — Ch. [11 (Testing)](https://doc.rust-lang.org/stable/book/ch11-00-testing.html).
* *Zero To Production in Rust* — Chapter 2 (testing foundations); code: [https://github.com/LukeMathWalker/zero-to-production](https://github.com/LukeMathWalker/zero-to-production)

**Tasks**

* [ ] Learn `#[test]`, integration tests, and test fixtures
* [ ] Learn logging crates: `log`/`env_logger` and `tracing`
* [ ] Project: **Log Analyzer** — parse logs, aggregate by severity, output CSV; include unit & integration tests

**Deliverable**

* CLI with tests and GitHub Actions for CI.

---

## Phase 2 — Async Rust & Web Backends (Weeks 5–8)

**Outcome:** Build async web APIs with `tokio` + `axum`.

### Week 5 — Async/Await & Tokio Runtime

**Book references**

* *Asynchronous Programming in Rust* (Async Book) — Ch. [1](https://rust-lang.github.io/async-book/01_getting_started/01_chapter.html) → [4](https://rust-lang.github.io/async-book/04_channels/01_chapter.html).
* Tokio docs: [https://docs.rs/tokio/latest/tokio/](https://docs.rs/tokio/latest/tokio/)

**Tasks**

* [ ] Read Async Book ch. 1–4
* [ ] Practice spawning tasks, `async`/`await`, `join!`, `select!`
* [ ] Project: **Concurrent Downloader** (download many URLs concurrently, save results)

**Deliverable**

* Async CLI using `tokio` that runs concurrent downloads with retries.

---

### Week 6 — HTTP Servers with axum

**Book references**

* *Zero To Production in Rust* — Ch. 1–3 (patterns for web apps) — repo: [https://github.com/LukeMathWalker/zero-to-production](https://github.com/LukeMathWalker/zero-to-production)
* Axum docs: [https://docs.rs/axum/latest/axum/](https://docs.rs/axum/latest/axum/)

**Tasks**

* [ ] Build **TaskHub** REST API (`GET/POST/PUT/DELETE`) using `axum` and `serde`
* [ ] Add request validation and error handling

**Deliverable**

* Working async API server (local run + tests).

---

### Week 7 — Databases & Observability

**Book references**

* SQLx docs: [https://docs.rs/sqlx/latest/sqlx/](https://docs.rs/sqlx/latest/sqlx/)
* `dotenvy`: [https://docs.rs/dotenvy/latest/dotenvy/](https://docs.rs/dotenvy/latest/dotenvy/)
* `tracing`: [https://docs.rs/tracing/latest/tracing/](https://docs.rs/tracing/latest/tracing/)
* OpenTelemetry for Rust: [https://opentelemetry.io/docs/languages/rust/](https://opentelemetry.io/docs/languages/rust/)

**Tasks**

* [ ] Add Postgres persistence via `sqlx`
* [ ] Manage config via `dotenvy` / environment variables
* [ ] Add structured logging/traces with `tracing` and export via OpenTelemetry

**Deliverable**

* API with DB persistence and observability (logs + traces).

---

### Week 8 — Testing and Dockerization

**Book references**

* `reqwest` (HTTP client for tests): [https://docs.rs/reqwest/latest/reqwest/](https://docs.rs/reqwest/latest/reqwest/)
* Docker docs: [https://docs.docker.com/](https://docs.docker.com/)
* Zero To Production deployment/testing patterns: [https://github.com/LukeMathWalker/zero-to-production](https://github.com/LukeMathWalker/zero-to-production)

**Tasks**

* [ ] Write integration tests using `reqwest` + test DB (e.g., ephemeral Postgres)
* [ ] Create a multi-stage Dockerfile and compose file (Postgres + API)

**Deliverable**

* Dockerized Rust microservice with integration tests.

---

## Phase 3 — Concurrency & Messaging (Weeks 9–12)

**Outcome:** Master threads, async streams, actors, messaging.

### Week 9 — Concurrency Primitives

**Book references**

* *The Book* — Ch. [16 (Fearless Concurrency)](https://doc.rust-lang.org/stable/book/ch16-00-concurrency.html).
* *Rust for Rustaceans* — Ch. 12 (Concurrency).

**Tasks**

* [ ] Practice `Arc`, `Mutex`, `RwLock`, atomics, `std::thread`
* [ ] Compare multi-threaded vs async approaches; small benchmarks

**Deliverable**

* Parallel word-count demo with benchmarks.

---

### Week 10 — Async Channels & Streams

**Book references**

* Async Book — Ch. [6 (Streams)](https://rust-lang.github.io/async-book/06_streams/01_chapter.html).
* `tokio::sync::mpsc`: [https://docs.rs/tokio/latest/tokio/sync/mpsc/](https://docs.rs/tokio/latest/tokio/sync/mpsc/)

**Tasks**

* [ ] Use channels and async streams to pipeline work
* [ ] Project: **Concurrent Crawler** — fetch pages concurrently, stream results downstream

**Deliverable**

* Async crawler with graceful shutdown and backpressure.

---

### Week 11 — Actor Model in Rust

**Book references / Docs**

* `xtra`: [https://docs.rs/xtra/latest/xtra/](https://docs.rs/xtra/latest/xtra/)
* `ractor`: [https://docs.rs/ractor/latest/ractor/](https://docs.rs/ractor/latest/ractor/)
* `actix`: [https://actix.rs/](https://actix.rs/)

**Tasks**

* [ ] Study an actor framework (pick one)
* [ ] Convert crawler workers to actor-based workers

**Deliverable**

* Actor-based crawler demonstrating message passing and supervision.

---

### Week 12 — Messaging & Queue Integration

**Book references**

* `rdkafka` (Kafka): [https://docs.rs/rdkafka/latest/rdkafka/](https://docs.rs/rdkafka/latest/rdkafka/)
* `nats.rs`: [https://docs.rs/nats/latest/nats/](https://docs.rs/nats/latest/nats/)

**Tasks**

* [ ] Integrate Kafka or NATS for task distribution
* [ ] Implement producer/consumer and simple retry semantics

**Deliverable**

* Multi-node async pipeline demo using a message broker.

---

## Phase 4 — Distributed Systems (Weeks 13–16)

**Outcome:** Implement consensus, replication, and fault tolerance.

### Week 13 — Distributed Systems Theory

**Book references**

* *Designing Data-Intensive Applications* — Chapters 5–7 (Replication, Partitioning, Transactions). Summaries:

    * Replication: [https://martin.kleppmann.com/2015/05/25/ddia-replication.html](https://martin.kleppmann.com/2015/05/25/ddia-replication.html)
    * Transactions: [https://martin.kleppmann.com/2017/01/18/ddia-transactions.html](https://martin.kleppmann.com/2017/01/18/ddia-transactions.html)

**Tasks**

* [ ] Study CAP, consistency models, replication strategies
* [ ] Write short comparative notes: CP vs AP systems

**Deliverable**

* Written summary comparing consistency tradeoffs.

---

### Week 14 — Raft Algorithm Deep Dive

**Book references**

* Raft paper (full PDF): [https://raft.github.io/raft.pdf](https://raft.github.io/raft.pdf)
* `async-raft` crate: [https://docs.rs/async-raft/latest/async_raft/](https://docs.rs/async-raft/latest/async_raft/)

**Tasks**

* [ ] Read the Raft paper (leader election, log replication, safety)
* [ ] Explore `async-raft` examples
* [ ] Build an in-memory 3-node Raft cluster (toy)

**Deliverable**

* Working leader election + log replication demo.

---

### Week 15 — Build “RaftiDB”

**References**

* Storage crates: `sled` ([https://docs.rs/sled/latest/sled/](https://docs.rs/sled/latest/sled/)), `rocksdb` ([https://docs.rs/rocksdb/latest/rocksdb/](https://docs.rs/rocksdb/latest/rocksdb/))
* `axum` for REST: [https://docs.rs/axum/latest/axum/](https://docs.rs/axum/latest/axum/)

**Tasks**

* [ ] Implement a distributed key-value store using Raft for consensus
* [ ] Expose REST API for reads/writes
* [ ] Persist logs/snapshots to `sled` or `rocksdb`

**Deliverable**

* Fault-tolerant KV store (basic CRUD + replication).

---

### Week 16 — Fault Injection & Metrics

**Book references**

* Prometheus client: [https://docs.rs/prometheus/latest/prometheus/](https://docs.rs/prometheus/latest/prometheus/)
* OpenTelemetry: [https://opentelemetry.io/docs/languages/rust/](https://opentelemetry.io/docs/languages/rust/)

**Tasks**

* [ ] Add chaos experiments (kill nodes, partition, restart)
* [ ] Expose Prometheus metrics and traces

**Deliverable**

* Resilient RaftiDB cluster with observability dashboards.

---

## Phase 5 — Cloud & Containerization (Weeks 17–20)

**Outcome:** Deploy and monitor Rust systems in the cloud.

### Week 17 — Docker & Kubernetes Basics

**References**

* Docker: [https://docs.docker.com/](https://docs.docker.com/)
* Kubernetes concepts: [https://kubernetes.io/docs/concepts/](https://kubernetes.io/docs/concepts/)
* `kubectl` ref: [https://kubernetes.io/docs/reference/kubectl/](https://kubernetes.io/docs/reference/kubectl/)

**Tasks**

* [ ] Build optimized multi-stage Docker images for Rust services
* [ ] Deploy service to Minikube (Deployment + Service)

**Deliverable**

* TaskHub running on local K8s.

---

### Week 18 — AWS & EKS

**References**

* AWS EKS: [https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
* Terraform AWS provider: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

**Tasks**

* [ ] Learn basic EKS setup and Terraform for infra as code
* [ ] Deploy RaftiDB to EKS (LoadBalancer or Ingress)

**Deliverable**

* Publicly accessible cluster (or EKS sandbox).

---

### Week 19 — Observability

**References**

* Grafana: [https://grafana.com/docs/](https://grafana.com/docs/)
* Jaeger: [https://www.jaegertracing.io/docs/latest/](https://www.jaegertracing.io/docs/latest/)
* Prometheus / OpenTelemetry links above

**Tasks**

* [ ] Configure Prometheus + Grafana dashboards
* [ ] Add distributed tracing with Jaeger/OpenTelemetry

**Deliverable**

* Dashboards + tracing for RaftiDB.

---

### Week 20 — CI/CD Automation

**References**

* GitHub Actions: [https://docs.github.com/en/actions](https://docs.github.com/en/actions)
* Helm: [https://helm.sh/docs/](https://helm.sh/docs/)

**Tasks**

* [ ] Build CI pipeline: lint, test, build, publish image
* [ ] Automate Helm deployments to cluster

**Deliverable**

* Full CI/CD pipeline for build → deploy.

---

## Phase 6 — Agents, Performance & Final Project (Weeks 21–26)

**Outcome:** Demonstrate mastery via advanced distributed Rust system.

### Week 21 — Actor-Based Agents

**References**

* `xtra`: [https://docs.rs/xtra/latest/xtra/](https://docs.rs/xtra/latest/xtra/)
* `actix`: [https://actix.rs/](https://actix.rs/)

**Tasks**

* [ ] Build single-node reactive agent simulation (actors respond to environment events)

**Deliverable**

* Agent demo that responds to stimuli and logs state.

---

### Week 22 — Multi-Agent Communication

**References**

* gRPC/tonic: [https://docs.rs/tonic/latest/tonic/](https://docs.rs/tonic/latest/tonic/)
* NATS: [https://docs.rs/nats/latest/nats/](https://docs.rs/nats/latest/nats/)

**Tasks**

* [ ] Connect agents across nodes via NATS or gRPC
* [ ] Implement discovery, retries, and simple consensus for task allocation

**Deliverable**

* Multi-agent network demonstrating resilience.

---

### Week 23 — Performance & Profiling

**References**

* Criterion: [https://docs.rs/criterion/latest/criterion/](https://docs.rs/criterion/latest/criterion/)
* Flamegraph: [https://github.com/flamegraph-rs/flamegraph](https://github.com/flamegraph-rs/flamegraph)
* Tokio Console: [https://docs.rs/console-subscriber/latest/console_subscriber/](https://docs.rs/console-subscriber/latest/console_subscriber/)

**Tasks**

* [ ] Benchmark critical paths (Criterion)
* [ ] Generate flamegraphs and use tokio-console for async traces
* [ ] Optimize locks and memory layout

**Deliverable**

* Benchmark report + flamegraph artifacts.

---

### Week 24 — Fault Tolerance & Recovery

**Tasks**

* [ ] Add snapshotting of state, leader recovery logic, exponential backoff
* [ ] Harden restart and rejoin flows

**Deliverable**

* RaftiDB v2 with snapshots & improved recovery.

---

### Week 25 — Final Project: Distributed Event-Sourced Ledger in Rust

**Project Overview:**
Build a complete, production-style Rust system implementing:

* Event sourcing
* CQRS (Command Query Responsibility Segregation)
* Async microservices
* Persistence (SQLite, PostgreSQL, or RocksDB)
* Message-based communication (NATS or Kafka)
* REST API & gRPC
* Background workers
* Unit, integration, and performance tests

*Prepares you for Rust backend roles, Substrate development, and distributed systems.*

---

**Tasks**

* [ ] **Design Domain & Event Model**

    * Define commands: `CreateAccount`, `CreditAccount`, `DebitAccount`, `TransferFunds`
    * Define events: `AccountOpened`, `FundsCredited`, `FundsDebited`, `TransferCompleted`
    * Plan read and write models (CQRS)

* [ ] **Implement Command Service (API Layer)**

    * Accept commands via REST API (`axum` or `actix-web`)
    * Validate domain rules
    * Emit domain events to event store or message broker

* [ ] **Build Event Store**

    * Append-only storage (SQLite, RocksDB, or PostgreSQL)
    * Store all events with metadata (timestamp, event type, aggregate ID)
    * Implement basic snapshotting (optional)

* [ ] **Develop Projection Service**

    * Listen to events via NATS/Kafka
    * Build read models: account balances, transaction history
    * Ensure eventual consistency

* [ ] **Query Service (Read API)**

    * Provide REST/gRPC endpoints for queries: balance, transaction history
    * Use read models from projection service

* [ ] **Async Background Workers**

    * Apply event processing, retries, and background reconciliation

* [ ] **Testing**

    * Unit tests for commands, events, and projections
    * Integration tests for API + event store
    * Performance/load tests (optional: `criterion` or `k6`)

* [ ] **Optional Frontend / Dashboard**

    * Web UI (Yew or Leptos) to visualize accounts and transactions
    * Interactive test of event sourcing and projections

* [ ] **Containerization**

    * Docker Compose file to run services locally
    * Each service isolated (Command API, Query API, Event Store, Message Broker)

---

**Deliverable**

* Fully working distributed ledger system:

    * Command API accepts commands
    * Event Store persists all events
    * Projection service updates read models
    * Query API serves current balances and transaction history
* Optional: Interactive frontend dashboard showing account updates in real time
* Complete **README** including:

    * Architecture overview
    * Event model & CQRS explanation
    * How to run locally (`docker-compose up`)
    * Example API commands & queries
    * Testing & benchmarking instructions
* Public **GitHub repository** demonstrating production-quality Rust, async microservices, and distributed system patterns


---

### Week 26 — Polish & Showcase

**Tasks**

* [ ] Refactor code, improve tests & docs
* [ ] Write a technical blog post describing architecture and lessons learned
* [ ] Update LinkedIn and GitHub portfolio with project highlights

**Deliverable**

* Public portfolio that demonstrates Rust distributed systems expertise.

---

## Recommended References (quick links)

* **The Rust Programming Language (The Book)** — [https://doc.rust-lang.org/stable/book/](https://doc.rust-lang.org/stable/book/)
* **Rust Async Book** — [https://rust-lang.github.io/async-book/](https://rust-lang.github.io/async-book/)
* **Zero To Production In Rust (code & examples)** — [https://github.com/LukeMathWalker/zero-to-production](https://github.com/LukeMathWalker/zero-to-production)
* **Designing Data-Intensive Applications (summaries)** — [https://martin.kleppmann.com/](https://martin.kleppmann.com/)
* **Raft Paper** — [https://raft.github.io/raft.pdf](https://raft.github.io/raft.pdf)
* **Tokio** — [https://docs.rs/tokio/latest/tokio/](https://docs.rs/tokio/latest/tokio/)
* **axum** — [https://docs.rs/axum/latest/axum/](https://docs.rs/axum/latest/axum/)
* **sqlx** — [https://docs.rs/sqlx/latest/sqlx/](https://docs.rs/sqlx/latest/sqlx/)
* **sled** — [https://docs.rs/sled/latest/sled/](https://docs.rs/sled/latest/sled/)
* **rocksdb** — [https://docs.rs/rocksdb/latest/rocksdb/](https://docs.rs/rocksdb/latest/rocksdb/)
* **Prometheus (Rust client)** — [https://docs.rs/prometheus/latest/prometheus/](https://docs.rs/prometheus/latest/prometheus/)
* **OpenTelemetry (Rust)** — [https://opentelemetry.io/docs/languages/rust/](https://opentelemetry.io/docs/languages/rust/)
* **Criterion** — [https://docs.rs/criterion/latest/criterion/](https://docs.rs/criterion/latest/criterion/)
* **Flamegraph** — [https://github.com/flamegraph-rs/flamegraph](https://github.com/flamegraph-rs/flamegraph)
