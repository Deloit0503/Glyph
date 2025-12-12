# **Glyph — Vision**

`Glyph` exists to fill a missing layer in modern software stacks:
a **small, safe, embeddable scripting and module language** that works seamlessly across **UI, backend, tooling, and the web**.

It is not a replacement for Go, Dart, or Flutter.
It is the *glue* that makes them more expressive, dynamic, and extensible.

---

## 🌍 The Problem `Glyph` Solves

Modern stacks suffer from fragmentation:

* UI logic lives in one language
* Backend plugins live in another
* Configuration is YAML or JSON
* Scripting uses ad-hoc JS, Lua, or unsafe eval
* Plugins are hard to sandbox
* Hot reload is inconsistent across layers

Existing options fail because they are either:

* too large (JavaScript),
* too unsafe (dynamic eval),
* too restrictive (Starlark),
* too low-level (Rust/C++),
* or not portable (platform-locked DSLs).

**`Glyph` is designed specifically to live in the middle.**

---

## 🎯 `Glyph`’s Purpose

`Glyph` is designed to be:

* A **scripting language** for apps, UIs, and games
* A **plugin language** for servers, tools, and frameworks
* A **WASM module language** for portable, sandboxed execution
* A **configuration + automation language** that replaces brittle YAML
* A **developer-extension layer** for Flutter, Go, and rig

`Glyph` is what you embed when:

* you want runtime extensibility without compromising safety
* you want one script to run in Flutter, Go, and the browser
* you want to ship plugins you don’t fully trust
* you want hot reload without recompiling the world

---

## 🧠 Design Philosophy

### 1. **Complement, Don’t Compete**

* Go remains the language for systems, servers, concurrency.
* Dart remains the language for UI and app logic.

### 2. **Small Things, Composed Well**

* Minimal syntax
* Minimal runtime
* Orthogonal features
* No “language magic”

### 3. **WASM Is the Contract**

* WASM is the canonical production target.
* Native execution is an optimization, not a requirement.
* Portability beats cleverness.

### 4. **Security Is a Language Feature**

* No implicit access to IO, network, or time.
* Capabilities must be declared and granted.
* Sandboxing is non-negotiable.

### 5. **Great DX Is Mandatory**

* Hot reload
* REPL
* Clear errors
* Strong tooling
* Predictable behavior

---

## 🧩 The `Glyph` Triad

Glyph is intentionally split into three cooperating systems:

### **`Glyph` (the language)**

* Syntax, semantics, standard library
* Canonical AST and module definitions

### **Wisp (dev mode)**

* Dart-based interpreter
* Hot reload and Flutter integration
* Fast iteration and prototyping

### **Sparq (production mode)**

* Rust-based compiler and runtime
* AOT → WASM
* Deterministic, sandboxed execution

This split allows:

* maximum productivity during development
* maximum safety and performance in production

---

## 🧱 Target Ecosystem

`Glyph` is designed first-class for the **Golden Stack**:

* **Go** — servers, tooling, infrastructure
* **Dart / Flutter** — UI, apps, client logic
* **Templ / HTMX** — server-rendered UI
* **Jaspr** — Dart-first web
* **rig** — build, package, orchestrate

But `Glyph` is **not limited** to this ecosystem.

Its true scope includes:

* game engines
* plugin systems
* edge computing
* WASM-based web apps
* embedded scripting in any host language

---

## 🧭 Long-Term Direction

`Glyph` aims to become:

* The **modern successor to Lua**
* The **default scripting language for WASM-first systems**
* A **safe alternative to ad-hoc JavaScript embedding**
* A **unified plugin language across frontend and backend**
* A **quiet but essential layer** in serious software systems

`Glyph` is not flashy.
It is dependable, composable, and precise.

---

## 🚫 Non-Goals

`Glyph` intentionally avoids:

* Being a general-purpose systems language
* Replacing Go, Rust, or Dart
* Large standard libraries
* Implicit global state
* Hidden runtime behavior
* Platform-specific hacks

If something belongs in the host language, it stays there.

---

## 🏁 Guiding Principle

> **`Glyph` should make the system more powerful without making it more fragile.**

That is the standard every feature must meet.
