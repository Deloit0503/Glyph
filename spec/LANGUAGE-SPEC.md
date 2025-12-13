# **Glyph — Language Specification**

> **Status:** Draft (authoritative; implementations must conform)

This document defines the **syntax, semantics, and core concepts** of the Glyph language.
It is the **canonical definition** of what Glyph programs mean.

All implementations (Wisp, Sparq, tooling, LSP) must follow this specification.

---

## 1. Scope of the Language

Glyph is a **small, embeddable scripting and module language** designed for:

* UI scripting
* Application plugins
* Game logic
* Server-side extensions
* Configuration and automation
* WASM-based portable modules

Glyph is **not** a systems language, and **not** a replacement for Go, Rust, or Dart.

---

## 2. Design Goals

Glyph prioritizes:

* Simplicity and readability
* Predictable execution
* Sandboxed, capability-based effects
* WASM portability
* Excellent developer experience

Glyph intentionally avoids:

* Implicit global state
* Ambient authority
* Hidden runtime behavior
* Large standard libraries

---

## 3. Program Structure

A Glyph program is a **module**.

A module consists of:

* Declarations
* Imports
* Exports
* Executable statements
* Optional metadata (via manifest, not source)

Example:

```glyph
import io
import math

export fn main() {
  let x = math.sqrt(16)
  io.log(x)
}
```

---

## 4. Lexical Elements

### 4.1 Identifiers

* Start with a letter or `_`
* May contain letters, digits, `_`
* Case-sensitive

Examples:

```
foo
_bar
computeValue
```

---

### 4.2 Keywords

Reserved keywords (initial set):

```
let   fn   export   import
if    else for      while
return await async
true  false null
```

Keywords may expand over time but are versioned.

---

### 4.3 Literals

Supported literals:

* Numbers (64-bit floating point)
* Strings (UTF-8)
* Booleans
* Null
* Arrays
* Maps

Examples:

```glyph
42
3.14
"hello"
true
[1, 2, 3]
{ "a": 1, "b": 2 }
```

---

## 5. Types & Values

Glyph is **dynamically typed**, with optional static hints later.

Core value types:

* Number
* String
* Boolean
* Null
* Array
* Map
* Function
* HostHandle (opaque, host-defined)

Type errors occur at runtime.

---

## 6. Variables & Binding

Variables are declared using `let`.

* Block-scoped
* Immutable by default (future: `mut` if needed)

Example:

```glyph
let x = 10
let y = x + 5
```

Shadowing is allowed.

---

## 7. Functions

Functions are first-class values.

Declaration:

```glyph
fn add(a, b) {
  return a + b
}
```

Exported function:

```glyph
export fn main() {
  // entry point
}
```

Anonymous function:

```glyph
let f = fn(x) { x * x }
```

---

## 8. Control Flow

### 8.1 Conditionals

```glyph
if x > 0 {
  io.log("positive")
} else {
  io.log("non-positive")
}
```

---

### 8.2 Loops

```glyph
for i in [1,2,3] {
  io.log(i)
}

while x < 10 {
  x = x + 1
}
```

---

## 9. Async & Await

Glyph supports **async functions** and `await`.

```glyph
export async fn fetchData() {
  let data = await net.get("https://example.com")
  return data
}
```

Concurrency is cooperative and host-mediated.

---

## 10. Imports & Capabilities

Imports are **logical**, not filesystem-based.

```glyph
import io
import net
```

Each import must correspond to:

* a declared capability in the module manifest
* a host-provided namespace

Source code cannot grant itself capabilities.

---

## 11. Exports & Entry Points

Exports define the module’s public API.

```glyph
export fn init() {}
export fn handle(event) {}
```

There is no implicit `main`; hosts decide which exported functions to invoke.

---

## 12. Error Handling

Errors are values.

* Unhandled errors propagate to host
* Hosts may map errors to exceptions or error objects

Example:

```glyph
fn risky() {
  error("something went wrong")
}
```

(Final error model defined in ABI spec.)

---

## 13. Execution Semantics

* Single-threaded execution per module instance
* No shared mutable state between modules
* All side effects go through host capabilities
* Deterministic execution unless host provides nondeterminism

---

## 14. What This Spec Does *Not* Define

This document does **not** define:

* AST structure (see `AST-SPEC.md`)
* ABI and memory layout (see `ABI-SPEC.md`)
* Module packaging (see `MANIFEST-SPEC.md`)
* Runtime enforcement details (see `RUNTIME.md`)

---

## 15. Versioning & Compatibility

* This spec is versioned
* Backward-incompatible changes increment major version
* Runtimes must declare supported spec versions

---

## 16. Guiding Rule

> **If behavior is not specified here, it is undefined.**

Implementations must not invent semantics.

