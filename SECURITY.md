# **Glyph — Security Model**

This document defines the **security model** of Glyph.

Security is not an afterthought in Glyph.
It is a **core language and runtime property**, enforced consistently across tooling, runtimes, and hosts.

---

## 1. Threat Model

Glyph assumes:

* **Modules are untrusted by default**
* Authors may be malicious or compromised
* Modules may be executed in sensitive environments
* Hosts must remain secure under all module behavior

Glyph is designed to **fail safe**.

---

## 2. Security Goals

Glyph guarantees:

* **Isolation** — modules cannot affect host state except through explicit capabilities
* **Least privilege** — no ambient authority
* **Determinism (optional)** — reproducible execution
* **Resource containment** — no denial-of-service via runaway execution
* **Portability of guarantees** — same security properties across platforms

---

## 3. Capability-Based Security

### Core Principle

> **Modules can only do what the host explicitly allows.**

Capabilities are:

* Declared in the module manifest
* Granted by the host at instantiation
* Enforced at runtime (Sparq)
* Visible to tooling (LSP, rig)

Examples:

* `io.read`
* `net.fetch`
* `render.draw`
* `time.now`

No capability → no access.

---

## 4. No Ambient Authority

Glyph intentionally provides **no default access** to:

* Filesystem
* Network
* Time
* Randomness
* Environment variables
* Host memory
* System calls

All such access must go through:

1. A declared capability
2. A host-provided function
3. ABI validation

---

## 5. Sandboxing & Isolation

### Runtime Isolation (Sparq)

* WASM-based memory isolation
* No pointer access outside module memory
* No shared memory by default
* No host stack access
* No native syscalls

### Development Mode (Wisp)

* Best-effort isolation
* Strong warnings for dev-only behavior
* No claim of production-grade sandboxing

---

## 6. Resource Limits

Each module executes under strict limits:

* **Memory** — linear memory capped
* **CPU** — instruction or time budget
* **Wall time** — real-time timeout
* **Handles** — bounded host resource usage

Limits are:

* Declared in the manifest
* Enforced continuously
* Non-negotiable at runtime

Exceeding limits terminates execution immediately.

---

## 7. Deterministic Execution

When enabled:

* Time and randomness are virtualized
* IO is host-controlled
* Async scheduling is fixed
* No hidden sources of nondeterminism

This enables:

* Reproducible builds
* Reliable testing
* Deterministic CI runs

---

## 8. Error Containment

Errors are:

* First-class values
* Propagated safely to host
* Never allowed to crash the host

Uncaught errors result in:

* Module termination
* Resource cleanup
* Structured error reporting

---

## 9. Supply Chain Security

Glyph supports:

* Signed `.rwm` artifacts
* Verified source hashes
* Registry trust policies
* Offline verification

The toolchain (`rig`) enforces policy at:

* Publish time
* Install time
* Load time

---

## 10. Tooling Enforcement

Security is enforced across layers:

* **Spec** — defines what is allowed
* **Toolchain** — validates contracts early
* **LSP** — surfaces violations during editing
* **Runtime** — enforces at execution

No single layer is trusted alone.

---

## 11. Forbidden Behaviors

Glyph explicitly forbids:

* Arbitrary memory access
* Self-granted capabilities
* Dynamic privilege escalation
* Host code execution
* Reflection into host internals
* JIT or native code generation inside modules

---

## 12. Auditing & Testing

Required practices:

* Fuzz testing of runtime and ABI
* Sandbox escape tests
* Manifest tampering tests
* Regression tests for vulnerabilities

Security fixes are backported aggressively.

---

## 13. Responsible Disclosure

Security issues should be reported privately.

A coordinated disclosure process will be documented as the project matures.

---

## 14. Invariant

> **Glyph must remain safe even when executing hostile code.**

This invariant overrides performance and convenience.