# Rust Expert Journey

## Distributed Event Processing Platform

**A 12-month engineering programme to become a Rust specialist in Distributed Systems and High-Performance Backend Engineering.**

| Attribute | Value |
|---|---|
| Duration | 12 months |
| Total effort | 1,560 hours |
| Weekly commitment | 30 hours |
| Daily commitment | 6 hours, Monday to Friday |
| Primary language | Rust |
| Primary specialization | Distributed Systems & Backend Engineering |
| Secondary specialization | Systems Programming |
| Learning allocation | 70% / 30% |
| Main project | Distributed Event Processing Platform |

---

# 1. Mission and Professional Objective

## 1.1 Mission

The objective of this repository is to document and demonstrate a structured, intensive, practical journey towards becoming a highly capable Rust engineer.

The programme builds upon more than 30 years of software engineering experience, including extensive Java development, software architecture, backend engineering, and distributed systems.

The goal is not to learn programming from scratch.

The goal is to transfer existing engineering experience into the Rust ecosystem while developing the language-specific expertise required to design, implement, optimize, and operate sophisticated Rust systems.

## 1.2 Target Professional Positioning

The intended professional positioning is:

**Senior Software Engineer | Rust | Distributed Systems | High-Performance Backend**

The programme focuses on the engineering capabilities expected in roles involving:

- Distributed backend systems.
- High-performance services.
- Event-driven architectures.
- Concurrent and asynchronous applications.
- Storage and networking systems.
- Systems programming.

Completing this programme does not automatically establish professional Rust experience or guarantee a Senior title. The objective is to build demonstrable technical competence and a portfolio strong enough to compete for relevant positions.

## 1.3 Learning Strategy

The programme follows three principles:

1. **Project-driven learning:** almost all learning activities contribute to one substantial software system.
2. **Engineering depth:** understand why architectural and implementation decisions are made.
3. **Verifiable progress:** every phase produces working software, tests, benchmarks, documentation, and demonstrable results.

The main project is not a collection of unrelated exercises.

It is one evolving system developed progressively over 12 months.

---

# 2. Project Vision and Scope

## 2.1 Project Name

**Distributed Event Processing Platform**

## 2.2 Vision

Design and implement a distributed event processing platform in Rust.

The platform will receive events from client applications, validate and persist them, distribute them across partitions, and allow independent consumers to process them.

The system will progressively evolve from a single-process application into a multi-node distributed backend.

Its development will provide practical experience with Rust language fundamentals, memory management, concurrency, asynchronous networking, storage engineering, replication, fault tolerance, and performance optimization.

## 2.3 Architectural Inspiration

The project takes inspiration from selected architectural concepts used by Apache Kafka and other distributed messaging systems.

However, this project is NOT intended to:

- Reimplement Apache Kafka.
- Provide Kafka protocol compatibility.
- Reproduce the complete Kafka feature set.
- Replace Kafka in production environments.
- Build an entire distributed database or messaging ecosystem.

The objective is to implement a smaller, independently designed event processing platform with clearly defined guarantees.

Kafka is a source of architectural inspiration, not the implementation specification.

## 2.4 Main Functional Capabilities

The final platform should progressively support the following capabilities.

| ID | Capability | Description |
|---|---|---|
| F-01 | Event ingestion | Accept events from external clients |
| F-02 | Event validation | Validate event structure and metadata |
| F-03 | Event serialization | Encode and decode event records |
| F-04 | Persistent storage | Store events in an append-oriented log |
| F-05 | Topics and partitions | Organize events into logical streams |
| F-06 | Event consumption | Allow consumers to retrieve events |
| F-07 | Consumer offsets | Track consumer processing progress |
| F-08 | Concurrent processing | Support multiple producers and consumers |
| F-09 | Replication | Replicate selected data between nodes |
| F-10 | Node coordination | Manage a limited cluster coordination model |
| F-11 | Recovery | Recover data and service state after failures |
| F-12 | Observability | Expose metrics, logs, and distributed traces |

These capabilities will not be implemented simultaneously. Their implementation is distributed across the six roadmap phases.

## 2.5 Non-Functional Requirements

The project must also address the following engineering concerns.

| ID | Requirement | Objective |
|---|---|---|
| NFR-01 | Performance | Measure throughput and latency under different workloads |
| NFR-02 | Reliability | Handle expected process and node failures |
| NFR-03 | Durability | Define and test persistence guarantees |
| NFR-04 | Concurrency | Support concurrent operations safely |
| NFR-05 | Scalability | Distribute selected workloads across nodes |
| NFR-06 | Maintainability | Use clear module boundaries and documented interfaces |
| NFR-07 | Testability | Automate unit, integration, and failure tests |
| NFR-08 | Observability | Make system behavior measurable and diagnosable |
| NFR-09 | Security | Validate inputs and protect service interfaces |
| NFR-10 | Reproducibility | Make tests, benchmarks, and deployments reproducible |

## 2.6 Initial Engineering Guarantees

The platform must document exactly what it guarantees.

Initial design targets:

| Area | Target |
|---|---|
| Ordering | Ordering within an individual partition |
| Persistence | Append-oriented storage with restart recovery |
| Consumer progress | Persisted consumer offsets |
| Delivery semantics | At-least-once behavior for the defined workflow, with duplicate processing documented |
| Replication | Limited leader/follower replication introduced in a later phase |
| Failure handling | Tested restart, timeout, retry, and node-loss scenarios |
| Performance | Reproducible measurements of throughput, latency, and resource consumption |

These are targets, not claims about already implemented features.

The actual guarantees must be validated by tests and documented engineering decisions.

---

# 3. Target Architecture

## 3.1 Logical Architecture

The target architecture is:

                         +--------------------------+
                         |    Client Applications   |
                         +------------+-------------+
                                      |
                                HTTP / gRPC
                                      |
                         +------------v-------------+
                         |       Ingestion API      |
                         +------------+-------------+
                                      |
                         +------------v-------------+
                         |  Protocol & Validation   |
                         +------------+-------------+
                                      |
                         +------------v-------------+
                         |     Partition Manager    |
                         +------------+-------------+
                                      |
                    +-----------------+-----------------+
                    |                                   |
             +------v-------+                    +------v-------+
             |  Partition 0 |                    |  Partition 1 |
             +------+-------+                    +------+-------+
                    |                                   |
                    +-----------------+-----------------+
                                      |
                         +------------v-------------+
                         |       Storage Engine     |
                         |  Append Log / Index      |
                         |  Recovery                |
                         +------------+-------------+
                                      |
                         +------------v-------------+
                         |     Replication Layer    |
                         +------------+-------------+
                                      |
                         +------------v-------------+
                         |     Consumer Engine      |
                         | Processing / Offsets     |
                         +--------------------------+

                Observability: Metrics, Logs and Traces
                applies across all components.

## 3.2 Component Responsibilities

| Component | Responsibility |
|---|---|
| Event Core | Domain models, event identifiers, and shared types |
| Protocol | Event encoding, decoding, and network message definitions |
| Ingestion API | External client interfaces and request handling |
| Partition Manager | Topic and partition management |
| Storage Engine | Persistent event storage, indexing, and recovery |
| Consumer Engine | Event retrieval and offset management |
| Replication Layer | Data replication between nodes |
| Coordination | Limited cluster membership and leadership management |
| Observability | Metrics, logs, and distributed tracing |

## 3.3 Architectural Evolution

The architecture will evolve through six stages:

1. Single-process event processing.
2. Persistent local storage.
3. Asynchronous network service.
4. Multi-node distributed system.
5. Performance optimization.
6. Production-oriented deployment and operations.

Do not implement the complete architecture at the beginning.

Introduce components only when the corresponding roadmap phase requires them.

---

# 4. Technology Stack and Technical Decisions

## 4.1 Core Technology Stack

| Area | Technology | Purpose |
|---|---|---|
| Programming language | Rust | Main implementation language |
| Build system | Cargo | Workspace and dependency management |
| Async runtime | Tokio | Asynchronous execution |
| HTTP framework | Axum | Client-facing HTTP API |
| RPC framework | Tonic | Optional gRPC interfaces |
| Serialization | Serde | Event serialization |
| Persistent storage | Custom append-oriented log | Storage engineering practice |
| Database | PostgreSQL / SQLx, if justified | Metadata and management data |
| Testing | Built-in Rust testing | Unit and integration tests |
| Property testing | Proptest | Validate data and system invariants |
| Benchmarking | Criterion | Performance measurement |
| Profiling | flamegraph | CPU profiling |
| Containers | Docker | Reproducible deployment |
| Orchestration | Kubernetes | Final deployment exercises |
| Observability | OpenTelemetry | Metrics and distributed tracing |
| CI/CD | GitHub Actions | Automated validation |

The stack is a target selection. Each dependency will be introduced when it becomes relevant.

## 4.2 Important Technical Decisions

### Decision 1 — Implement the storage engine

The initial storage engine will be implemented as part of the learning project.

The purpose is to understand:

- Append-only logs.
- Record framing.
- Segment management.
- Indexing.
- Persistence.
- Recovery.
- Corruption handling.

Do not introduce an external messaging broker as a substitute for implementing the core storage concepts.

### Decision 2 — Use Tokio for asynchronous execution

Tokio will provide the asynchronous runtime.

The objective is to understand its execution model, task lifecycle, channels, synchronization, cancellation, and backpressure.

### Decision 3 — Start with a single node

The first version will run as a single process.

Distributed functionality will only be introduced after the storage and networking foundations are stable.

### Decision 4 — Keep consensus scope controlled

Raft will be studied as part of distributed systems engineering.

Any implementation will have an explicitly limited educational scope.

The project must not claim production-grade consensus or strong consistency without the necessary implementation, reasoning, and verification.

### Decision 5 — Measure before optimizing

Performance improvements must be supported by reproducible benchmarks and profiling evidence.

Avoid introducing unsafe Rust, SIMD, or complex low-level optimizations without a demonstrated need.

### Decision 6 — Build one coherent product

All components belong to the same platform and repository.

The six learning phases are development stages, not independent projects.

---

# 5. Learning Allocation and Roadmap

## 5.1 Learning Allocation

The programme follows a 70/30 specialization strategy.

| Learning Track | Allocation | Total Hours |
|---|---:|---:|
| Distributed Systems & High-Performance Backend | 70% | 1,092 h |
| Systems Programming & Low-Level Rust | 30% | 468 h |
| **Total** | **100%** | **1,560 h** |

### Track A — Distributed Systems & Backend Rust

This is the primary specialization.

Topics:

- Backend architecture.
- Event-driven systems.
- Persistent storage.
- Asynchronous networking.
- Distributed systems.
- Replication.
- Consistency models.
- Fault tolerance.
- Performance engineering.
- Production operations.

### Track B — Systems Programming & Low-Level Rust

This track provides the technical depth required to understand how Rust applications interact with memory, processors, operating systems, and hardware resources.

Topics:

- Ownership and borrowing.
- Lifetimes.
- Memory layout.
- Stack and heap.
- Smart pointers.
- Concurrency primitives.
- Atomics.
- Unsafe Rust.
- Zero-copy techniques.
- Linux I/O.
- CPU and memory profiling.

## 5.2 Twelve-Month Roadmap

The programme is divided into six sequential phases.

| Phase | Weeks | Duration | Hours | Main Result |
|---|---|---:|---:|---|
| 1. Rust Core & Ownership | 1–8 | 2 months | 240 h | Single-process event engine |
| 2. Advanced Rust & Storage Engine | 9–16 | 2 months | 240 h | Persistent storage engine |
| 3. Async Rust & Networking | 17–24 | 2 months | 240 h | Networked event service |
| 4. Distributed Systems Engineering | 25–36 | 3 months | 360 h | Multi-node event platform |
| 5. Performance & Systems Programming | 37–44 | 2 months | 240 h | Optimized platform |
| 6. Production Readiness & Final Evaluation | 45–52 | 2 months | 240 h | Release candidate |
| **Total** | **52 weeks** | **12 months** | **1,560 h** | **One complete platform** |

---

# 6. Detailed Learning and Implementation Plan

# Phase 1 — Rust Core & Ownership

**Weeks:** 1–8  
**Effort:** 240 hours  
**Platform milestone:** Single-process event processing engine.

## 6.1 Learning Objectives

### Rust fundamentals

Study and practice:

- Rust installation and toolchain.
- Cargo and workspace organization.
- Modules and visibility.
- Variables and mutability.
- Primitive and compound types.
- Structs and enums.
- Pattern matching.
- Collections.
- Functions and control flow.
- Error handling with `Option` and `Result`.

### Ownership and memory management

Develop a deep understanding of:

- Ownership rules.
- Borrowing.
- Immutable and mutable references.
- Slices.
- Lifetimes.
- Move semantics.
- Copy and Clone.
- Stack and heap fundamentals.

### Abstraction and code organization

Study:

- Traits.
- Generics.
- Trait bounds.
- Associated types.
- Iterators.
- Closures.
- Error types.
- API design.

### Testing

Learn:

- Unit tests.
- Integration tests.
- Documentation tests.
- Test organization.
- Assertions.
- Test fixtures.

## 6.2 Project Implementation

Build the first version of the Distributed Event Processing Platform.

At this stage, everything runs inside one process.

### Implementation tasks

1. Create the Rust workspace.
2. Define the initial event domain model.
3. Implement event identifiers.
4. Define topics and partitions.
5. Implement an in-memory event store.
6. Implement append operations.
7. Implement read operations.
8. Implement basic filtering by partition.
9. Define domain-specific errors.
10. Create a basic command-line client.
11. Add unit and integration tests.

### Initial domain model

The platform should introduce concepts such as:

- Event.
- EventId.
- Topic.
- Partition.
- Offset.
- Producer.
- Consumer.

Keep the domain model simple. Do not introduce distributed coordination or persistence yet.

## 6.3 Deliverables

- Working Rust workspace.
- Initial domain model.
- In-memory event store.
- Basic producer and consumer operations.
- CLI demonstration.
- Unit and integration tests.
- Initial architecture documentation.
- First Architecture Decision Records.

## 6.4 Acceptance Criteria

You must be able to:

- Explain ownership and borrowing decisions.
- Implement generic and trait-based abstractions.
- Design idiomatic error handling.
- Explain why a value is moved, borrowed, or cloned.
- Write tests for the event processing logic.
- Organize a multi-crate Rust project.

**Phase 1 result:** a working single-process event processing engine.

---

# Phase 2 — Advanced Rust & Storage Engine

**Weeks:** 9–16  
**Effort:** 240 hours  
**Platform milestone:** Persistent event storage.

## 6.5 Learning Objectives

### Advanced Rust

Study:

- Advanced lifetimes.
- Trait objects.
- Dynamic dispatch and static dispatch.
- Associated types.
- Smart pointers.
- `Box`.
- `Rc`.
- `Arc`.
- `Cow`.
- Interior mutability.
- `Mutex`.
- `RwLock`.
- `RefCell`.
- `Send` and `Sync`.

### Memory and concurrency fundamentals

Study:

- Memory layout.
- Alignment.
- Stack and heap.
- Atomic operations.
- Memory ordering fundamentals.
- Synchronization.
- Data races.
- Shared ownership.

### Storage engineering

Study:

- File I/O.
- Buffered I/O.
- Binary formats.
- Serialization.
- Append-only logs.
- Indexing.
- Checksums.
- Crash consistency.
- Recovery algorithms.

## 6.6 Project Implementation

Replace the in-memory-only storage implementation with a persistent storage engine.

### Storage Engine capabilities

Implement:

1. Append-only log files.
2. Record framing.
3. Record serialization.
4. Event offsets.
5. Segment files.
6. Segment rotation.
7. Basic indexes.
8. Sequential reads.
9. Recovery after restart.
10. Detection of incomplete final records.
11. Corruption handling.
12. Storage integration tests.

### Storage format

Define a binary record format.

A record should contain enough information to identify and recover an event, including:

- Record length.
- Event identifier.
- Offset.
- Timestamp.
- Payload.
- Integrity information.

Document the format and its evolution.

### Recovery behavior

When the service restarts:

1. Discover existing segments.
2. Read segment records.
3. Validate record framing and integrity.
4. Rebuild the required indexes.
5. Identify an incomplete or corrupted final record.
6. Recover the valid portion of the log.
7. Make recovered events available to consumers.

Define explicitly how corruption is handled and what data can be recovered.

## 6.7 Deliverables

- Persistent storage engine.
- Append-only segment implementation.
- Index implementation.
- Recovery mechanism.
- Binary storage format documentation.
- Corruption and restart tests.
- Initial storage benchmarks.

## 6.8 Acceptance Criteria

You must be able to:

- Explain the storage format.
- Explain how offsets are assigned.
- Explain the recovery algorithm.
- Demonstrate recovery after process termination.
- Test incomplete and corrupted records.
- Justify memory management decisions.
- Explain the trade-offs between durability and performance.

**Phase 2 result:** a persistent event processing engine capable of recovering its stored data after restart.

---

# Phase 3 — Async Rust & Networking

**Weeks:** 17–24  
**Effort:** 240 hours  
**Platform milestone:** Networked asynchronous event service.

## 6.9 Learning Objectives

### Async Rust

Study:

- Futures.
- Async and await.
- Tokio runtime.
- Tasks.
- Task scheduling.
- Channels.
- Bounded and unbounded queues.
- Async synchronization.
- Cancellation.
- Timeouts.
- Graceful shutdown.
- Backpressure.

### Networking

Study:

- TCP fundamentals.
- Connection lifecycle.
- Network framing.
- HTTP.
- Request and response handling.
- Serialization over the network.
- Connection errors.
- Network timeouts.

### Backend development

Study:

- Axum.
- Tonic and gRPC.
- API design.
- Request validation.
- Middleware.
- Service boundaries.
- Integration testing.

### Performance and reliability

Study:

- Concurrent request processing.
- Queue limits.
- Resource management.
- Load testing.
- Async debugging.

## 6.10 Project Implementation

Expose the platform through a network API.

### Ingestion API

Implement:

- Event publishing endpoint.
- Event retrieval endpoint.
- Topic management endpoint.
- Partition information endpoint.
- Basic health endpoint.

### Consumer API

Implement:

- Consumer registration.
- Event retrieval.
- Offset management.
- Consumer progress tracking.

### Asynchronous processing

Introduce:

- Tokio runtime.
- Concurrent producer handling.
- Concurrent consumer handling.
- Bounded channels.
- Backpressure.
- Request timeouts.
- Graceful shutdown.

Use HTTP with Axum as the initial external interface.

Introduce gRPC with Tonic only when there is a clear technical reason to support a second protocol.

## 6.11 Deliverables

- Asynchronous Rust server.
- HTTP API.
- API documentation.
- Concurrent producers and consumers.
- Backpressure implementation.
- Graceful shutdown.
- Integration tests.
- Initial load-testing report.

## 6.12 Acceptance Criteria

You must be able to:

- Explain how Tokio schedules tasks.
- Explain the lifecycle of an asynchronous task.
- Implement bounded channels.
- Explain backpressure.
- Handle cancellation and timeouts correctly.
- Demonstrate concurrent event ingestion.
- Test the API under concurrent load.

**Phase 3 result:** a network-accessible event processing service supporting concurrent clients.

---

# Phase 4 — Distributed Systems Engineering

**Weeks:** 25–36  
**Effort:** 360 hours  
**Platform milestone:** Multi-node distributed event processing.

## 6.13 Learning Objectives

### Distributed systems fundamentals

Study:

- Distributed system architecture.
- Partial failures.
- Network partitions.
- Latency and timeouts.
- Failure detection.
- Replication.
- Consistency models.
- Leader and follower architectures.
- Idempotency.
- Deduplication.
- Message delivery semantics.
- Consumer groups.
- Partition assignment.

### Distributed coordination

Study:

- Leader election.
- Heartbeats.
- Membership.
- Quorum concepts.
- Raft architecture.
- Replicated state machines.
- Split-brain scenarios.

### Reliability

Study:

- Retry strategies.
- Exponential backoff.
- Circuit breakers.
- Failure recovery.
- Fault injection.
- Distributed integration testing.

## 6.14 Project Implementation

Extend the existing platform to support multiple nodes.

### Cluster architecture

Implement progressively:

1. Configurable node identities.
2. Node-to-node communication.
3. Partition ownership.
4. Leader/follower replication.
5. Replication acknowledgements.
6. Heartbeats.
7. Basic leader election.
8. Node failure detection.
9. Recovery after node restart.
10. Consumer access to replicated partitions.

### Replication model

Start with a deliberately limited replication model.

Document:

- Which node accepts writes.
- How records are replicated.
- When an event is acknowledged.
- What happens when a follower is unavailable.
- What happens when the leader fails.
- What data can be lost under each failure scenario.

### Consensus study

Study Raft deeply.

A limited educational implementation can be developed if the preceding cluster milestones are stable.

Do not claim production-grade consensus, linearizability, or complete split-brain prevention without appropriate implementation and verification.

## 6.15 Deliverables

- Multi-node cluster.
- Node communication protocol.
- Replication implementation.
- Failure detection mechanism.
- Documented leadership model.
- Restart and recovery tests.
- Network partition test scenarios.
- Consistency and delivery guarantees documentation.

## 6.16 Acceptance Criteria

You must be able to:

- Explain partial failures.
- Explain replication trade-offs.
- Describe the consistency guarantees.
- Demonstrate node failure and recovery.
- Explain the limitations of the coordination model.
- Explain the consequences of network partitions.
- Defend the replication architecture.

**Phase 4 result:** a multi-node event processing platform with a documented and tested replication model.

---

# Phase 5 — Performance & Systems Programming

**Weeks:** 37–44  
**Effort:** 240 hours  
**Platform milestone:** Performance-optimized event processing platform.

## 6.17 Learning Objectives

Study:

- CPU profiling.
- Memory profiling.
- Allocation analysis.
- Memory layout.
- Cache locality.
- Zero-copy techniques.
- Buffer management.
- SIMD fundamentals.
- Linux processes and threads.
- Linux I/O.
- `epoll`.
- Lock contention.
- Atomic operations.
- Unsafe Rust.
- Performance benchmarking.

## 6.18 Project Implementation

Optimize the existing platform using measurements.

Investigate:

1. Serialization and deserialization.
2. Storage writes.
3. Storage reads.
4. Index lookups.
5. Network request processing.
6. Memory allocation.
7. Producer throughput.
8. Consumer throughput.
9. Replication overhead.

Use profiling to identify bottlenecks.

Implement optimizations only when measurements demonstrate a meaningful improvement.

## 6.19 Deliverables

- Criterion benchmark suite.
- CPU profiling reports.
- Memory profiling reports.
- Before-and-after performance comparisons.
- Documented optimization decisions.
- Performance regression tests.
- Throughput and latency analysis.

## 6.20 Acceptance Criteria

You must be able to:

- Identify CPU and memory bottlenecks.
- Interpret profiling results.
- Explain allocation costs.
- Measure p50, p95, and p99 latency.
- Justify zero-copy or low-level optimizations.
- Explain when unsafe Rust is appropriate.

**Phase 5 result:** a measured and optimized version of the distributed event processing platform.

---

# Phase 6 — Production Readiness & Final Evaluation

**Weeks:** 45–52  
**Effort:** 240 hours  
**Platform milestone:** Production-oriented release candidate.

## 6.21 Learning Objectives

Study:

- Production Rust project organization.
- Configuration management.
- Secrets management.
- Structured logging.
- Metrics.
- Distributed tracing.
- Health checks.
- Graceful shutdown.
- Security fundamentals.
- Docker.
- Kubernetes.
- CI/CD.
- Load testing.
- Chaos testing.
- Operational documentation.

## 6.22 Project Implementation

Prepare the platform for a realistic technical demonstration.

Implement:

1. Structured application configuration.
2. Health and readiness endpoints.
3. Metrics and structured logs.
4. Distributed tracing.
5. Docker images.
6. Local multi-node deployment.
7. CI pipeline.
8. Automated quality checks.
9. Load and stress tests.
10. Failure and recovery demonstrations.
11. Security review.
12. Installation and operational documentation.

Use Kubernetes to demonstrate deployment and operational understanding, without introducing unnecessary infrastructure complexity.

## 6.23 Deliverables

- Versioned release candidate.
- Docker deployment.
- Local cluster deployment.
- CI/CD pipeline.
- Observability dashboards.
- Load-testing reports.
- Failure-testing reports.
- Architecture documentation.
- Operational runbooks.
- Final engineering retrospective.

## 6.24 Acceptance Criteria

You must be able to:

- Deploy the platform.
- Operate a multi-node environment.
- Monitor system behavior.
- Diagnose performance issues.
- Demonstrate failure recovery.
- Explain security decisions.
- Defend the architecture and its trade-offs.

**Phase 6 result:** a documented, tested, observable, and deployable Rust distributed backend.

---

# 7. Roadmap Completion Summary

| Phase | Main Engineering Capability |
|---|---|
| 1 | Rust language and ownership |
| 2 | Advanced Rust and storage engineering |
| 3 | Async Rust and networking |
| 4 | Distributed systems and replication |
| 5 | Performance and systems programming |
| 6 | Production engineering and operations |

**The six phases progressively build one platform. No phase creates an unrelated standalone project.**

---

# 8. Learning Resources

The learning resources are organized by technical specialization and roadmap phase.

The priority is to combine rigorous theoretical foundations with practical implementation.

## 8.1 Rust Language Fundamentals

### Essential books

| Book | Author | Main purpose | Relevant phases |
|---|---|---|---|
| The Rust Programming Language (2nd Edition) | Steve Klabnik and Carol Nichols | Rust fundamentals, ownership, borrowing, lifetimes | 1–2 |
| Programming Rust (2nd Edition) | Jim Blandy, Jason Orendorff, and Leonora F. S. Tindall | Advanced language concepts and systems programming | 1–5 |
| Rust for Rustaceans | Jon Gjengset | Advanced Rust design and idiomatic engineering | 2–5 |
| Effective Rust | David Drysdale | Practical Rust engineering practices | 1–6 |

### Official documentation

| Resource | Purpose |
|---|---|
| https://doc.rust-lang.org/book/ | Main Rust language guide |
| https://doc.rust-lang.org/rust-by-example/ | Executable examples |
| https://github.com/rust-lang/rustlings | Hands-on Rust exercises |
| https://doc.rust-lang.org/reference/ | Detailed language reference |
| https://doc.rust-lang.org/nomicon/ | Unsafe Rust and memory invariants |
| https://rust-lang.github.io/api-guidelines/ | Public API design guidelines |

### Expected outcome

Develop the ability to write idiomatic Rust, understand the ownership model, design APIs, manage errors, and organize maintainable Rust applications.

---

## 8.2 Advanced Rust and Memory Management

### Recommended resources

| Resource | Purpose | Relevant phases |
|---|---|---|
| Rust for Rustaceans | Advanced type system and language design | 2–5 |
| The Rustonomicon | Unsafe Rust and memory safety | 2, 5 |
| Rust Reference | Precise language semantics | 1–5 |
| Rust Atomics and Locks — Mara Bos | Atomics, synchronization, and concurrency | 2–5 |

### Topics to master

- Ownership and borrowing.
- Lifetimes.
- Generic programming.
- Trait objects.
- Smart pointers.
- Interior mutability.
- Send and Sync.
- Atomics.
- Memory layout.
- Unsafe code and invariants.

### Expected outcome

Understand the memory and concurrency model deeply enough to make sound architectural decisions in complex Rust applications.

---

## 8.3 Async Rust and Networking

### Essential resources

| Resource | Purpose | Relevant phases |
|---|---|---|
| Async Rust: Unleashing the Power of Fearless Concurrency | Async programming concepts | 3 |
| Tokio Tutorial | Practical asynchronous Rust | 3–4 |
| Tokio Documentation | Runtime, tasks, channels, synchronization | 3–4 |
| Axum Documentation | HTTP service development | 3 |
| Tonic Documentation | gRPC implementation | 3–4 |
| Hyper Documentation | HTTP fundamentals | 3 |

### Documentation

- https://tokio.rs/tokio/tutorial
- https://docs.rs/tokio/latest/tokio/
- https://docs.rs/axum/latest/axum/
- https://docs.rs/tonic/latest/tonic/
- https://docs.rs/hyper/latest/hyper/

### Topics to master

- Futures and async/await.
- Tokio runtime.
- Task scheduling.
- Channels.
- Shared state.
- Cancellation.
- Timeouts.
- Backpressure.
- TCP networking.
- HTTP.
- gRPC.
- Graceful shutdown.

### Expected outcome

Build reliable asynchronous services and understand the implications of concurrency, scheduling, cancellation, and resource management.

---

## 8.4 Distributed Systems and Storage Engineering

This is the most important theoretical specialization for the target role.

### Essential books

| Book | Author | Main purpose | Relevant phases |
|---|---|---|---|
| Designing Data-Intensive Applications | Martin Kleppmann | Distributed systems, replication, partitioning, consistency | 2–4 |
| Database Internals | Alex Petrov | Storage engines, indexes, database architecture | 2, 4 |
| Understanding Distributed Systems | Roberto Vitillo | Distributed systems fundamentals | 4 |

### Courses and technical material

| Resource | Purpose |
|---|---|
| MIT 6.5840 Distributed Systems | Distributed systems theory and implementation |
| Raft Paper | Consensus and leader election |
| Raft Visualization | Understand Raft behavior |
| PostgreSQL Documentation | Relational storage and transaction concepts |

Links:

- https://pdos.csail.mit.edu/6.824/
- https://raft.github.io/raft.pdf
- https://raft.github.io/
- https://www.postgresql.org/docs/

### Topics to master

- Replication.
- Partitioning.
- Consistency models.
- Partial failure.
- Network partitions.
- Leader election.
- Quorums.
- Replicated state machines.
- Message delivery semantics.
- Idempotency.
- Recovery.

### Expected outcome

Understand the theoretical and practical foundations of distributed systems and apply them to the platform's replication and coordination design.

---

## 8.5 Performance and Systems Programming

### Essential resources

| Resource | Purpose | Relevant phases |
|---|---|---|
| Systems Performance — Brendan Gregg | Performance analysis and Linux systems | 5–6 |
| The Rust Performance Book | Rust performance optimization | 5 |
| Criterion.rs | Statistical benchmarking | 5 |
| flamegraph | CPU profiling | 5 |
| Linux man-pages | Linux system interfaces | 5 |

Links:

- https://nnethercote.github.io/perf-book/
- https://bheisler.github.io/criterion.rs/book/
- https://github.com/flamegraph-rs/flamegraph
- https://man7.org/linux/man-pages/

### Topics to master

- CPU profiling.
- Memory profiling.
- Allocation analysis.
- Cache locality.
- Memory alignment.
- Zero-copy.
- Buffer management.
- SIMD.
- Linux I/O.
- epoll.
- Lock contention.
- Benchmark methodology.

### Expected outcome

Identify performance bottlenecks and optimize the platform based on reproducible measurements.

---

## 8.6 Testing and Quality Engineering

### Recommended tools

| Tool | Purpose | Relevant phases |
|---|---|---|
| Cargo Test | Unit and integration tests | 1–6 |
| Proptest | Property-based testing | 2–6 |
| Cargo Fuzz | Fuzz testing | 2–6 |
| Criterion | Performance benchmarking | 2–6 |
| Docker | Reproducible environments | 4–6 |
| GitHub Actions | Continuous integration | 1–6 |

Links:

- https://doc.rust-lang.org/cargo/commands/cargo-test.html
- https://github.com/proptest-rs/proptest
- https://github.com/rust-fuzz/cargo-fuzz
- https://docs.docker.com/
- https://docs.github.com/actions

### Testing strategy

Testing must be introduced from the beginning.

The project should progressively include:

1. Unit tests.
2. Integration tests.
3. Property-based tests.
4. Storage recovery tests.
5. Concurrent execution tests.
6. Network failure tests.
7. Multi-node integration tests.
8. Load tests.
9. Performance regression tests.

### Expected outcome

Build confidence in correctness and demonstrate that the platform behaves predictably under normal and failure conditions.

---

## 8.7 Observability and Production Engineering

### Recommended resources

| Resource | Purpose | Relevant phases |
|---|---|---|
| OpenTelemetry Documentation | Distributed tracing and telemetry | 6 |
| Prometheus Documentation | Metrics and monitoring | 6 |
| Grafana Documentation | Dashboards and visualization | 6 |
| Docker Documentation | Container deployment | 6 |
| Kubernetes Documentation | Container orchestration | 6 |

Links:

- https://opentelemetry.io/docs/
- https://prometheus.io/docs/
- https://grafana.com/docs/
- https://docs.docker.com/
- https://kubernetes.io/docs/

### Topics to master

- Structured logging.
- Metrics.
- Distributed tracing.
- Health checks.
- Readiness and liveness.
- Configuration.
- Secrets management.
- Docker.
- Kubernetes.
- CI/CD.
- Operational troubleshooting.

### Expected outcome

Deploy, observe, troubleshoot, and explain the operational behavior of the final platform.

---

# 9. Resource Prioritization by Phase

The following table maps learning resources to the implementation roadmap.

| Phase | Primary resources | Main practical application |
|---|---|---|
| 1. Rust Core | The Rust Programming Language, Rustlings | Event domain model and in-memory engine |
| 2. Storage Engine | Programming Rust, Rust for Rustaceans, Database Internals | Persistent log and recovery |
| 3. Async & Networking | Async Rust, Tokio, Axum | Network API and concurrent clients |
| 4. Distributed Systems | DDIA, MIT 6.5840, Raft Paper | Replication and cluster coordination |
| 5. Performance | Systems Performance, Rust Performance Book | Profiling and optimization |
| 6. Production | OpenTelemetry, Docker, Kubernetes documentation | Deployment and operational readiness |

Do not try to complete every book before writing code.

Study the concepts required by the current phase and apply them immediately.

---

# 10. Study Method and Weekly Organization

## 10.1 Weekly Commitment

The programme requires 30 hours per week.

| Activity | Hours per week |
|---|---:|
| Books and technical documentation | 5 |
| Exercises and experiments | 5 |
| Main project implementation | 12.5 |
| Testing, debugging, and profiling | 5 |
| Documentation and technical review | 2.5 |
| **Total** | **30** |

## 10.2 Daily Schedule

The target daily commitment is six hours, Monday to Friday.

| Activity | Daily time |
|---|---:|
| Reading and technical documentation | 1 hour |
| Exercises and experiments | 1 hour |
| Project implementation | 2.5 hours |
| Testing and debugging | 1 hour |
| Documentation and review | 0.5 hour |
| **Total** | **6 hours** |

## 10.3 Weekly Engineering Routine

Every week should produce measurable progress.

Required activities:

1. Study the relevant Rust or distributed systems concepts.
2. Complete focused exercises.
3. Implement a new platform capability or improve an existing one.
4. Write automated tests.
5. Review compiler warnings and Clippy suggestions.
6. Document important design decisions.
7. Commit working code.
8. Review what was learned and identify knowledge gaps.

## 10.4 Learning Principles

Follow these principles throughout the programme:

- Prefer understanding over memorization.
- Read compiler errors carefully.
- Avoid copying solutions without understanding them.
- Prefer simple designs before introducing abstractions.
- Measure performance before optimizing.
- Write tests alongside implementation.
- Document important technical decisions.
- Review and refactor earlier code as your Rust knowledge improves.

The main objective is to develop independent engineering judgment in Rust.

---

# 11. Repository Structure and Workspace Organization

## 11.1 Repository Organization

The project will be maintained in a single Git repository:

`rust-expert-journey`

The repository will contain the complete implementation of the Distributed Event Processing Platform, its documentation, tests, benchmarks, deployment configuration, and engineering decisions.

The repository must evolve progressively throughout the programme.

Do not create every crate, module, or directory at the beginning. Introduce them when the corresponding implementation phase requires them.

The main organizational principles are:

- Keep domain logic independent of infrastructure.
- Separate storage, networking, and distributed coordination concerns.
- Define clear interfaces between components.
- Avoid unnecessary abstractions.
- Keep tests close to the code they validate.
- Document significant architectural decisions.
- Maintain a reproducible development environment.

## 11.2 Workspace Organization

Use a Cargo workspace to organize the platform into independent but coordinated Rust crates.

The target workspace structure is:

```text
rust-expert-journey/
│
├── Cargo.toml
├── Cargo.lock
├── rust-toolchain.toml
├── .gitignore
├── README.md
│
├── crates/
│   │
│   ├── event-core/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   ├── event-protocol/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   ├── event-storage/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   ├── event-ingestion/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   ├── event-consumer/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   ├── event-cluster/
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   └── event-observability/
│       ├── Cargo.toml
│       └── src/
│
├── apps/
│   └── event-node/
│       ├── Cargo.toml
│       └── src/
│
├── tools/
│   └── event-cli/
│       ├── Cargo.toml
│       └── src/
│
├── tests/
│   ├── integration/
│   ├── storage/
│   ├── networking/
│   └── distributed/
│
├── benches/
│
├── docs/
│   ├── architecture/
│   ├── adr/
│   ├── storage/
│   ├── distributed-systems/
│   ├── performance/
│   └── operations/
│
├── deploy/
│   ├── docker/
│   └── kubernetes/
│
├── scripts/
│
└── .github/
    └── workflows/
```

This is the target organization, not the initial implementation requirement.

### Crate responsibilities

| Crate | Responsibility |
|---|---|
| `event-core` | Domain types, events, topics, partitions, offsets, and domain errors |
| `event-protocol` | Serialization, deserialization, and communication message definitions |
| `event-storage` | Append-only logs, segments, indexes, persistence, and recovery |
| `event-ingestion` | Event ingestion and publishing operations |
| `event-consumer` | Event retrieval and consumer offset management |
| `event-cluster` | Node communication, replication, and coordination |
| `event-observability` | Shared telemetry and observability capabilities |
| `event-node` | Application entry point and component composition |
| `event-cli` | Command-line client for interacting with the platform |

### Workspace evolution

Introduce the workspace incrementally:

| Phase | Workspace evolution |
|---|---|
| Phase 1 | Start with `event-core` and a simple application |
| Phase 2 | Introduce `event-storage` |
| Phase 3 | Introduce `event-protocol` and `event-ingestion` |
| Phase 4 | Introduce `event-consumer` and `event-cluster` |
| Phase 5 | Refine crate boundaries based on profiling results |
| Phase 6 | Complete observability, deployment, and operational organization |

The final structure should reflect actual architectural boundaries rather than an arbitrary number of crates.

## 11.3 Documentation Organization

Documentation is part of the engineering deliverable.

Maintain the following documents:

| Document | Content |
|---|---|
| Architecture overview | System architecture and component responsibilities |
| Architecture Decision Records | Important technical decisions and alternatives |
| Storage format | Binary record format, segments, indexes, and recovery |
| Replication design | Replication model and failure assumptions |
| Consistency guarantees | Ordering, acknowledgements, and delivery semantics |
| Performance reports | Benchmark methodology and results |
| Testing strategy | Test levels, scenarios, and coverage |
| Operational guide | Configuration, deployment, monitoring, and troubleshooting |

Keep documentation synchronized with the implementation.

---

# 12. Engineering Standards and Code Quality

## 12.1 Rust Engineering Standards

The platform must follow idiomatic Rust development practices.

Apply the following principles:

- Prefer ownership and borrowing over unnecessary cloning.
- Use explicit and meaningful types.
- Prefer `Result` and typed errors for recoverable failures.
- Avoid unnecessary `unwrap()` and `expect()` in production paths.
- Use traits when they provide meaningful abstraction.
- Keep public APIs small and documented.
- Avoid premature generic abstractions.
- Minimize shared mutable state.
- Keep unsafe Rust isolated and justified.
- Handle asynchronous cancellation and shutdown correctly.

## 12.2 Formatting and Static Analysis

Use the standard Rust development tools.

Required checks:

```bash
cargo fmt --all -- --check
cargo check --workspace
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace
```

All checks must pass before considering a change complete.

Additional checks can be introduced when the project requires them.

## 12.3 Dependency Management

Dependencies must be introduced deliberately.

For each significant dependency:

1. Identify the technical problem it solves.
2. Evaluate whether the standard library is sufficient.
3. Review maintenance and compatibility.
4. Understand its impact on the dependency graph.
5. Document important choices.

Keep `Cargo.lock` committed for reproducible application builds.

Avoid adding dependencies simply because they are popular.

## 12.4 Architecture Decision Records

Use Architecture Decision Records (ADRs) to document significant decisions.

Each ADR should contain:

- Title and identifier.
- Context.
- Problem statement.
- Alternatives considered.
- Decision.
- Consequences.
- Status.

Suggested initial ADRs:

| ADR | Decision |
|---|---|
| ADR-001 | Cargo workspace organization |
| ADR-002 | Event and partition domain model |
| ADR-003 | Persistent storage format |
| ADR-004 | Offset assignment strategy |
| ADR-005 | Async runtime and networking |
| ADR-006 | Replication architecture |
| ADR-007 | Delivery semantics |
| ADR-008 | Observability architecture |

ADRs should describe decisions actually made, not decisions that have not yet been implemented.

---

# 13. Testing Strategy

Testing is a continuous engineering activity throughout all six phases.

The objective is not simply to achieve code coverage. It is to demonstrate that the platform satisfies its documented behavior and guarantees.

## 13.1 Unit Testing

Test individual components independently.

Examples:

- Event validation.
- Event identifier generation.
- Partition assignment.
- Offset calculation.
- Serialization and deserialization.
- Storage record framing.
- Index operations.
- Error handling.

Use Rust's built-in testing framework.

## 13.2 Integration Testing

Validate interactions between components.

Examples:

- Event ingestion followed by retrieval.
- Storage writes followed by restart recovery.
- Consumer offset persistence.
- Concurrent producer operations.
- API requests interacting with the storage engine.
- Replication between nodes.

## 13.3 Property-Based Testing

Use Proptest to validate important invariants.

Potential properties:

- Serialization followed by deserialization preserves valid events.
- Event offsets remain ordered within a partition.
- Valid appended records can be recovered.
- Index lookups return the expected records.
- Recovery does not expose incomplete records as valid events.

Property tests should focus on meaningful system invariants.

## 13.4 Fault and Recovery Testing

Introduce failure scenarios progressively.

| Scenario | Expected behavior |
|---|---|
| Process restart | Recover valid persisted records |
| Incomplete final record | Detect and handle according to recovery policy |
| Storage corruption | Report and handle the affected data |
| Network timeout | Apply documented timeout and retry behavior |
| Follower unavailable | Follow the documented replication policy |
| Leader failure | Apply the implemented leadership and recovery model |
| Network partition | Respect the documented consistency limitations |

Do not assume that a failure scenario is handled correctly merely because the application does not crash.

Validate the resulting data and system state.

## 13.5 Fuzz Testing

Introduce Cargo Fuzz when the serialization and storage formats are sufficiently stable.

Prioritize:

- Binary record parsing.
- Event deserialization.
- Network message parsing.
- Storage recovery.
- Input validation.

The objective is to identify malformed inputs, unexpected states, and potential crashes.

## 13.6 Test Evolution by Phase

| Phase | Main testing focus |
|---|---|
| 1 | Domain and in-memory engine tests |
| 2 | Storage, serialization, and recovery tests |
| 3 | API, concurrency, and asynchronous integration tests |
| 4 | Replication, node failure, and distributed integration tests |
| 5 | Performance and regression tests |
| 6 | Load, stress, recovery, and operational tests |

---

# 14. Performance Engineering and Benchmarking

Performance engineering is a dedicated part of the programme.

The objective is to understand how the platform behaves under different workloads and identify measurable opportunities for improvement.

## 14.1 Performance Metrics

Collect and analyze:

| Metric | Description |
|---|---|
| Throughput | Events processed per second |
| Publish latency | Time required to publish an event |
| Consumer latency | Time required to retrieve an event |
| p50 latency | Median observed latency |
| p95 latency | 95th percentile latency |
| p99 latency | 99th percentile latency |
| CPU utilization | CPU consumption under load |
| Memory usage | Memory consumption |
| Allocation rate | Frequency and volume of memory allocations |
| Storage throughput | Data written and read per second |

Document the workload and test environment for every benchmark.

## 14.2 Benchmarking Strategy

Use Criterion for repeatable microbenchmarks.

Create benchmarks for:

- Event serialization.
- Event deserialization.
- Record framing.
- Append operations.
- Sequential reads.
- Index lookups.
- Offset management.
- Event ingestion.
- Consumer retrieval.

Introduce system-level load tests when the network service is available.

## 14.3 Profiling Strategy

Use profiling to identify actual bottlenecks.

Investigate:

- CPU hotspots.
- Memory allocations.
- Lock contention.
- Serialization overhead.
- Storage I/O.
- Network processing.
- Buffer management.
- Replication overhead.

Potential tools include:

- Criterion.
- flamegraph.
- Linux performance tools.
- Memory profiling tools.

## 14.4 Optimization Rules

Follow this sequence:

1. Establish a reproducible baseline.
2. Run representative workloads.
3. Identify the bottleneck.
4. Form a technical hypothesis.
5. Implement one focused optimization.
6. Repeat the benchmark.
7. Compare the results.
8. Document the trade-offs.

Do not optimize based on assumptions.

Do not introduce unsafe Rust, SIMD, or complex memory-management techniques without a demonstrated technical reason.

## 14.5 Performance Reports

Every significant optimization should produce a report containing:

- Initial implementation.
- Workload description.
- Hardware and software environment.
- Baseline measurements.
- Identified bottleneck.
- Optimization implemented.
- Measurements after optimization.
- Interpretation of the results.
- Remaining limitations.

The objective is to demonstrate evidence-based performance engineering.

---

# 15. Project Deliverables and Evidence

The repository must provide concrete evidence of engineering progress.

Each phase should produce working software and supporting documentation.

## 15.1 Phase Deliverables

| Phase | Main deliverables |
|---|---|
| 1 | Rust workspace, domain model, in-memory engine, tests |
| 2 | Persistent storage engine, binary format, recovery, benchmarks |
| 3 | Async server, HTTP API, concurrent clients, integration tests |
| 4 | Multi-node cluster, replication, failure tests, consistency documentation |
| 5 | Profiling reports, optimized implementation, benchmark comparisons |
| 6 | Release candidate, Docker deployment, observability, CI/CD, operational documentation |

## 15.2 Technical Evidence

The repository should progressively contain:

- Working source code.
- Automated tests.
- Architecture diagrams.
- Architecture Decision Records.
- Storage format documentation.
- Replication and consistency documentation.
- Benchmark results.
- Profiling reports.
- Deployment instructions.
- Operational runbooks.
- Demonstration scenarios.

## 15.3 Demonstration Scenarios

Prepare reproducible demonstrations of the platform.

Examples:

1. Publish events to a topic.
2. Retrieve events from a partition.
3. Restart a node and recover persisted events.
4. Run multiple producers and consumers concurrently.
5. Demonstrate consumer offset persistence.
6. Replicate data between nodes.
7. Stop a node and demonstrate the implemented failure behavior.
8. Inspect metrics, logs, and traces.
9. Execute a benchmark and explain its results.

Every demonstration must reflect functionality that has actually been implemented and tested.

---

# 16. Definition of Done

A feature is not complete simply because its code compiles.

Every significant feature must satisfy the following engineering criteria.

## 16.1 Implementation

- The feature has a clearly defined purpose.
- Its responsibilities are appropriately separated.
- Its public interfaces are documented.
- Error conditions are handled explicitly.
- It follows idiomatic Rust practices.

## 16.2 Validation

- Unit tests are implemented.
- Relevant integration tests are implemented.
- Important invariants are validated.
- Failure scenarios are considered.
- Existing tests continue to pass.

## 16.3 Quality

- Formatting checks pass.
- Clippy checks pass.
- Compiler warnings are addressed.
- Dependencies are justified.
- No unnecessary unsafe code is introduced.

## 16.4 Documentation

- Relevant technical documentation is updated.
- Significant design decisions are documented.
- Known limitations are explicitly described.
- Operational instructions are updated when necessary.

## 16.5 Evidence

- The feature can be demonstrated.
- Its expected behavior can be reproduced.
- Relevant measurements are available when performance is involved.

A feature should only be considered complete when its implementation, validation, documentation, and evidence are consistent.

---

# 17. Portfolio and Technical Interview Preparation

The final objective is to demonstrate engineering competence through a substantial Rust project.

The repository should make it possible for another engineer or technical interviewer to understand the architecture, inspect the implementation, reproduce tests, and evaluate technical decisions.

## 17.1 Portfolio Presentation

The repository should clearly explain:

- What the platform does.
- Why it was developed.
- Its architectural principles.
- Its main components.
- Its implementation roadmap.
- Its technical guarantees.
- Its limitations.
- How to build and run it.
- How to execute its tests.
- How to reproduce its benchmarks.

Keep the main README focused on communicating the project.

Place detailed technical documentation in the `docs/` directory.

## 17.2 Architecture Discussion

Prepare to explain:

- Why the system is organized into its current components.
- Why the storage engine uses an append-oriented log.
- How offsets are assigned and recovered.
- How asynchronous processing works.
- How backpressure is implemented.
- How replication works.
- What happens when a node fails.
- What consistency guarantees are provided.
- Which architectural trade-offs were made.

## 17.3 Rust Technical Discussion

Prepare to demonstrate practical knowledge of:

- Ownership and borrowing.
- Lifetimes.
- Traits and generics.
- Smart pointers.
- `Send` and `Sync`.
- Async Rust.
- Tokio.
- Concurrency and synchronization.
- Error handling.
- Memory management.
- Unsafe Rust and safety invariants.
- Performance profiling.

The objective is to explain the reasoning behind the code, not merely describe Rust syntax.

## 17.4 Distributed Systems Discussion

Prepare to discuss:

- Partial failures.
- Replication.
- Leader/follower models.
- Consistency models.
- Network partitions.
- Failure detection.
- Leader election.
- Idempotency.
- At-least-once delivery.
- Recovery behavior.
- The limitations of the implemented coordination model.

Be precise about what the platform guarantees and what it does not guarantee.

## 17.5 Professional Positioning

The intended professional positioning remains:

**Senior Software Engineer | Rust | Distributed Systems | High-Performance Backend**

Relevant target roles include:

- Senior Rust Software Engineer.
- Senior Backend Engineer — Rust.
- Distributed Systems Engineer.
- Systems Software Engineer.
- High-Performance Backend Engineer.

The project is supporting evidence of Rust engineering capability. It must not be presented as commercial production experience unless such experience has actually been acquired.

---

# 18. Progress Tracking and Final Objective

## 18.1 Progress Tracking

Track progress against the six roadmap phases.

| Phase | Weeks | Planned Hours | Completion |
|---|---:|---:|---|
| 1. Rust Core & Ownership | 1–8 | 240 | Not started |
| 2. Advanced Rust & Storage Engine | 9–16 | 240 | Not started |
| 3. Async Rust & Networking | 17–24 | 240 | Not started |
| 4. Distributed Systems Engineering | 25–36 | 360 | Not started |
| 5. Performance & Systems Programming | 37–44 | 240 | Not started |
| 6. Production Readiness & Final Evaluation | 45–52 | 240 | Not started |
| **Total** | **52 weeks** | **1,560** | **Not started** |

Update the status as work progresses.

Use the following status values:

- Not started.
- In progress.
- Completed.
- Blocked.

A phase is completed only when its implementation, deliverables, and acceptance criteria have been validated.

## 18.2 Weekly Progress Review

At the end of each week, record:

- Hours invested.
- Topics studied.
- Exercises completed.
- Features implemented.
- Tests added.
- Problems encountered.
- Technical decisions made.
- Documentation updated.
- Remaining knowledge gaps.
- Priorities for the following week.

The weekly review should identify actual progress, not just time spent studying.

## 18.3 Monthly Technical Evaluation

At the end of each month, evaluate:

1. Rust language understanding.
2. Ability to implement features independently.
3. Code quality and architecture.
4. Testing and debugging capability.
5. Understanding of the relevant systems concepts.
6. Ability to explain technical decisions.
7. Progress on the main platform.

Identify weaknesses and adjust the following month's learning activities when necessary.

## 18.4 Final Objective

At the end of the 12-month programme, the objective is to have developed:

- Strong practical Rust programming skills.
- A deep understanding of ownership, borrowing, memory management, and concurrency.
- The ability to design and implement asynchronous backend services.
- Practical storage engineering experience.
- A solid understanding of distributed systems.
- Experience implementing and testing replication and failure scenarios.
- Practical performance profiling and optimization skills.
- Experience deploying and operating a distributed Rust application.
- A substantial, documented, and reproducible technical portfolio.

The central outcome is one coherent software platform developed progressively throughout the programme.

**The goal is not simply to finish 1,560 hours of study. The goal is to demonstrate that you can independently design, implement, test, optimize, and explain a sophisticated Rust backend system.**

**Final professional objective: become a highly capable Rust Software Engineer specializing in Distributed Systems, High-Performance Backend Engineering, and Systems Programming.**
