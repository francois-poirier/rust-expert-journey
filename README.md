# 🦀 Rust Expert Journey

**A 12-month, 1,560-hour transition from Senior Java Engineer to Rust-focused Distributed Systems Engineer.**

[![Rust](https://img.shields.io/badge/language-Rust-orange?logo=rust)](https://www.rust-lang.org/)
[![Duration](https://img.shields.io/badge/duration-12%20months-blue)](#roadmap-overview)
[![Effort](https://img.shields.io/badge/effort-30%20hours%2Fweek-informational)](#study-method)
[![Focus](https://img.shields.io/badge/focus-70%25%20distributed%20systems%20%2F%2030%25%20systems%20programming-success)](#learning-allocation)

## 1. Mission

This repository documents a practical, hands-on journey to become a Rust engineer specializing in **distributed systems and high-performance backend engineering**, supported by a solid foundation in systems programming.

The starting point is an experienced Senior Java Engineer / Software Architect. The goal is to transfer existing software architecture and distributed-systems experience into Rust while developing demonstrable Rust engineering skills.

### Target professional positioning

> **Senior Software Engineer | Rust | Distributed Systems | High-Performance Backend | Systems Programming**

Completing this roadmap does not automatically establish professional Rust experience or guarantee a Senior title. Readiness must be demonstrated through code, tests, design decisions, benchmarks, and the ability to explain technical trade-offs.

## 2. Learning allocation: 70/30

| Track | Allocation | Annual hours | Main outcome |
|---|---:|---:|---|
| Distributed Systems & High-Performance Backend | 70% | 1,092 h | Design, implement, test, and operate the platform |
| Systems Programming & Low-Level Rust | 30% | 468 h | Understand memory, concurrency, networking, Linux, and performance |
| **Total** | **100%** | **1,560 h** | **12-month programme** |

The 70/30 split describes the overall effort. Rust fundamentals are integrated into both tracks.

## 3. Programme at a glance

- **Duration:** 52 weeks (12 months).
- **Weekly commitment:** 30 hours.
- **Daily commitment:** 6 hours, Monday to Friday.
- **Total planned effort:** 1,560 hours.
- **Primary language:** Rust.
- **Primary specialization:** Distributed Systems & Backend.
- **Supporting specialization:** Systems Programming & Low-Level Rust.
- **Project strategy:** one platform, one repository, progressive milestones.

| Phase | Weeks | Hours | Main focus |
|---|---:|---:|---|
| 1. Rust Core & Ownership | 1–8 | 240 | Rust fundamentals and in-memory event engine |
| 2. Advanced Rust & Storage | 9–16 | 240 | Memory, concurrency, persistent storage and recovery |
| 3. Async Rust & Networking | 17–24 | 240 | Tokio, network API and concurrent clients |
| 4. Distributed Systems | 25–36 | 360 | Replication, coordination and failure handling |
| 5. Performance & Systems Programming | 37–44 | 240 | Profiling, memory, Linux I/O and optimization |
| 6. Production Readiness | 45–52 | 240 | Observability, deployment, testing and final release |
| **Total** | **1–52** | **1,560** | **One evolving platform** |

## 4. Roadmap details

All six phases contribute to the same project: **Distributed Event Processing Platform**. Exercises are useful, but the main implementation remains in this project repository.

### Phase 1 --- Rust Core & Ownership

**Weeks 1–8 · 240 hours**

**Goal:** become productive with idiomatic Rust and understand ownership rather than translating Java patterns mechanically.

**Topics:**
- Toolchain, Cargo, crates, modules and workspaces.
- Variables, structs, enums, pattern matching and collections.
- Ownership, moves, borrowing, references, stack and heap.
- Traits, generics, iterators and closures.
- `Option`, `Result` and custom errors.
- Unit and integration testing.

**Project tasks:**
- Define events, identifiers, topics and partitions.
- Implement an in-memory event store.
- Add append and read operations.
- Define module boundaries and error types.
- Add tests and a small CLI to publish and read events.

**Definition of Done:**
- Explain ownership and borrowing decisions.
- Implement append/read operations without unnecessary cloning.
- Organize code into modules and use `Result` / `Option` deliberately.
- Provide meaningful unit and integration tests.

### Phase 2 --- Advanced Rust & Memory

**Weeks 9–16 · 240 hours**

**Goal:** understand advanced Rust abstractions, memory behaviour and safe concurrency.

**Topics:**
- Lifetimes, advanced traits, generics and trait objects.
- `Box`, `Rc`, `Arc`, `Cow`, `Cell` and `RefCell`.
- `Mutex`, `RwLock`, `Send` and `Sync`.
- Atomics and memory ordering fundamentals.
- Memory layout, alignment and allocation.
- File I/O, buffering, serialization and checksums.
- `unsafe` Rust and safety invariants.

**Project tasks:**
- Design the persistent append-only log format.
- Write events to segment files.
- Add a basic offset index and checksums.
- Recover valid records after restart.
- Detect a truncated or corrupted final record.
- Test storage and recovery behaviour.

**Definition of Done:**
- Explain ownership and synchronization choices.
- Explain the storage format and recovery procedure.
- Demonstrate restart recovery with automated tests.
- Justify any `unsafe` block; avoid it when a safe solution exists.

### Phase 3 --- Async Rust & Networking

**Weeks 17–24 · 240 hours**

**Goal:** build reliable asynchronous network services with the Rust ecosystem.

**Topics:**
- Futures, `async` / `await` and Tokio runtime.
- Tasks, channels, synchronization and shared state.
- Cancellation, timeouts and graceful shutdown.
- TCP fundamentals and network framing.
- HTTP APIs with Axum; gRPC with Tonic when justified.
- Backpressure, bounded queues and load testing.

**Project tasks:**
- Expose publish and read operations through an HTTP API.
- Support concurrent producers and consumers.
- Add bounded queues and backpressure.
- Implement timeouts and graceful shutdown.
- Add API and concurrency integration tests.

**Definition of Done:**
- Explain Tokio tasks, scheduling and cancellation.
- Demonstrate bounded resource usage under load.
- Run concurrent producer/consumer tests.
- Report throughput and latency percentiles.

### Phase 4 --- Distributed Systems Engineering

**Weeks 25–36 · 360 hours**

**Goal:** implement and test distributed behaviour, not merely deploy several independent services.

**Topics:**
- Failure models, network partitions, timeouts and retries.
- Ordering, idempotency, duplicate handling and delivery semantics.
- Leader/follower replication and consistency trade-offs.
- Heartbeats, failure detection and cluster membership.
- Raft concepts: elections, log replication and safety.
- Recovery, fault injection and resilience.

**Project tasks:**
- Introduce node identity and cluster configuration.
- Assign partitions to nodes.
- Implement a limited leader/follower replication model.
- Handle node restart and replica catch-up.
- Test timeouts, retries, node loss and network delays.
- Document consistency and delivery guarantees.

**Definition of Done:**
- Run a small multi-node cluster.
- Demonstrate replication and recovery.
- Test selected node and network failures.
- Explain precisely what the platform guarantees and what it does not.

**Scope control:** this is a focused learning platform, not a Kafka clone or production replacement. Do not claim exactly-once delivery, linearizability or production-grade consensus unless the implementation and tests establish those guarantees. Studying Raft does not require implementing the full algorithm.

### Phase 5 --- Performance & Systems Programming

**Weeks 37–44 · 240 hours**

**Goal:** understand performance from application code down to memory, CPU and operating-system interactions.

**Topics:**
- Criterion benchmarks and statistical interpretation.
- CPU, allocation and memory profiling.
- Cache locality, data layout and alignment.
- Zero-copy parsing and buffer management.
- Lock contention and synchronization costs.
- Linux processes, threads, sockets and I/O.
- `epoll`, SIMD fundamentals and justified `unsafe`.

**Project tasks:**
- Establish reproducible workloads and baselines.
- Profile ingestion, storage and consumer paths.
- Measure throughput and P50/P95/P99 latency.
- Reduce avoidable allocations and copies.
- Evaluate batching and data-structure alternatives.
- Record before/after results and preserve correctness tests.

**Definition of Done:**
- Identify bottlenecks using profiling evidence.
- Publish reproducible benchmark results.
- Explain the trade-offs behind each optimization.
- Demonstrate that optimizations preserve correctness.

### Phase 6 --- Production Distributed Rust

**Weeks 45–52 · 240 hours**

**Goal:** deliver a production-oriented release candidate and present it as a professional engineering portfolio project.

**Topics:**
- Structured logging, metrics and distributed tracing.
- OpenTelemetry and health checks.
- Configuration, secrets and security basics.
- Docker and Kubernetes deployment fundamentals.
- CI/CD and automated quality gates.
- Load, soak and fault-injection testing.
- Architecture Decision Records and operational runbooks.

**Project tasks:**
- Add metrics, structured logs and traces.
- Provide reproducible Docker deployment.
- Add a Kubernetes deployment example.
- Automate formatting, linting, tests and builds.
- Run load, restart and fault-injection tests.
- Document architecture, guarantees and known limitations.

**Definition of Done:**
- A reviewer can clone and run the platform using the README.
- CI runs formatting, linting, tests and builds.
- Deployment and observability are demonstrated.
- Test and benchmark results are reproducible.
- Architecture, failure model and limitations are documented.
- A technical walkthrough is ready for interviews.

## 5. Main project: Distributed Event Processing Platform

One project evolves throughout the roadmap. It receives events, validates and stores them, distributes them across partitions, and allows consumers to process them. Later phases add persistence, networking, replication, recovery and observability.

The project takes inspiration from selected event-streaming concepts. **It is not a Kafka clone, Kafka-compatible broker or production Kafka replacement.**

| Phase | Project increment |
|---|---|
| 1 | In-memory event model, topics, partitions, append and read |
| 2 | Persistent append-only log, index and restart recovery |
| 3 | Async network API, concurrent producers/consumers and backpressure |
| 4 | Small multi-node cluster, replication and failure handling |
| 5 | Profiling and measured performance improvements |
| 6 | Observability, deployment, automated tests and release candidate |

### Final project expectations

- Documented event and API contracts.
- Persistence and restart recovery.
- Partition ordering and consumer offset tracking.
- A clearly scoped replication model.
- Bounded queues and backpressure.
- Graceful shutdown and tested failure handling.
- Metrics, logs and traces.
- Repeatable load and fault-injection tests.
- Explicit guarantees and known limitations.

## 6. Study method

A standard 30-hour week is divided as follows:

| Activity | Hours/week |
|---|---:|
| Books and official documentation | 5 |
| Focused exercises and code reading | 5 |
| Project implementation | 12.5 |
| Testing, debugging and profiling | 5 |
| Review and technical documentation | 2.5 |
| **Total** | **30** |

Suggested daily allocation (Monday–Friday):

| Activity | Hours/day |
|---|---:|
| Reading and documentation | 1 |
| Exercises and code reading | 1 |
| Project implementation | 2.5 |
| Testing, debugging and profiling | 1 |
| Review and learning log | 0.5 |
| **Total** | **6** |

Use this as a planning baseline, not a rigid rule. Some weeks will need more implementation or debugging time.

### Weekly review

At the end of each week, record:

1. What was learned and implemented?
2. Which code was written without following a tutorial?
3. Which tests demonstrate the expected behaviour?
4. What technical problem took the most time?
5. Which design decision can be explained and defended?
6. What remains incomplete?
7. What is next week's measurable objective?

## 7. Core resources

Prefer official documentation and primary learning material. Use books at the relevant phase; they do not all need to be read sequentially.

### Rust language and engineering

- [The Rust Programming Language](https://doc.rust-lang.org/book/)
- [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings](https://github.com/rust-lang/rustlings)
- [The Rust Standard Library](https://doc.rust-lang.org/std/)
- [The Cargo Book](https://doc.rust-lang.org/cargo/)
- [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
- [The Rust Reference](https://doc.rust-lang.org/reference/)
- [Rust for Rustaceans](https://rust-for-rustaceans.com/)
- *Programming Rust* — Jim Blandy, Jason Orendorff and Leonora Tindall.
- *Effective Rust* — David Drysdale.

### Async Rust and networking

- [Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/)
- [Tokio Tutorial](https://tokio.rs/tokio/tutorial)
- [Tokio documentation](https://docs.rs/tokio/)
- [Axum documentation](https://docs.rs/axum/)
- [Tonic documentation](https://docs.rs/tonic/)

### Distributed systems

- [MIT Distributed Systems](https://pdos.csail.mit.edu/6.824/)
- [Raft paper](https://raft.github.io/raft.pdf)
- [Raft visualization](https://raft.github.io/)
- *Designing Data-Intensive Applications* — Martin Kleppmann.
- *Database Internals* — Alex Petrov.

### Performance and systems

- *Systems Performance* — Brendan Gregg.
- [Criterion.rs](https://github.com/criterion-rs/criterion.rs)
- [Flamegraph](https://github.com/flamegraph-rs/flamegraph)
- [Rust Performance Book](https://nnethercote.github.io/perf-book/)
- [Linux man-pages](https://man7.org/linux/man-pages/)

## 8. Engineering standards

Apply these standards throughout the repository:

- Format code with `cargo fmt`.
- Run `cargo clippy` and address relevant warnings.
- Use `cargo test` for automated tests.
- Add integration tests for externally observable behaviour.
- Use property-based tests where they add meaningful coverage.
- Use benchmarks for performance claims.
- Avoid unexplained `unwrap()` and `expect()` in production paths.
- Keep `unsafe` minimal and document its safety invariants.
- Document concurrency assumptions and failure behaviour.
- Record significant design decisions as ADRs.
- Never publish secrets or sensitive test data.

Useful baseline commands:

```bash
rustup update
rustc --version
cargo --version

cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace
```

Use workspace-wide commands once the repository is configured as a Cargo workspace.

## 9. Assessment and readiness gates

Progress is measured by evidence, not by hours completed.

| Gate | Expected evidence |
|---|---|
| End of Phase 1 | Idiomatic Rust fundamentals, ownership explanations and tested in-memory event engine |
| End of Phase 2 | Persistent storage, recovery tests and sound memory/concurrency decisions |
| End of Phase 3 | Async API, concurrent clients, cancellation and load-test evidence |
| End of Phase 4 | Multi-node platform, replication tests and documented failure semantics |
| End of Phase 5 | Reproducible benchmarks, profiling evidence and measured optimizations |
| End of Phase 6 | Deployable release candidate, observability, automated tests and technical walkthrough |

### Final senior-level assessment

Before applying for Rust-focused senior roles, verify that you can:

- Solve non-trivial Rust problems without step-by-step tutorials.
- Explain ownership, lifetimes, traits, `Send`, `Sync` and async execution.
- Diagnose concurrency and performance problems.
- Design network protocols and service interfaces.
- Explain consistency, replication and partial failures.
- Build and operate a multi-node Rust system.
- Review unfamiliar Rust code and identify risks.
- Defend technical decisions and explain trade-offs clearly.

The project is evidence of capability, not a substitute for commercial Rust employment history.

## 10. Repository structure

The repository contains one product. Add modules progressively; do not create empty directories for future phases on day one.

```text
rust-expert-journey/
├── README.md
├── Cargo.toml
├── Cargo.lock
├── rust-toolchain.toml
├── src/
│   ├── event/
│   ├── topic/
│   ├── partition/
│   ├── storage/
│   ├── consumer/
│   ├── network/
│   ├── replication/       # introduced in Phase 4
│   └── observability/     # introduced in Phase 6
├── tests/
├── benches/
├── docs/
│   ├── architecture/
│   ├── adr/
│   └── operations/
├── deploy/
│   ├── docker/
│   └── kubernetes/
└── .github/
    └── workflows/
```

## 11. Progress tracker

Update this table as evidence is completed.

| Phase | Status | Evidence / link |
|---|---|---|
| 1. Rust Core & Ownership | Not started | |
| 2. Advanced Rust & Storage | Not started | |
| 3. Async Rust & Networking | Not started | |
| 4. Distributed Systems | Not started | |
| 5. Performance & Systems Programming | Not started | |
| 6. Production Readiness | Not started | |

Suggested status values: `Not started`, `In progress`, `Completed`, `Needs review`.

---

## Guiding principle

**Build, measure, break, recover, and explain.**

The goal is not to collect Rust syntax or finish books. The goal is to demonstrate that you can design, implement, test, optimize and operate reliable distributed software in Rust.
