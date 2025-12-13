# **Glyph Wisp — Development Mode**

> **Status:** Draft (authoritative for development-time behavior)

This document specifies **Wisp**, the **development-mode interpreter** for Glyph.

Wisp exists to optimize **developer experience**: fast iteration, hot reload, and deep Flutter integration.
It is intentionally **not** a production runtime.

---

## 1. Role of Wisp

Wisp is the **developer-facing execution engine** for Glyph.

Its responsibilities are to:

* Parse Glyph source into the canonical AST
* Interpret the AST directly
* Provide instant feedback (REPL, hot reload)
* Integrate seamlessly with Flutter
* Produce accurate source maps and diagnostics
* Mirror Sparq semantics as closely as possible

Wisp is written in **Dart**.

---

## 2. Why a Separate Dev Runtime Exists

Production runtimes optimize for:

* safety
* determinism
* performance
* sandboxing

Development workflows optimize for:

* iteration speed
* debuggability
* live feedback
* UI experimentation

Wisp exists so Sparq does not need to compromise.

---

## 3. Execution Model

* AST is interpreted directly (no WASM)
* Single-threaded execution
* Async/await supported via Dart futures
* Host effects routed through Dart bindings
* No hard resource limits by default

Semantics must match Sparq wherever possible.

---

## 4. Parsing & AST Handling

* Wisp is allowed to:

  * parse Glyph source
  * emit canonical AST
  * serialize AST for tooling

* Wisp must **not**:

  * invent semantics
  * perform optimizations
  * diverge from the spec

All parsing behavior must conform to `LANGUAGE-SPEC.md`.

---

## 5. Hot Reload Semantics

Hot reload is a first-class feature.

Rules:

* Source changes trigger re-parse → AST diff
* State may be preserved where safe
* Function bodies may be replaced live
* Hosts may choose reset vs patch semantics

Hot reload behavior is **best-effort**, not guaranteed identical to cold start.

---

## 6. Flutter Integration

Wisp provides tight Flutter integration via:

### ScriptWidget

* A Flutter widget bound to a Glyph script
* Rebuilds UI on script updates
* Allows script-driven UI logic
* Ideal for rapid prototyping and experimentation

Wisp is the **recommended way to develop Glyph-powered Flutter UIs**.

---

## 7. REPL

Wisp provides a REPL with:

* Incremental evaluation
* History
* Pretty-printed values
* Error highlighting
* Async support

The REPL uses the same interpreter as the app runtime.

---

## 8. Error Reporting

Wisp prioritizes readable errors:

* Clear messages
* Source spans
* Call stacks
* Suggestions when possible

Errors should help developers fix problems quickly.

---

## 9. Debugging Support

Wisp supports:

* Breakpoints (optional)
* Step execution (future)
* Variable inspection
* AST inspection

Debugging features are dev-only.

---

## 10. Limitations (By Design)

Wisp does **not** guarantee:

* Deterministic execution
* Strict resource enforcement
* Production-level performance
* Strong sandboxing

Developers must test production behavior via Sparq.

---

## 11. Consistency Requirements

* Wisp and Sparq must share a common test suite
* Any semantic divergence must be documented
* Wisp must fail loudly when encountering unsupported production-only features

---

## 12. Tooling Integration

Wisp integrates with:

* `rig dev`
* glyph-lsp (via AST)
* Flutter tooling (hot reload, logs)

Wisp is a **tool**, not a deployment target.

---

## 13. Invariant

> **Wisp must make Glyph joyful to use without weakening Sparq’s guarantees.**