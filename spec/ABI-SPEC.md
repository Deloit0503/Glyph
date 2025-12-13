# **Glyph — Host ABI Specification**

> **Status:** Draft (authoritative; required for all runtimes and hosts)

This document defines the **Application Binary Interface (ABI)** between **Glyph modules** and their **host environments**.

The ABI specifies *how data moves*, *how functions are called*, and *how errors propagate* across the module boundary.

---

## 1. Purpose of the ABI

The ABI exists to:

* Enable safe, predictable communication between modules and hosts
* Allow multiple host languages (Go, Dart, JS) to interoperate
* Support WASM-first execution
* Prevent undefined behavior across runtime boundaries
* Provide a stable contract independent of runtime implementation

If something crosses the module boundary, it must be defined here.

---

## 2. ABI Design Principles

1. **Minimal surface area**
   Only essential primitives are supported.

2. **Explicit ownership**
   Memory ownership and lifetimes are always clear.

3. **Copy-free where possible**
   Zero-copy is preferred for byte buffers.

4. **Host-controlled effects**
   Modules cannot perform effects without host mediation.

5. **Language-agnostic**
   ABI does not expose Rust, Go, Dart, or JS types.

---

## 3. Execution Context

Each module instance executes with an **Execution Context** provided by the host.

The context includes:

* Capability table
* Resource limits
* Host function registry
* Determinism configuration
* Error handling hooks

The context is immutable during execution.

---

## 4. Supported Value Types

The ABI supports the following value types:

| ABI Type | Description                   |
| -------- | ----------------------------- |
| `i64`    | 64-bit signed integer         |
| `f64`    | 64-bit floating point         |
| `bool`   | Boolean                       |
| `string` | UTF-8 encoded string          |
| `bytes`  | Raw byte buffer               |
| `array`  | Ordered list of values        |
| `map`    | String-keyed associative map  |
| `null`   | Absence of value              |
| `handle` | Opaque host-managed reference |
| `error`  | Structured error object       |

No other types may cross the boundary.

---

## 5. Strings & Bytes

### Strings

* UTF-8 encoded
* Passed as `(pointer, length)`
* Immutable

### Bytes

* Raw byte buffers
* May be zero-copy if host allows
* Lifetime rules:

  * Host-owned unless explicitly transferred
  * Transfer semantics must be declared

---

## 6. Arrays & Maps

### Arrays

* Homogeneous or heterogeneous
* Passed as references
* Immutable from module side

### Maps

* Keys are strings
* Values are any ABI-supported type
* No implicit ordering guarantees

---

## 7. Handles

Handles represent **host-managed objects**.

Examples:

* File descriptors
* Network connections
* UI widgets
* Textures
* Database connections

Rules:

* Modules cannot inspect handle internals
* Handles are passed back to host for use
* Handles may be revoked by host at any time

---

## 8. Host Function Calls

### Invocation Model

* Modules call host functions by **name**
* Host functions are grouped by capability namespace

Example:

```
io.read(path: string) -> bytes
```

Rules:

* Call fails if capability is not granted
* Arguments are validated at boundary
* Host controls scheduling and async resolution

---

## 9. Async & Await at ABI Level

* Async calls return a **promise handle**
* Await suspends module execution
* Host resumes execution when promise resolves

No shared concurrency or threading is exposed.

---

## 10. Error Model

Errors are first-class values.

### Error Object

```json
{
  "type": "RuntimeError",
  "message": "Something went wrong",
  "details": {}
}
```

Rules:

* Errors propagate across ABI boundary
* Hosts may map errors to native exceptions
* Unhandled errors terminate module execution

---

## 11. Memory Model

* WASM linear memory is the default
* Modules cannot access host memory directly
* Hosts may expose shared memory regions explicitly
* Memory growth is bounded by manifest limits

---

## 12. Determinism Guarantees

When `execution.deterministic = true`:

* Time, randomness, and IO must be host-provided
* No implicit nondeterminism allowed
* Identical inputs must produce identical outputs

---

## 13. Versioning & Compatibility

* ABI is versioned independently
* Breaking changes increment major version
* Runtimes must reject incompatible ABI versions
* Hosts must declare supported ABI versions

---

## 14. Forbidden Behaviors

The ABI must never allow:

* Arbitrary memory access
* Host stack manipulation
* Pointer arithmetic
* File system access without capability
* Network access without capability

---

## 15. Testing Requirements

* ABI conformance tests are mandatory
* All runtimes must pass the same ABI test suite
* Fuzzing must target boundary conditions

---

## 16. Invariant

> **If two hosts implement this ABI correctly, the same module must behave identically on both.**

This guarantees portability.
