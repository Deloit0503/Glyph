# **Glyph Toolchain — rig Integration**

> **Status:** Draft (authoritative for build, packaging, and orchestration)

This document defines the **Glyph toolchain**, implemented in **Go** and integrated with **rig**.

The toolchain is responsible for turning Glyph source into runnable artifacts, validating security contracts, and orchestrating execution across environments.

---

## 1. Role of the Toolchain

The Glyph toolchain exists to:

* Build Glyph source into canonical artifacts
* Validate specs, manifests, and compatibility
* Package and sign modules
* Orchestrate execution (dev and prod)
* Provide a consistent CLI and workflows
* Integrate with registries and CI systems

The toolchain does **not** define language semantics.

---

## 2. Design Principles

* **Spec-driven** — tooling consumes `spec/`, never invents behavior
* **Deterministic** — identical inputs produce identical artifacts
* **Host-agnostic** — works for Flutter, servers, CLI, and web
* **Composable** — integrates cleanly with existing rig workflows
* **Fail-fast** — invalid modules are rejected early

---

## 3. CLI Overview

Primary commands (names illustrative):

```bash
glyph build        # compile source → .rwm
glyph dev          # run via Wisp (dev mode)
glyph run          # execute .rwm via Sparq
glyph publish      # sign and publish module
glyph inspect      # inspect AST / manifest / capabilities
glyph test         # run module tests
```

These commands are available:

* directly as `glyph`
* or namespaced under rig: `rig glyph build`

---

## 4. Build Pipeline

### 4.1 Development Build

* Source → Wisp parser
* AST emitted and cached
* Interpreted directly
* Hot reload enabled

### 4.2 Production Build

* Source → canonical AST
* AST validated against spec
* Sparq compiler invoked
* `.rwm` produced (WASM + manifest)
* Integrity metadata attached

---

## 5. Validation Responsibilities

The toolchain validates:

* Language spec version compatibility
* AST structural correctness
* Manifest schema correctness
* Capability declarations
* Export consistency
* Resource limit sanity
* ABI compatibility

Validation failures stop the pipeline.

---

## 6. Packaging Format

The toolchain produces `.rwm` artifacts:

```
example.rwm
├── module.wasm
├── manifest.json
└── source.tar.gz   (optional)
```

Packaging is deterministic and reproducible.

---

## 7. Signing & Integrity

* Toolchain may sign artifacts using local keys
* Supports multiple signatures
* Enforces integrity policy on publish/install
* Integrates with registry trust models

Signing is optional but recommended.

---

## 8. Registry Integration

The toolchain integrates with a Glyph registry to:

* Publish modules
* Fetch dependencies
* Resolve versions
* Verify signatures
* Cache artifacts locally

Registry policy is configurable.

---

## 9. Execution Orchestration

The toolchain orchestrates execution by:

* Selecting runtime (Wisp or Sparq)
* Providing host bindings
* Applying sandbox policies
* Collecting metrics
* Handling lifecycle events

The toolchain never executes code directly.

---

## 10. CI & Automation

Designed for CI use:

* Non-interactive mode
* Deterministic output
* Machine-readable diagnostics
* Artifact caching

Ideal for:

* build pipelines
* security audits
* reproducible releases

---

## 11. Extensibility

Toolchain supports extensions for:

* custom registries
* policy plugins
* alternative runtimes
* organization-specific workflows

Extensions must not bypass security guarantees.

---

## 12. Non-Goals

The toolchain does not:

* Define runtime semantics
* Perform optimization
* Manage host infrastructure
* Replace general-purpose build systems

---

## 13. Invariant

> **The toolchain must never weaken the guarantees provided by the spec or runtime.**