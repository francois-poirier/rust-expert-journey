# 🦀 Rust Expert Journey

**A 12-month, 1,560-hour transition from Senior Java Engineer to
Rust-focused Distributed Systems Engineer.**

[![Rust](https://img.shields.io/badge/language-Rust-orange?logo=rust)](https://www.rust-lang.org/)
[![Duration](https://img.shields.io/badge/duration-12%20months-blue)](#roadmap-overview)
[![Effort](https://img.shields.io/badge/effort-30%20hours%2Fweek-informational)](#study-method)
[![Focus](https://img.shields.io/badge/focus-70%25%20distributed%20systems%20%2F%2030%25%20systems%20programming-success)](#learning-allocation)

## 1. Mission

This repository documents a structured, hands-on journey to become a
production-capable Rust engineer, specializing in **distributed systems
and high-performance backend engineering**, supported by a solid
foundation in systems programming.

The starting point is an experienced Senior Java Engineer and Software
Architect with extensive experience in distributed systems. The
objective is not to restart an engineering career, but to transfer and
deepen that experience using Rust.

### Target professional positioning

> **Senior Software Engineer \| Rust \| Distributed Systems \|
> High-Performance Backend \| Systems Programming**

The roadmap is designed to build demonstrable skills and a portfolio.
Completing the roadmap does not automatically establish professional
Rust experience or guarantee a Senior title; readiness will be assessed
through code, project evidence, technical explanations, and
interview-style evaluations.

## 2. Learning allocation: 70/30

The learning effort is deliberately weighted toward the target
specialization.

  ----------------------------------------------------------------------------
  Track                        Allocation         Annual hours Main outcome
  ------------------ -------------------- -------------------- ---------------
  Distributed                         70%              1,092 h Design,
  Systems &                                                    implement,
  High-Performance                                             test, and
  Backend                                                      operate
                                                               distributed
                                                               Rust services

  Systems                             30%                468 h Understand
  Programming &                                                memory,
  Low-Level Rust                                               concurrency,
                                                               networking,
                                                               Linux, and
                                                               performance at
                                                               a lower level

  **Total**                      **100%**          **1,560 h** **12-month
                                                               intensive
                                                               learning
                                                               programme**
  ----------------------------------------------------------------------------

The 70/30 split describes the overall allocation. Rust language
fundamentals are prerequisites and are integrated into both tracks.

## 3. Programme at a glance

-   **Duration:** 52 weeks (12 months).
-   **Weekly commitment:** 30 hours.
-   **Daily commitment:** 6 hours, Monday to Friday.
-   **Total planned effort:** 1,560 hours.
-   **Primary language:** Rust.
-   **Primary specialization:** distributed systems and high-performance
    backend.
-   **Secondary specialization:** systems programming and low-level
    performance.

  --------------------------------------------------------------------------
  Phase                          Weeks                Hours Main focus
  --------------- -------------------- -------------------- ----------------
  1\. Rust Core &                 1--8                  240 Language
  Ownership                                                 fundamentals,
                                                            ownership,
                                                            borrowing, APIs

  2\. Advanced                   9--16                  240 Lifetimes,
  Rust & Memory                                             traits, memory
                                                            model,
                                                            concurrency
                                                            primitives,
                                                            unsafe

  3\. Async Rust                17--24                  240 Tokio, async,
  & Networking                                              networking,
                                                            HTTP, gRPC,
                                                            persistence

  4\. Distributed               25--36                  360 Replication,
  Systems                                                   consistency,
  Engineering                                               consensus,
                                                            messaging,
                                                            resilience

  5\. Performance               37--44                  240 Profiling,
  & Systems                                                 memory,
  Programming                                               zero-copy, Linux
                                                            I/O,
                                                            optimization

  6\. Production                45--52                  240 Production
  Distributed                                               architecture,
  Rust                                                      observability,
                                                            deployment,
                                                            capstone

  **Total**                  **1--52**            **1,560** 
  --------------------------------------------------------------------------

## 4. Roadmap details

### Phase 1 --- Rust Core & Ownership

**Weeks 1--8 · 240 hours**

**Goal:** become productive with idiomatic Rust and understand the
ownership model rather than translating Java patterns mechanically.

Topics:

-   Toolchain, Cargo, crates, modules, workspaces.
-   Variables, mutability, structs, enums, pattern matching.
-   Ownership, moves, borrowing, references.
-   Stack and heap fundamentals.
-   Collections, iterators, closures.
-   Traits, generics, and basic type-driven design.
-   `Option`, `Result`, error propagation, custom errors.
-   Unit and integration testing.
-   Introduction to threads and memory safety.

Projects and exercises:

-   `rust-cli-toolkit`: a CLI file scanner and log analyser.
-   Initial `in-memory-store`: a simple in-memory key/value library.

Definition of Done:

-   Explain moves, borrowing, and ownership in code reviews.
-   Design APIs that avoid unnecessary cloning.
-   Use `Result` and `Option` deliberately.
-   Organize code into modules and crates.
-   Add meaningful unit and integration tests.
-   Keep compiler and Clippy warnings under control.

### Phase 2 --- Advanced Rust & Memory

**Weeks 9--16 · 240 hours**

**Goal:** understand advanced Rust abstractions, memory behaviour, and
the foundations of safe concurrency.

Topics:

-   Advanced traits, generics, associated types, trait objects.
-   Lifetimes, lifetime elision, and API lifetime design.
-   Smart pointers: `Box`, `Rc`, `Arc`, `Cow`.
-   Interior mutability: `Cell`, `RefCell`, `Mutex`, `RwLock`.
-   `Send` and `Sync`.
-   Atomics and memory ordering fundamentals.
-   Memory layout, alignment, stack and heap.
-   Pinning and self-referential type challenges.
-   `unsafe` Rust: invariants, safety contracts, and auditing.
-   FFI fundamentals.
-   Allocation and memory profiling.

Projects and exercises:

-   Extend `in-memory-store` with concurrent access patterns.
-   Implement selected data structures and buffer abstractions.
-   Document memory layout and ownership decisions.

Definition of Done:

-   Explain when to use borrowing, `Arc`, locks, and interior
    mutability.
-   Explain the role of `Send` and `Sync`.
-   Identify data races and synchronization hazards.
-   Justify every `unsafe` block with explicit invariants.
-   Measure allocations and explain relevant memory trade-offs.

### Phase 3 --- Async Rust & Networking

**Weeks 17--24 · 240 hours**

**Goal:** build reliable asynchronous network services using the Rust
ecosystem.

Topics:

-   Futures, `async`/`await`, executors, and runtime concepts.
-   Tokio tasks, scheduling, channels, and synchronization.
-   `Send` futures, pinning, streams, and async traits.
-   Cancellation safety, timeouts, graceful shutdown.
-   TCP and UDP fundamentals.
-   HTTP services with Axum.
-   gRPC services with Tonic and Protocol Buffers.
-   PostgreSQL access with SQLx.
-   Connection pools and transaction boundaries.
-   Backpressure, bounded queues, retries, and circuit breakers.
-   Structured logging, metrics, and tracing foundations.

Projects:

-   `async-task-engine`: concurrent task processing engine.
-   `rust-http-service`: REST and gRPC service backed by PostgreSQL.

Definition of Done:

-   Explain how the chosen Tokio runtime and task model work.
-   Avoid blocking operations on async executor threads.
-   Handle cancellation, timeouts, and shutdown deliberately.
-   Implement REST and gRPC endpoints with integration tests.
-   Run load tests and report throughput and latency percentiles.
-   Explain the behaviour of bounded queues and backpressure.

### Phase 4 --- Distributed Systems Engineering

**Weeks 25--36 · 360 hours**

**Goal:** design and implement distributed behaviour, not merely deploy
several independent services.

Topics:

**Communication and messaging** - RPC, service-to-service communication,
and protocol design. - Message brokers and event-driven architectures. -
Delivery semantics: at-most-once, at-least-once, and practical
limitations. - Ordering, idempotency, deduplication, and consumer
groups. - Backpressure and flow control.

**Consistency and replication** - Consistency models and their
trade-offs. - Linearizability and eventual consistency. -
Leader/follower replication. - Quorums and read/write consistency. - Log
replication and snapshot fundamentals. - Partitioning and sharding.

**Coordination and consensus** - Leader election and failure
detection. - Distributed locks and leases: limitations and fencing. -
Raft concepts: terms, elections, log replication, safety. - Network
partitions, split brain, and quorum loss.

**Reliability** - Partial failures and failure models. - Retries,
timeouts, circuit breakers, and idempotency. - Recovery after process or
node restart. - Fault injection and network impairment. - Graceful
degradation and operational failure handling.

Projects:

-   Build distributed-systems laboratories for replication, leader
    election, consistency, and failure scenarios.
-   Evolve `rust-event-backend` into a multi-node event ingestion and
    processing system.

Definition of Done:

-   Run a cluster of independent Rust nodes.
-   Demonstrate leader election or a clearly documented coordination
    approach.
-   Implement and test replication behaviour.
-   Document consistency and delivery guarantees precisely.
-   Demonstrate recovery after node restart.
-   Test network partitions and partial failures.
-   Explain what the system guarantees and what it does not guarantee.

### Phase 5 --- Performance & Systems Programming

**Weeks 37--44 · 240 hours**

**Goal:** understand performance from application code down to memory,
CPU, and operating-system interactions.

Topics:

-   Benchmark design and statistical interpretation.
-   Criterion benchmarks.
-   CPU profiling and flamegraphs.
-   Allocation profiling and memory usage.
-   Cache locality, data layout, and alignment.
-   Zero-copy parsing and buffer management.
-   SIMD fundamentals and when it is useful.
-   Lock contention and low-contention designs.
-   Lock-free concepts and their trade-offs.
-   Linux processes, threads, sockets, and I/O fundamentals.
-   `epoll` and readiness-based I/O concepts.
-   Safe optimization and carefully justified `unsafe`.

Project:

-   `low-latency-core`: reusable high-performance data structures,
    buffers, and processing primitives.

Definition of Done:

-   Establish a reproducible baseline before optimizing.
-   Report throughput and P50/P95/P99 latency where relevant.
-   Measure allocation behaviour.
-   Use profiling evidence to identify bottlenecks.
-   Compare alternative implementations fairly.
-   Explain optimization trade-offs and any unsafe invariants.

### Phase 6 --- Production Distributed Rust

**Weeks 45--52 · 240 hours**

**Goal:** deliver a production-oriented distributed Rust system and
present it as a professional engineering portfolio project.

Topics:

-   Production architecture and service boundaries.
-   Configuration, secrets, authentication, and authorization.
-   OpenTelemetry: traces, metrics, and logs.
-   Health checks and operational diagnostics.
-   Docker and container image optimization.
-   Kubernetes deployment fundamentals.
-   CI/CD and automated quality gates.
-   Integration, load, soak, and fault-injection testing.
-   Performance regression checks.
-   Architecture Decision Records (ADRs).
-   Technical documentation and operational runbooks.

Capstone:

Complete and harden `rust-event-backend` as a multi-node event ingestion
and processing platform.

Definition of Done:

-   Reproducible local environment and deployment instructions.
-   Documented architecture and important ADRs.
-   REST or gRPC interfaces documented.
-   Persistence and recovery behaviour demonstrated.
-   Metrics, logs, and distributed traces available.
-   Automated unit, integration, and failure tests.
-   Load-test methodology and results published.
-   Known limitations and consistency guarantees documented.
-   A technical walkthrough suitable for a senior engineering interview.

## 5. Portfolio projects

The projects are intended to build progressively toward the capstone.
Avoid treating them as unrelated tutorial exercises.

  -----------------------------------------------------------------------
  Project                 Purpose                 Main technologies
  ----------------------- ----------------------- -----------------------
  `rust-cli-toolkit`      CLI file scanner and    Rust, Cargo, Clap,
                          log analyser            testing

  `in-memory-store`       In-memory key/value     Ownership, lifetimes,
                          engine                  collections,
                                                  synchronization

  `rust-http-service`     Backend API service     Axum, Tonic, SQLx,
                                                  PostgreSQL

  `async-task-engine`     Concurrent task         Tokio, channels,
                          processing              cancellation,
                                                  backpressure

  `low-latency-core`      Performance-focused     Criterion, profiling,
                          library                 buffers, memory layout

  `rust-event-backend`    Distributed event       Tokio, networking,
                          ingestion and           persistence,
                          processing platform     replication,
                                                  observability
  -----------------------------------------------------------------------

### Capstone expectations

The final system should demonstrate engineering depth in the following
areas:

-   Multi-node execution.
-   Explicit message and protocol contracts.
-   Persistence and restart recovery.
-   Replication or a clearly scoped consistency model.
-   Idempotent event processing.
-   Backpressure and bounded resource usage.
-   Graceful shutdown and failure handling.
-   Observability across service boundaries.
-   Repeatable load and fault-injection tests.

Do not claim exactly-once delivery or linearizability unless the
implementation and tests establish the relevant guarantees.

## 6. Study method

A standard 30-hour week is divided as follows:

  Activity                                       Hours/week
  -------------------------------------------- ------------
  Books and official documentation                        5
  Focused exercises and code reading                      5
  Project implementation                               12.5
  Testing, debugging, and profiling                       5
  Review, notes, and technical documentation            2.5
  **Total**                                          **30**

Suggested daily allocation (Monday--Friday):

  Activity                              Hours/day
  ----------------------------------- -----------
  Reading and documentation                     1
  Exercises and code reading                    1
  Project implementation                      2.5
  Testing, debugging, and profiling             1
  Review and learning log                     0.5
  **Total**                                 **6**

Use this allocation as a planning baseline, not a rigid rule. Some weeks
will need more implementation or debugging time.

### Weekly review

At the end of each week, record:

1.  What was learned and implemented?
2.  Which code was written without following a tutorial?
3.  Which tests demonstrate the expected behaviour?
4.  What technical problem took the most time?
5.  Which design decision can be explained and defended?
6.  What remains incomplete?
7.  What is the next week's measurable objective?

## 7. Core resources

Prefer official documentation and primary learning material. Check the
edition and publication details before purchasing books.

### Rust language and engineering

-   [The Rust Programming Language](https://doc.rust-lang.org/book/)
-   [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
-   [Rustlings](https://github.com/rust-lang/rustlings)
-   [The Rust Standard Library](https://doc.rust-lang.org/std/)
-   [The Cargo Book](https://doc.rust-lang.org/cargo/)
-   [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
-   [The Rust Reference](https://doc.rust-lang.org/reference/)
-   [Rust for Rustaceans](https://rust-for-rustaceans.com/)
-   *Programming Rust* --- Jim Blandy, Jason Orendorff, and Leonora
    Tindall.
-   *Effective Rust* --- David Drysdale.

### Async Rust and networking

-   [Asynchronous Programming in
    Rust](https://rust-lang.github.io/async-book/)
-   [Tokio Tutorial](https://tokio.rs/tokio/tutorial)
-   [Tokio documentation](https://docs.rs/tokio/)
-   [Axum documentation](https://docs.rs/axum/)
-   [Tonic documentation](https://docs.rs/tonic/)
-   [SQLx documentation](https://docs.rs/sqlx/)

### Distributed systems

-   [MIT 6.5840 Distributed Systems](https://pdos.csail.mit.edu/6.824/)
-   [Raft paper](https://raft.github.io/raft.pdf)
-   [Raft visualization](https://raft.github.io/)
-   *Designing Data-Intensive Applications* --- Martin Kleppmann.
-   *Database Internals* --- Alex Petrov.

### Performance and systems

-   *Systems Performance* --- Brendan Gregg.
-   [Criterion.rs](https://github.com/criterion-rs/criterion.rs)
-   [Flamegraph](https://github.com/flamegraph-rs/flamegraph)
-   [Rust Performance Book](https://nnethercote.github.io/perf-book/)
-   [Linux man-pages](https://man7.org/linux/man-pages/)

The books are references to be used at the relevant phase, not a
requirement to read every page sequentially. Documentation and exercises
should be tied to the code being built.

## 8. Engineering standards

Apply these standards across the repository:

-   Format code with `cargo fmt`.
-   Run `cargo clippy` and address relevant warnings.
-   Use `cargo test` for automated tests.
-   Add integration tests for externally observable behaviour.
-   Use property-based testing where it provides meaningful coverage.
-   Use benchmarks for performance claims.
-   Avoid unexplained `unwrap()` and `expect()` in production paths.
-   Keep `unsafe` isolated, minimal, and documented with safety
    invariants.
-   Document concurrency assumptions and failure behaviour.
-   Record significant design decisions as ADRs.
-   Pin toolchain and dependency versions when reproducibility requires
    it.
-   Never publish secrets, credentials, or sensitive test data.

Useful baseline commands:

``` bash
rustup update
rustc --version
cargo --version

cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace
```

Add workspace-wide commands only when the repository has been configured
as a Cargo workspace and all members support the selected features.

## 9. Assessment and readiness gates

Progress is measured by evidence, not by hours completed.

  -----------------------------------------------------------------------
  Gate                                Expected evidence
  ----------------------------------- -----------------------------------
  End of Phase 1                      Idiomatic Rust fundamentals,
                                      ownership explanations, tested CLI
                                      and initial store

  End of Phase 2                      Advanced type and memory design,
                                      concurrency primitives, justified
                                      unsafe where needed

  End of Phase 3                      Async service, REST/gRPC
                                      integration, cancellation and
                                      load-test evidence

  End of Phase 4                      Multi-node system,
                                      replication/coordination
                                      experiments, documented failure
                                      semantics

  End of Phase 5                      Reproducible benchmarks, profiling
                                      evidence, measured optimizations

  End of Phase 6                      Production-oriented capstone,
                                      deployment, observability, testing,
                                      and technical defence
  -----------------------------------------------------------------------

### Final senior-level assessment

Before applying for Rust-focused senior roles, verify that you can:

-   Solve non-trivial Rust problems without relying on step-by-step
    tutorials.
-   Explain ownership, lifetimes, traits, `Send`, `Sync`, and async
    execution.
-   Diagnose concurrency and performance problems.
-   Design network protocols and service interfaces.
-   Explain consistency, replication, consensus, and partial failures.
-   Build and operate a multi-node Rust system.
-   Review unfamiliar Rust code and identify risks.
-   Defend technical decisions and explain trade-offs clearly.

The target is to become a credible candidate for roles where
distributed-systems experience is valued alongside demonstrable Rust
proficiency. Some employers will still require several years of
commercial Rust experience; this portfolio is evidence of capability,
not a substitute for employment history.

## 10. Repository structure

Suggested repository layout:

``` text
rust-expert-journey/
├── README.md
├── Cargo.toml
├── rust-toolchain.toml
│
├── fundamentals/
│   ├── ownership/
│   ├── lifetimes/
│   ├── traits/
│   ├── memory-layout/
│   └── unsafe-rust/
│
├── projects/
│   ├── rust-cli-toolkit/
│   ├── in-memory-store/
│   ├── rust-http-service/
│   ├── async-task-engine/
│   ├── low-latency-core/
│   └── rust-event-backend/
│
├── distributed-systems/
│   ├── replication/
│   ├── consensus/
│   ├── leader-election/
│   ├── partitioning/
│   ├── consistency/
│   └── fault-tolerance/
│
├── performance/
│   ├── benchmarks/
│   ├── profiling/
│   └── memory/
│
├── infrastructure/
│   ├── docker/
│   ├── kubernetes/
│   └── observability/
│
├── docs/
│   ├── architecture/
│   ├── adr/
│   └── learning-log/
│
└── .github/
    └── workflows/
```

Create directories as the relevant phase begins; do not create empty
scaffolding for every future topic on day one.

## 11. Progress tracker

Update this table as evidence is completed.

  Phase                                   Status        Evidence / link
  --------------------------------------- ------------- -----------------
  1\. Rust Core & Ownership               Not started   
  2\. Advanced Rust & Memory              Not started   
  3\. Async Rust & Networking             Not started   
  4\. Distributed Systems Engineering     Not started   
  5\. Performance & Systems Programming   Not started   
  6\. Production Distributed Rust         Not started   

Suggested status values: `Not started`, `In progress`, `Completed`,
`Needs review`.

------------------------------------------------------------------------

## Guiding principle

**Build, measure, break, recover, and explain.**

The goal is not to collect Rust syntax or finish books. The goal is to
demonstrate that you can design, implement, test, optimize, and operate
reliable distributed software in Rust.
