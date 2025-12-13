# **Glyph — Module Manifest Specification**

> **Status:** Draft (authoritative; required for all `.rwm` modules)

This document defines the **module manifest** for Glyph.

The manifest declares **what a module is allowed to do**, independent of its source code.
It is the primary mechanism for **security, capability control, and compatibility**.

---

## 1. Purpose of the Manifest

The manifest exists to:

* Declare required **capabilities** (IO, network, rendering, etc.)
* Describe module **exports** and entry points
* Specify **resource limits** and execution hints
* Enable **static validation** before execution
* Support **secure distribution** and signing
* Decouple permissions from source code

A Glyph module **cannot grant itself authority**.

---

## 2. Manifest Format

* Format: **JSON**
* Encoding: UTF-8
* Schema: Versioned and validated
* Location inside `.rwm`:

  ```
  manifest.json
  ```

---

## 3. Top-Level Structure

```json
{
  "spec_version": "1.0",
  "module": {
    "name": "example",
    "version": "0.1.0",
    "description": "Example Glyph module"
  },
  "capabilities": [],
  "exports": [],
  "resources": {},
  "execution": {},
  "integrity": {}
}
```

All top-level fields are mandatory unless marked optional.

---

## 4. Module Metadata

### `module`

```json
"module": {
  "name": "my_module",
  "version": "1.2.3",
  "description": "Optional human-readable description"
}
```

Rules:

* `name` must be lowercase, kebab-case
* `version` follows semantic versioning
* `description` is optional

---

## 5. Capabilities

### Purpose

Capabilities define **what side effects** the module may perform.

### Structure

```json
"capabilities": [
  "io.read",
  "io.write",
  "net.fetch",
  "render.draw"
]
```

Rules:

* Capabilities are **strings**
* Namespaces are dot-separated
* Absence of a capability = **no access**
* Capabilities must be explicitly granted by the host

Examples:

* `io.read`
* `io.write`
* `net.send`
* `time.now`
* `random.secure`
* `render.texture`

---

## 6. Exports

Defines which functions are callable by the host.

```json
"exports": [
  {
    "name": "init",
    "async": false
  },
  {
    "name": "handle_event",
    "async": true
  }
]
```

Rules:

* Exported names must match AST exports
* Export metadata is declarative (no code here)
* Hosts choose which exports to invoke

---

## 7. Resource Limits

Defines execution constraints.

```json
"resources": {
  "memory_kb": 1024,
  "cpu_ms": 200,
  "wall_ms": 1000
}
```

Rules:

* All limits are **upper bounds**
* Hosts may enforce stricter limits
* Exceeding limits terminates execution

---

## 8. Execution Hints

Optional execution preferences.

```json
"execution": {
  "deterministic": true,
  "wasm": {
    "stack_kb": 64
  }
}
```

Notes:

* Hints are non-binding
* Hosts may ignore hints
* Deterministic execution disables implicit nondeterminism

---

## 9. Integrity & Signing

Used for secure distribution.

```json
"integrity": {
  "source_sha256": "abc123...",
  "signatures": [
    {
      "key_id": "maintainer-1",
      "sig": "base64..."
    }
  ]
}
```

Rules:

* `source_sha256` hashes original source bundle
* Signatures are optional but recommended
* rig enforces policy at publish/install time

---

## 10. Validation Rules

A module is **invalid** if:

* Required fields are missing
* Capabilities requested are unknown
* Exported functions do not exist
* Manifest spec version unsupported
* Integrity checks fail (when enforced)

Validation occurs:

* At build time (`rigc`)
* At publish time
* At load time

---

## 11. Relationship to Source Code

* Source code **cannot** declare capabilities
* Source code **cannot** override limits
* Manifest is authoritative for permissions

This separation is intentional.

---

## 12. Versioning & Compatibility

* `spec_version` governs manifest semantics
* Backward-incompatible changes increment major version
* Runtimes must reject unsupported versions

---

## 13. Security Invariant

> **A Glyph module can only do what its manifest explicitly allows.**

There are no exceptions.
