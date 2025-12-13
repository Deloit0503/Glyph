# **Glyph — Roadmap**

This roadmap outlines the **intended evolution of Glyph** from early development to a stable, production-ready ecosystem.

Glyph is a long-horizon project.
The roadmap emphasizes **correct sequencing**, **spec-first development**, and **ecosystem coherence** over speed.

---

## 🧭 Guiding Principles for the Roadmap

* **Spec before implementation**
* **Dev experience before optimization**
* **Security before convenience**
* **WASM before native**
* **Tooling is part of the language**

Milestones may shift, but ordering should not.

---

## 🟢 Phase 0 — Foundation (Now)

**Goal:** Establish a solid conceptual and structural base.

### Deliverables

* Canonical repository structure
* Core documentation:

  * `VISION.md`
  * `ARCHITECTURE.md`
  * `SECURITY.md`
* Initial specs:

  * `LANGUAGE-SPEC.md`
  * `AST-SPEC.md`
  * `MANIFEST-SPEC.md`
  * `ABI-SPEC.md`
* Basic CI skeleton
* Governance files (CONTRIBUTING, CoC)

### Exit Criteria

* Clear, written definition of Glyph
* No ambiguity about ownership or architecture

---

## 🟢 Phase 1 — Wisp MVP (Dev Experience First)

**Goal:** Make Glyph *usable and delightful* in development.

### Deliverables

* Wisp (Dart) interpreter
* Parser → canonical AST
* REPL
* Hot reload
* ScriptWidget for Flutter
* Basic standard library (io.log, math, collections)
* Golden AST test suite

### Exit Criteria

* Live-editable Glyph scripts inside Flutter
* Stable AST emitted from source
* Developers can iterate without friction

---

## 🟡 Phase 2 — Sparq Alpha (WASM First)

**Goal:** Prove production viability with WASM artifacts.

### Deliverables

* Sparq compiler (AST → WASM)
* Minimal runtime using WASM engine
* `.rwm` packaging
* Manifest validation
* Go/Dart host shims
* Simple execution via toolchain

### Exit Criteria

* Same Glyph code runs in:

  * Wisp (dev)
  * WASM (prod)
* Artifacts are portable and reproducible

---

## 🟡 Phase 3 — Toolchain & LSP

**Goal:** Make Glyph feel like a real language.

### Deliverables

* `glyph` CLI (Go)
* rig integration
* Full LSP server (Go):

  * diagnostics
  * completions
  * navigation
  * manifest awareness
* Editor extensions (VS Code first)
* Deterministic build pipeline

### Exit Criteria

* End-to-end workflow:

  ```
  edit → diagnose → build → run
  ```
* Tooling reflects runtime behavior accurately

---

## 🟠 Phase 4 — Security Hardening & Performance

**Goal:** Production readiness.

### Deliverables

* Capability enforcement in runtime
* Resource quotas (CPU, memory, wall-time)
* Deterministic execution mode
* Fuzz testing
* Sandbox escape test suite
* Runtime metrics & observability

### Exit Criteria

* Untrusted modules can run safely
* Clear security guarantees documented and enforced

---

## 🟠 Phase 5 — Ecosystem & Registry

**Goal:** Enable sharing and reuse.

### Deliverables

* Module registry service
* Artifact signing
* Version resolution
* Policy enforcement
* Example libraries
* Documentation expansion

### Exit Criteria

* Modules can be published, discovered, and reused safely

---

## 🔵 Phase 6 — Stabilization (v1.0)

**Goal:** Commit to long-term compatibility.

### Deliverables

* Spec freeze (LANGUAGE, AST, ABI, MANIFEST)
* Compatibility guarantees
* Migration tooling
* Performance tuning
* Comprehensive docs & tutorials

### Exit Criteria

* Clear v1.0 promise
* Production users onboarded
* Low churn in core specs

---

## 🔵 Phase 7 — Growth & Expansion (Post–v1.0)

**Goal:** Sustainable evolution.

### Possible Directions

* Optional static typing layer
* Macro system (AST-level)
* WASM threads support
* Advanced debugging & tracing
* Certified modules
* Commercial support options

All expansion must respect v1 guarantees.

---

## 🚫 Explicit Non-Roadmap Items

Glyph does **not** plan to:

* Become a systems language
* Replace Go, Rust, or Dart
* Compete with JavaScript for general web scripting
* Accumulate a large standard library

---

## 🧠 Final Note

> **Glyph succeeds if it quietly becomes indispensable.**

Progress is measured by:

* fewer hacks
* fewer unsafe embeds
* fewer one-off DSLs
* better developer confidence