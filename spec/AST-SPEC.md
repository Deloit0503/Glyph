# **Glyph — Canonical AST Specification**

> **Status:** Draft (authoritative; all runtimes and tools must conform)

This document defines the **canonical Abstract Syntax Tree (AST)** for the Glyph language.

The AST is the **single source of truth** for Glyph semantics.
All implementations — **Wisp**, **Sparq**, **rig**, **glyph-lsp**, tooling, and tests — must consume or emit this AST without semantic deviation.

---

## 1. Purpose of the Canonical AST

The canonical AST exists to:

* Decouple **language semantics** from implementation details
* Allow multiple runtimes (Dart, Rust) to coexist safely
* Enable deterministic compilation and execution
* Provide a stable contract for tooling (LSP, formatters, linters)
* Prevent semantic drift across implementations

**No runtime may define its own semantics outside the AST.**

---

## 2. Representation Format

* The canonical AST is defined in **Protocol Buffers** (`ast.proto`).
* JSON serialization is permitted **only** for debugging and tooling.
* Binary Protobuf is the authoritative interchange format.

Location:

```
glyph/spec/ast.proto
```

---

## 3. AST Design Principles

1. **Explicit over implicit**
   All semantics must be encoded directly in nodes.

2. **Minimal node set**
   Prefer fewer node types with clear meaning.

3. **No syntactic sugar**
   Sugar must be desugared during parsing, not execution.

4. **Host-agnostic**
   AST contains no Dart, Rust, Go, or WASM concepts.

5. **Forward-compatible**
   Nodes are versioned; unknown fields must be ignored safely.

---

## 4. Top-Level Structure

A Glyph module AST consists of:

```text
Module
 ├── Header
 ├── Imports
 ├── Declarations
 ├── Exports
 └── Body
```

There is exactly **one Module node** per source file.

---

## 5. Core Node Types (Conceptual)

### 5.1 Module

Represents a single compilation unit.

Fields:

* `spec_version`
* `imports[]`
* `declarations[]`
* `exports[]`
* `body[]`
* `source_map`

---

### 5.2 Import

Logical import tied to a capability.

Fields:

* `name` (string)
* `alias` (optional)
* `capability_ref`

Example:

```glyph
import io
```

---

### 5.3 Declaration

Top-level bindings.

Variants:

* FunctionDeclaration
* VariableDeclaration

---

### 5.4 FunctionDeclaration

Fields:

* `name`
* `parameters[]`
* `body[]`
* `is_async`
* `source_span`

---

### 5.5 VariableDeclaration

Fields:

* `name`
* `initializer` (Expression)
* `is_mutable` (default false)

---

## 6. Statements

Statement node variants:

* Block
* If
* While
* For
* Return
* ExpressionStatement

All statements include:

* `source_span`

---

## 7. Expressions

Expression node variants:

* Literal
* Identifier
* BinaryExpression
* UnaryExpression
* CallExpression
* FunctionExpression
* AwaitExpression
* ArrayLiteral
* MapLiteral
* IndexExpression
* MemberAccess

Expressions are **pure** unless they call host functions.

---

## 8. Literals

Literal node variants:

* NumberLiteral
* StringLiteral
* BooleanLiteral
* NullLiteral

Values are immutable.

---

## 9. Control Flow Semantics

* Control flow is explicit in the AST.
* There is no implicit fallthrough.
* Loops and conditionals are represented structurally, not via jumps.

Example:

```glyph
if x > 0 { ... }
```

→ `IfStatement(condition, then_block, else_block?)`

---

## 10. Async Semantics in the AST

Async behavior is explicit.

* Async functions have `is_async = true`
* Await is represented by `AwaitExpression`
* No implicit async propagation

---

## 11. Error & Panic Nodes

Errors are explicit AST nodes.

Variants:

* ErrorExpression
* PanicStatement (optional, future)

Uncaught errors propagate to the host.

---

## 12. Source Mapping

Every AST node contains:

* `source_span { file, start, end }`

This enables:

* Accurate error messages
* LSP diagnostics
* Source-level debugging
* Stack traces across runtimes

---

## 13. Versioning & Compatibility

* AST includes `spec_version`
* New fields must be optional
* Removing fields is a breaking change
* Runtimes must declare supported AST versions

---

## 14. Forbidden in the AST

The AST must **never** include:

* Host-specific handles
* Raw pointers
* Runtime state
* Implicit globals
* Environment variables
* Filesystem paths

---

## 15. Testing Requirements

* Golden AST tests must exist for all syntax features
* AST round-trip (parse → serialize → parse) must be lossless
* All runtimes must pass the same AST test corpus

---

## 16. Invariant

> **If two runtimes consume the same AST, they must produce the same observable behavior.**

This is the core correctness rule.
