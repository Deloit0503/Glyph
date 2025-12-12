# **Glyph**

> **Lightweight. Safe. Universal.**
> A WASM-first scripting language for UI, apps, and backend modules — built to complement **Go**, **Dart/Flutter**, **Templ**, **HTMX**, **Jaspr**, and **rig**.

`Glyph` is a small, embeddable, capability-secure language designed for modern full-stack development.
It provides a unified scripting and plugin model that runs everywhere — **Flutter**, **web**, **Go servers**, **edge environments**, and **game engines**.

`Glyph` is composed of three core parts:

* **Wisp** — the *Dart dev-mode interpreter* for hot reload, rapid prototyping, and seamless Flutter integration.
* **Sparq** — the *Rust AOT compiler + runtime*, producing tiny, deterministic **`.rwm`** (Glyph WASM Modules).
* **rig integration** — Go-based build, publish, sandbox, and orchestration support through the rig toolchain.

---

## ✨ Why `Glyph`?

Modern applications need a scripting layer that is:

* **Small & embeddable** — ideal for UI logic, game rules, plugins, and app extensions.
* **Safe by default** — capability-based sandboxing, resource quotas, deterministic execution.
* **WASM-native** — portable modules that run consistently across devices, servers, and browsers.
* **First-class across the Golden Stack**

  * Go backends
  * Dart + Flutter apps
  * Templ / HTMX server-side views
  * Jaspr + web frameworks
  * rig-based developer tooling
* **Developer-friendly** — hot reload, REPL, clear error messages, great tooling, and an ergonomic syntax.

`Glyph` aims to be **the modern successor to Lua** for embedded scripting, with the safety of WASM and the elegance of Dart + Go ecosystems.

---

## 🧱 Project Structure

```
spec/        → Language, AST, manifest, ABI specifications  
wisp/        → Dart interpreter + Flutter integration (dev mode)  
sparq/       → Rust compiler + runtime (production mode)  
rig/         → Go toolchain integration and CLI workflows  
lsp/         → Go LSP server for editor support  
playground/  → Web playground (TS)  
registry/    → Package registry service (optional)  
examples/    → Multi-stack examples (Flutter, Go, Templ, Jaspr, Gladiolus)  
```

---

## 🚀 Get Started (Early Development)

`Glyph` is in active early development.
To explore or contribute:

```bash
git clone https://github.com/<ORG>/glyph
cd glyph
```

### Run Wisp (dev-mode interpreter)

```bash
cd wisp
dart run bin/wisp.dart repl
```

### Build a Sparq WASM module (placeholder)

```bash
rig glyph build example.glyph
```

### Run the LSP server (Go)

```bash
go run ./lsp/cmd/glyph-lsp
```

---

## 🔭 Roadmap (High-Level)

* **MVP:** Wisp interpreter, Flutter ScriptWidget, basic AST, REPL.
* **WASM pipeline:** Rust Sparq compiler, `.rwm` packaging, host shims for Dart & Go.
* **Toolchain:** rig integration, module signing, registry publish workflows.
* **LSP:** diagnostics, completions, formatting, capability-aware warnings.
* **Playground:** browser-based WASM runner with shareable examples.
* **Examples:** comprehensive demos across the Golden Stack.

Full roadmap is maintained in `VISION.md` and `ARCHITECTURE.md`.

---

## 🔒 Security Model (Overview)

* **Capability-based imports** (explicit host-granted permissions).
* **Resource quotas** (memory, CPU time, wall time).
* **Deterministic execution** (optional).
* **Isolated WASM sandbox** (Sparq runtime).
  Details in `SECURITY.md`.

---

## 🤝 Contributing

We welcome early contributors who enjoy language design, runtimes, compilers, Flutter tooling, Go infrastructure, and Rust systems work.

Please read:

* `CONTRIBUTING.md`
* `CODE_OF_CONDUCT.md`

---

## 📜 License

MIT or Apache-2.0 (TBD). See `LICENSE.md`.
