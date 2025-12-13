# **Glyph — System Architecture**


This document defines the **system architecture of Glyph**:
how its parts fit together, why they are separated, and what invariants must *never* be violated.

Glyph is not a single program.
It is a **coherent system** composed of language, runtimes, tooling, and contracts.

---

## 1. Architectural Intent

Glyph exists to add a **missing layer** to modern software stacks:

> A small, safe, portable scripting and module layer that complements—not competes with—Go, Dart, and Flutter.

The architecture is intentionally:

* **Layered**
* **Spec-driven**
* **WASM-first**
* **Security-centric**
* **Tooling-aware**

Every architectural decision serves one of these goals.

---

## 2. Architectural Overview

At a high level, Glyph consists of **five orthogonal layers**:

```
┌──────────────────────────────┐
│        User & Editors        │
│     (VS Code, Neovim, CI)    │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│        Tooling Layer         │
│   (glyph CLI, rig, LSP)      │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│     Runtime Implementations  │
│   Wisp (Dart) | Sparq (Rust) │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│        Canonical Specs       │
│  Language | AST | ABI | Man.│
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│          Host Systems        │
│  Go | Flutter | Web | Games  │
└──────────────────────────────┘
```

**Key idea:**
Everything above the specs *consumes them*.
Nothing below them depends on implementation details.

---

## 3. The Canonical Core: Specifications

The **spec layer** is the heart of Glyph.

Located in:

```
glyph/spec/
```

It defines:

* Language semantics
* Canonical AST
* Module manifest
* Host ABI

### Architectural Rule #1 (Non-negotiable)

> **The specs define truth. Implementations conform.**

No runtime, tool, or editor may invent behavior not described in the specs.

---

## 4. Runtime Architecture

Glyph deliberately has **two runtimes**, each optimized for a different axis.

### 4.1 Wisp — Development Runtime (Dart)

**Location:** `glyph/wisp/`

Purpose:

* Fast iteration
* Hot reload
* Flutter integration
* REPL and debugging

Characteristics:

* AST interpreter
* No WASM
* Best-effort sandboxing
* Mirrors production semantics where possible

Wisp optimizes for **developer experience**.

---

### 4.2 Sparq — Production Runtime (Rust)

**Location:** `glyph/sparq/`

Purpose:

* Secure execution
* Determinism
* Performance
* Portability

Characteristics:

* AST → WASM compiler
* WASM-first execution
* Strict sandboxing
* Capability enforcement
* Resource quotas

Sparq optimizes for **correctness and safety**.

---

### Architectural Rule #2

> **Wisp may be forgiving. Sparq never is.**

If behavior differs, Sparq is authoritative.

---

## 5. Tooling & Orchestration

### 5.1 Glyph Toolchain (Go)

**Location:** `glyph/toolchain/`

Responsibilities:

* Build pipelines (source → AST → `.rwm`)
* Validation (specs, manifests, ABI)
* Packaging and signing
* Execution orchestration
* Registry interaction

The toolchain:

* never defines semantics
* never executes code directly
* never bypasses the runtime

It is a **control plane**, not an execution engine.

---

### 5.2 Language Server (Go)

**Location:** `glyph/lsp/`

The LSP is not just syntactic.

It is:

* spec-aware
* manifest-aware
* runtime-aware

It surfaces:

* real execution constraints
* capability mismatches
* production-only failures
* dev/prod divergence

### Architectural Rule #3

> **Editor feedback must never contradict runtime behavior.**

---

## 6. Artifact Flow (Source → Execution)

### Development Flow

```
.gl source
   ↓
Wisp parser
   ↓
Canonical AST
   ↓
Wisp interpreter
   ↓
Live execution (hot reload)
```

### Production Flow

```
.gl source
   ↓
Canonical AST
   ↓
Sparq compiler
   ↓
WASM
   ↓
.rwm (WASM + manifest)
   ↓
Sparq runtime
   ↓
Sandboxed execution
```

The **AST is the hinge** between dev and prod.

---

## 7. Module Model

Glyph modules are:

* isolated
* explicit
* capability-scoped

A module:

* cannot see the filesystem unless granted
* cannot access time or randomness implicitly
* cannot escape its sandbox
* cannot affect the host except via ABI

The manifest is **authoritative** for permissions.

---

## 8. Host Integration Model

Hosts (Go servers, Flutter apps, browsers, games):

* Provide capabilities
* Enforce policy
* Control scheduling
* Own resources

Modules:

* call into hosts
* never own host state
* never gain ambient authority

This inversion of control is intentional.

---

## 9. Security as Architecture

Security is not a feature layer.

It is enforced at:

1. **Spec level** (what is allowed)
2. **Tooling level** (what is validated)
3. **Runtime level** (what is enforced)

No single layer is trusted alone.

### Architectural Rule #4

> **If a module is malicious, the system must still be safe.**

---

## 10. Determinism & Reproducibility

When enabled:

* execution is reproducible
* async scheduling is fixed
* time/randomness are virtualized

This enables:

* reliable CI
* replayable simulations
* safe plugin execution

Determinism is an architectural capability, not an optimization.

---

## 11. Repository Architecture Mirrors System Architecture

```
spec/        → contracts (truth)
wisp/        → dev runtime
sparq/       → prod runtime
toolchain/   → orchestration
lsp/         → editor intelligence
docs/        → human guidance
examples/    → reference usage
```

### Architectural Rule #5

> **Directory boundaries reflect responsibility boundaries.**

---

## 12. What Glyph Is Not (Architecturally)

Glyph is **not**:

* a systems language
* a replacement for Go or Dart
* a general-purpose web scripting language
* a large standard library
* a monolithic runtime

Glyph succeeds by staying *small*.

---

## 13. Long-Term Architectural Stability

The architecture is designed so that:

* new runtimes can be added
* new hosts can embed Glyph
* tooling can evolve independently
* specs can version cleanly

Breaking changes are possible — but deliberate.

---

## 14. Final Architectural Invariants

These must **never** be broken:

1. Specs define semantics
2. No ambient authority
3. WASM is the production contract
4. Tooling does not weaken security
5. Editor feedback matches runtime reality

If a proposal violates any of these, it is rejected.

---

## Closing Thought

> **Glyph is successful when it disappears into the architecture of larger systems — quietly making them safer, more flexible, and more humane to build.**