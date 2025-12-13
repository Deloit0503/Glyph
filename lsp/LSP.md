# **Glyph Language Server (LSP)**

> **Status:** Draft (authoritative for editor and IDE integration)

This document defines the **Glyph Language Server**, which provides editor intelligence for Glyph source code.

The LSP is a **first-class component** of the Glyph ecosystem and is designed to reflect **runtime-aware semantics**, not just syntax.

---

## 1. Role of the LSP

The Glyph LSP exists to:

* Provide rich editor support (diagnostics, completions, navigation)
* Enforce spec and manifest correctness at edit time
* Surface runtime-aware errors and warnings
* Integrate tightly with the Glyph toolchain (`rig`)
* Improve developer productivity across editors

The LSP is implemented in **Go**.

---

## 2. Design Principles

* **Spec-driven** — the LSP consumes canonical AST and manifest specs
* **Runtime-aware** — errors reflect Sparq/Wisp behavior, not just syntax
* **Fast & incremental** — optimized for continuous editing
* **Editor-agnostic** — any LSP-compatible editor is supported
* **Single binary** — easy installation and CI usage

---

## 3. Architecture Overview

The LSP consists of:

* **Frontend protocol layer**

  * Implements LSP over stdio or TCP
* **Analysis engine**

  * Parses source → AST
  * Validates AST against spec
  * Reads and validates manifests
* **Toolchain bridge**

  * Calls `rig` internals for:

    * dependency resolution
    * registry metadata
    * capability definitions
* **Index & cache**

  * Incremental AST cache
  * Symbol index
  * Manifest cache

---

## 4. Core Capabilities

### 4.1 Diagnostics

* Syntax errors
* Spec violations
* Invalid imports or exports
* Missing or invalid capabilities
* Manifest mismatches
* Unsupported language or ABI versions

Diagnostics are:

* precise (source-mapped)
* actionable
* consistent with runtime behavior

---

### 4.2 Code Navigation

* Go-to-definition
* Find references
* Symbol outline
* Document symbols
* Workspace symbols

---

### 4.3 Completions

* Keywords
* Built-ins
* Imported module members
* Host-provided capability functions
* Export suggestions

Completions are context-aware and filtered by granted capabilities.

---

### 4.4 Hover & Documentation

* Inline documentation for:

  * language constructs
  * built-ins
  * host APIs
  * imported symbols
* Links to spec sections where applicable

---

### 4.5 Refactoring & Actions

* Rename symbols
* Auto-import suggestions
* Fix-its for common errors
* Manifest sync actions (e.g. add missing capability)

---

## 5. Manifest Awareness

The LSP treats the manifest as **part of the program**.

Features include:

* Real-time validation of `manifest.json`
* Warnings when source code uses undeclared capabilities
* Suggestions to update manifests
* Version compatibility checks

---

## 6. Runtime Awareness

The LSP is aware of:

* Wisp dev-mode semantics
* Sparq production constraints
* Deterministic vs nondeterministic execution modes
* Resource limits

This enables:

* Warnings for dev-only features
* Hints about production incompatibilities
* Clear separation of dev vs prod expectations

---

## 7. Integration with rig

The LSP integrates with rig to:

* Resolve dependencies
* Query registry metadata
* Validate module versions
* Surface policy violations

The LSP does not shell out to `rig`; it uses shared Go libraries.

---

## 8. Editor Clients

* **VS Code** — primary supported editor
* **Neovim / Vim** — via LSP clients
* **Others** — any LSP-compatible editor

Editor extensions are thin wrappers that:

* install/start the Go LSP binary
* handle UI-specific features

---

## 9. Performance Considerations

* Incremental parsing
* AST reuse across files
* Bounded memory usage
* Parallel analysis where safe

The LSP must remain responsive for large projects.

---

## 10. Testing Strategy

* LSP protocol tests
* Golden diagnostic tests
* Manifest/source mismatch tests
* Editor integration tests (VS Code, headless)

---

## 11. Non-Goals

The LSP does not:

* Execute Glyph code
* Perform compilation
* Replace runtime error reporting
* Manage project builds

---

## 12. Invariant

> **Editor feedback must never contradict runtime behavior.**