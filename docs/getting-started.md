# **Getting Started with Glyph**

This guide walks you through **running your first Glyph program** using **Wisp (dev mode)** and explains how it fits into the broader Glyph toolchain.

---

## 1. What You Need

Glyph is early-stage. For now, you need:

* **Git**
* **Dart SDK** (for Wisp dev mode)
* **Go** (for tooling and LSP, optional at first)
* **Rust** (later, for Sparq)

You do **not** need Rust to get started.

---

## 2. Clone the Repository

```bash
git clone https://github.com/divijg19/glyph
cd glyph
```

---

## 3. Your First Glyph Program

Create a file:

```text
hello.gl
```

```glyph
import io

export fn main() {
  io.log("Hello, Glyph")
}
```

This defines a simple module with one exported function.

---

## 4. Run in Dev Mode (Wisp)

Navigate to the Wisp directory:

```bash
cd wisp
```

Run the Wisp REPL:

```bash
dart run bin/wisp.dart repl
```

Or execute a file:

```bash
dart run bin/wisp.dart run ../hello.gl
```

You should see:

```text
Hello, Glyph
```

This execution:

* Parses source
* Emits canonical AST
* Interprets the AST directly
* Routes `io.log` through the host

---

## 5. Hot Reload (Dev Mode)

Wisp supports hot reload.

When running in dev mode:

* Edit the `.gl` file
* Save changes
* Wisp reloads the AST and updates execution state

This is especially powerful when used inside **Flutter** via `ScriptWidget`.

---

## 6. Using Glyph with Flutter (Preview)

Wisp integrates directly with Flutter.

In a Flutter app:

```dart
ScriptWidget(
  source: 'ui.gl',
  onEvent: (event) => print(event),
)
```

This allows Glyph scripts to:

* drive UI logic
* respond to events
* hot reload live

Full examples live in:

```
wisp/examples/flutter/
```

---

## 7. Production Builds (Coming Soon)

In production, Glyph programs are:

```text
Glyph source → AST → WASM → .rwm
```

This is handled by **Sparq** and the **Glyph toolchain**.

Example (future):

```bash
glyph build hello.gl
glyph run hello.rwm
```

---

## 8. Project Layout Recap

```text
spec/        → language contracts (authoritative)
wisp/        → Dart dev interpreter
sparq/       → Rust runtime + compiler
toolchain/   → Go CLI & rig integration
lsp/         → Go language server
docs/        → guides & tutorials
examples/    → real-world usage
```

---

## 9. Where to Go Next

* Learn the language: `spec/LANGUAGE-SPEC.md`
* Understand the architecture: `ARCHITECTURE.md`
* Explore dev mode: `wisp/DEVMODE.md`
* See examples: `examples/`

---

## 10. Early-Stage Notice

Glyph is evolving.

Expect:

* breaking changes
* missing features
* sharp edges

This is the right time to contribute ideas.