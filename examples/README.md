# **Glyph — Examples**

This directory contains **end-to-end examples** demonstrating how Glyph is used across the **Golden Stack** and beyond.

Examples are not demos.
They are **reference implementations** that reflect best practices, security constraints, and real integration patterns.

---

## 1. Purpose of Examples

The examples directory exists to:

* Show **how Glyph is embedded**, not just written
* Demonstrate **host–module boundaries**
* Illustrate **capability usage** in practice
* Provide **copy-paste starting points**
* Validate that the ecosystem works end-to-end

Examples must always align with the **spec and runtime behavior**.

---

## 2. Structure

```
examples/
├── flutter/
├── go-server/
├── templ-htmx/
├── jaspr/
├── gladiolus/
└── shared/
```

Each example is self-contained and documented.

---

## 3. Flutter Examples (`examples/flutter/`)

**Focus:** UI scripting, hot reload, dev experience

Demonstrates:

* Wisp integration
* `ScriptWidget`
* Live UI behavior
* Event handling
* Dev vs prod differences

Typical use cases:

* Prototyping UI logic
* Feature flags
* Animation rules
* In-app scripting

---

## 4. Go Server Examples (`examples/go-server/`)

**Focus:** Server-side plugins and extensions

Demonstrates:

* Loading `.rwm` modules
* Capability enforcement
* Request/response hooks
* Sandbox execution
* Error isolation

Typical use cases:

* Middleware plugins
* Business rule scripting
* Safe user extensions
* Automation hooks

---

## 5. Templ + HTMX Examples (`examples/templ-htmx/`)

**Focus:** Server-rendered UI augmentation

Demonstrates:

* Glyph driving UI logic
* Event handling with HTMX
* Templ rendering hooks
* Controlled server-side effects

Typical use cases:

* Conditional UI logic
* A/B testing
* Dynamic rendering rules
* Admin customization

---

## 6. Jaspr Examples (`examples/jaspr/`)

**Focus:** Dart-first web development

Demonstrates:

* Glyph modules running in the browser (WASM)
* Jaspr integration
* Shared logic between Flutter and web
* Deterministic client behavior

Typical use cases:

* Shared UI logic
* Client-side plugins
* Portable behavior modules

---

## 7. Gladiolus Examples (`examples/gladiolus/`)

**Focus:** Game scripting

Demonstrates:

* Gameplay rules in Glyph
* Event-driven logic
* Safe modding patterns
* Deterministic simulation

Typical use cases:

* Game mods
* AI behavior scripting
* Rule engines
* Replay-safe logic

---

## 8. Shared Utilities (`examples/shared/`)

Contains:

* Common Glyph modules
* Test fixtures
* Capability mocks
* Helper scripts

Used across multiple examples.

---

## 9. Quality Bar for Examples

All examples must:

* Declare capabilities explicitly
* Include a README explaining intent
* Run without undocumented setup
* Avoid dev-only shortcuts unless stated
* Match current specs and runtime behavior

Examples are part of the test surface.

---

## 10. Adding New Examples

When adding an example:

1. Choose the closest category
2. Add a README explaining:

   * what it demonstrates
   * which capabilities it needs
   * how to run it
3. Keep scope narrow
4. Update this index if needed

---

## 11. Invariant

> **If an example contradicts the spec, the example is wrong.**