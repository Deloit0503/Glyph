# **Glyph Sparq — Runtime Specification**

> **Status:** Draft (authoritative for production execution)

This document specifies the **Sparq runtime**:
the production-grade execution engine for Glyph modules.

Sparq is responsible for **correctness, safety, determinism, and performance**.

---

## 1. Role of Sparq

Sparq is the **canonical production runtime** for Glyph.

Its responsibilities are to:

* Compile canonical AST → optimized WASM
* Load and validate `.rwm` modules
* Enforce capability-based security
* Enforce resource limits
* Execute modules deterministically
* Provide host bindings via the ABI
* Fail safely under all error conditions

Sparq is written in **Rust**.

---

## 2. Execution Modes

Sparq supports two execution modes:

### 2.1 WASM Mode (Canonical)

* Default and preferred mode
* Uses a WASM engine (e.g., wasmtime)
* Strong sandboxing guarantees
* Portable across platforms

### 2.2 Native Mode (Optional)

* Host-embedded Rust runtime
* Used when WASM overhead is unacceptable
* Must preserve WASM semantics
* Considered an optimization, not a requirement

---

## 3. Module Lifecycle

Each module instance follows this lifecycle:

1. **Load**

   * Read `.rwm`
   * Validate manifest
   * Validate signatures (if enforced)
2. **Instantiate**

   * Allocate memory
   * Bind host capabilities
   * Apply resource limits
3. **Execute**

   * Invoke exported functions
   * Handle async suspension/resumption
4. **Terminate**

   * Normal return
   * Error propagation
   * Resource exhaustion
5. **Cleanup**

   * Release memory
   * Revoke handles
   * Emit metrics

---

## 4. Validation Rules

Before execution, Sparq must validate:

* Manifest spec version compatibility
* Capability requests against host policy
* Export definitions against AST
* Resource limits sanity
* ABI compatibility
* Integrity/signatures (if enabled)

Failure at this stage prevents instantiation.

---

## 5. Capability Enforcement

* Capabilities are resolved at instantiation
* Each host function is guarded by a capability check
* No dynamic escalation is allowed
* Revocation immediately disables access

Capabilities are **runtime-enforced**, not advisory.

---

## 6. Resource Enforcement

Sparq enforces limits continuously:

### Memory

* Linear memory capped by `memory_kb`
* Growth beyond limit traps execution

### CPU

* Instruction counting or time-slicing
* Hard stop on `cpu_ms` exhaustion

### Wall Time

* Real-time timeout enforced by host

Resource exhaustion results in **immediate termination**.

---

## 7. Deterministic Execution

When enabled:

* Time, randomness, and IO are virtualized
* Execution order is fixed
* Async scheduling is deterministic
* Identical inputs produce identical outputs

Determinism is a runtime guarantee.

---

## 8. Async & Scheduling Model

* Single-threaded execution per module
* Async suspension via `await`
* Host resumes execution on promise resolution
* No shared memory concurrency
* No preemption inside module execution

---

## 9. Error Handling

Errors may arise from:

* Runtime exceptions
* Host function failures
* Resource exhaustion
* Invalid operations

Rules:

* Errors propagate to the host
* Stack traces include source spans
* Runtime never crashes the host

---

## 10. Observability & Metrics

Sparq exposes:

* Execution time
* Memory usage
* Capability usage
* Error counts

Metrics are host-consumable.

---

## 11. Security Guarantees

Sparq guarantees:

* Memory isolation
* No arbitrary host access
* No filesystem or network access without capability
* Safe termination under malicious code

Sparq treats all modules as untrusted by default.

---

## 12. Compatibility Requirements

* Sparq must support all AST versions declared compatible
* ABI compatibility is enforced at load time
* Unsupported features must fail explicitly

---

## 13. Testing & Verification

Required test categories:

* Unit tests (compiler, runtime)
* Golden tests (AST → WASM → output)
* Fuzz tests (input, ABI, resource limits)
* Security tests (sandbox escape attempts)

---

## 14. Non-Goals

Sparq does not:

* Perform JIT optimizations by default
* Manage package dependencies
* Replace host scheduling
* Expose internal runtime state

---

## 15. Invariant

> **No Glyph module may escape the guarantees enforced by Sparq.**

This invariant is non-negotiable.