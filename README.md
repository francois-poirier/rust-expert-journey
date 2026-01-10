# 🦀 Rust Expert Roadmap (8–10 Months) – Week-by-Week

**Goal:** Become a production-grade Rust expert capable of building **high-performance backend systems** and producing a portfolio suitable for senior/staff interviews.

**Time commitment:** ~10–15 hours/week (40 weeks)

**Resources:**

* *The Rust Programming Language (TRPL, 2nd Ed.)*
* *Programming Rust (O’Reilly, 2nd Ed.)*
* *Rust for Rustaceans*
* *Effective Rust (O’Reilly)*
* *Async Rust: Unleashing the Power of Fearless Concurrency*
* *Code Like a Pro in Rust (Manning)*
* *The Rust Programming Handbook Pack*

---

📦 Repo Structure

```csharp

rust-expert-journey/
│
├─ README.md                  # This roadmap + instructions
├─ Cargo.toml                  # Root workspace Cargo.toml
├─ rust-toolchain.toml         # Optional, pinned Rust version
│
├─ projects/
│   ├─ rust-cli-toolkit/
│   ├─ in-memory-store/
│   ├─ rust-http-service/
│   ├─ async-task-engine/
│   ├─ low-latency-core/
│   └─ rust-event-backend/
│
├─ benchmarks/                 # Optional benchmark scripts
├─ docs/                       # Diagrams & architecture
└─ .github/workflows/          # Optional CI/CD
```
---

## 🗺️ Phase Overview

| Phase   | Weeks | Focus                              |
| ------- | ----- | ---------------------------------- |
| Phase 1 | 1–6   | Fundamentals & Ownership           |
| Phase 2 | 7–12  | Memory, Lifetimes & Data Modeling  |
| Phase 3 | 13–18 | Error Handling, APIs & Testing     |
| Phase 4 | 19–24 | Concurrency & Async Rust           |
| Phase 5 | 25–30 | Performance & Systems Design       |
| Phase 6 | 31–40 | Capstone: High-Performance Backend |

---

## Phase 1 — Fundamentals & Ownership (Weeks 1–6)

**Primary reading:**

* TRPL Ch.1–4 (Getting Started, Programming Concepts, Ownership)
* Programming Rust Ch.1–3 (Rust Basics, Ownership & Borrowing, Variables & Mutability)

**Project:** `rust-cli-toolkit` – CLI file scanner/log analyzer

**Weekly Plan & Definition of Done:**

* **Week 1:** Install Rust, Cargo, write “Hello Rust” (TRPL Ch.1, PR Ch.1)
* **Week 2:** Ownership & moves (TRPL Ch.2)
* **Week 3:** References & borrowing (TRPL Ch.3)
* **Week 4:** Structs, enums, pattern matching (TRPL Ch.4, PR Ch.2)
* **Week 5:** Modules & visibility (PR Ch.3)
* **Week 6:** Collections & iterators (TRPL Ch.8)

**DoD:**

* No unnecessary cloning
* Zero compiler warnings
* Uses `Result<T, E>` for error handling
* README explains ownership decisions

---

## Phase 2 — Memory, Lifetimes & Data Modeling (Weeks 7–12)

**Primary reading:**

* TRPL Ch.10, 19 (Generics, Advanced Traits)
* Programming Rust Ch.4–6 (Lifetimes, Structs/Enums, Generics)
* Rust for Rustaceans Ch.1–2 (Advanced Ownership, Lifetimes)

**Project:** `in-memory-store` – in-memory key/value engine

**Weekly Plan & Definition of Done:**

* **Week 7:** Lifetimes intro (PR Ch.4)
* **Week 8:** Structs & memory layout (PR Ch.5)
* **Week 9:** Generics & trait bounds (PR Ch.6)
* **Week 10:** Ownership patterns in complex systems (RfR Ch.1)
* **Week 11:** Lifetime elision & advanced patterns (RfR Ch.2)
* **Week 12:** Benchmarks & memory layout review

**DoD:**

* Explicit lifetimes used correctly
* No `Rc<RefCell<_>>` without reason
* Benchmarks via `criterion`
* Memory layout documented

---

## Phase 3 — Error Handling, APIs & Testing (Weeks 13–18)

**Primary reading:**

* TRPL Ch.9, 11 (Error Handling, Testing)
* Effective Rust Ch.3–4 (API design, error propagation)
* Rust for Rustaceans Ch.6 (Idiomatic API design)

**Project:** `rust-http-service` – minimal HTTP service using Axum or Actix

**Weekly Plan & DoD:**

* Week 13: Basic error handling (`Result`, `Option`)
* Week 14: Unit tests (TRPL Ch.11)
* Week 15: API ergonomics (ER Ch.3)
* Week 16: Error propagation (`?`, combinators) (ER Ch.4)
* Week 17: Integration testing & coverage
* Week 18: Project completion & review

**DoD:**

* No `unwrap()` in production
* All errors wrapped in enums
* Unit & integration tests complete
* API ergonomics documented

---

## Phase 4 — Concurrency & Async Rust (Weeks 19–24)

**Primary reading:**

* Async Rust Ch.1–6 (Futures, Tokio basics, async patterns)
* Programming Rust Ch.16–17 (Threads & concurrency)
* Effective Rust Ch.5 (Concurrency best practices)

**Project:** `async-task-engine` – concurrent task processing engine

**Weekly Plan & DoD:**

* Week 19: Async foundations, Futures, Tokio basics
* Week 20: Async I/O, tasks, executor model
* Week 21: Multi-threaded programming, Send & Sync
* Week 22: Channels, mutexes, atomics
* Week 23: Async patterns & error handling
* Week 24: Project completion

**DoD:**

* No blocking in async code
* Correct `Send`/`Sync` usage
* Load tests documented
* Concurrency design explained

---

## Phase 5 — Performance & Systems Design (Weeks 25–30)

**Primary reading:**

* Code Like a Pro in Rust Ch.1–7 (Zero-copy, memory layouts, SIMD)
* Effective Rust Ch.6 (Performance tuning, benchmarking)
* Rust for Rustaceans Ch.7 (Memory & cache optimization)

**Project:** `low-latency-core` – high-performance library for low-latency data structures

**Weekly Plan & DoD:**

* Week 25: Profiling & flamegraphs
* Week 26: Memory layouts & cache optimization
* Week 27: Zero-copy & borrow optimizations
* Week 28: SIMD & algorithm optimization
* Week 29: Unsafe optimizations & justification
* Week 30: Project completion

**DoD:**

* Benchmarks for P50/P99 latency
* Allocation count measured
* Unsafe code justified
* Design documented

---

## Phase 6 — Capstone: High-Performance Backend (Weeks 31–40)

**Primary reading:**

* Code Like a Pro in Rust Ch.8–10
* Async Rust Ch.7–10
* Programming Rust Ch.18

**Project:** `rust-event-backend` – high-throughput, low-latency event ingestion & processing backend

**Weekly Plan & DoD:**

* Week 31: Architecture & design
* Week 32: Ingestion layer (TCP/HTTP)
* Week 33: Zero-copy parsing & data flow
* Week 34: Async processing pipeline
* Week 35: Lock-free / low-contention queues
* Week 36: Metrics, tracing, backpressure
* Week 37: Benchmarking & profiling
* Week 38: Optimizations & code review
* Week 39: Documentation & diagrams
* Week 40: Final polish & deployment

**DoD:**

* Architecture diagram included
* P99 latency & throughput measured
* Trade-offs explained
* Unit & integration tests complete

---

## 🎯 Interview Mapping (Questions → Repo → Code)

| Topic                 | Question                               | Repo / File                        | Notes                                          |
| --------------------- | -------------------------------------- | ---------------------------------- | ---------------------------------------------- |
| Ownership & Borrowing | “How does Rust prevent data races?”    | `in-memory-store/src/store.rs`     | Compile-time guarantees, zero runtime overhead |
| Lifetimes             | “When do you need explicit lifetimes?” | `low-latency-core/src/buffer.rs`   | Lifetime-bound APIs                            |
| Error Handling        | “How do you design errors in Rust?”    | `rust-http-service/src/error.rs`   | Custom enums, `thiserror`                      |
| Async                 | “What are common async Rust pitfalls?” | `async-task-engine/src/worker.rs`  | Blocking vs non-blocking                       |
| Performance           | “How do you reduce allocations?”       | `rust-event-backend/src/parser.rs` | Zero-copy & borrow design                      |
| Unsafe Rust           | “When is unsafe acceptable?”           | `low-latency-core/src/arena.rs`    | Safety invariants documented                   |
