# **Contributing to Glyph**

Thank you for your interest in contributing to **Glyph**.
Glyph is a language project with a strong emphasis on **correctness, security, and long-term maintainability**.
This document defines how contributions should be made.

---

## 1. Project Philosophy

Glyph values:

* Clear specifications over implicit behavior
* Small, composable features
* Long-term stability over short-term convenience
* Security-first design
* Thoughtful, respectful collaboration

Contributions should strengthen these values.

---

## 2. Scope of Contributions

We welcome contributions in the following areas:

* Language specification and design
* AST, ABI, and manifest evolution
* Wisp (Dart dev-mode interpreter)
* Sparq (Rust compiler and runtime)
* Tooling and rig integration (Go)
* Language Server Protocol (Go)
* Documentation and examples
* Testing, fuzzing, and CI improvements

Large features should be discussed before implementation.

---

## 3. How to Propose Changes

### Minor changes

* Documentation fixes
* Small refactors
* Test improvements

→ Open a pull request directly.

### Major changes

* Language semantics
* AST or ABI modifications
* Security model changes
* Runtime behavior changes

→ Open an issue first to discuss the proposal.

---

## 4. Specification-First Rule

> **No implementation change is accepted without a corresponding spec change.**

If a change affects:

* semantics
* behavior
* compatibility
* security

Then it must update the appropriate file in `spec/`.

Implementations follow the spec — never the reverse.

---

## 5. Development Workflow

1. Fork the repository
2. Create a feature branch from `main`
3. Make changes with tests
4. Ensure formatting and linting pass
5. Open a pull request

Pull requests should be:

* focused
* well-documented
* accompanied by tests

---

## 6. Coding Standards

### General

* Prefer clarity over cleverness
* Avoid premature optimization
* Add comments where intent is non-obvious

### Language-specific

* **Dart (Wisp):** idiomatic Dart, null-safe
* **Rust (Sparq):** idiomatic Rust, no `unsafe` without justification
* **Go (Toolchain/LSP):** standard Go style, small packages

---

## 7. Testing Requirements

All contributions must include appropriate tests:

* Unit tests for logic
* Golden tests for AST and semantics
* Regression tests for bugs
* Security tests where applicable

Changes that reduce test coverage will not be accepted.

---

## 8. Backward Compatibility

Breaking changes must:

* Be explicitly documented
* Update versioning
* Include migration notes

Unintentional breaking changes are treated as bugs.

---

## 9. Review Process

Pull requests are reviewed for:

* Correctness
* Spec compliance
* Security implications
* Maintainability
* Alignment with project goals

Reviews may request changes or additional discussion.

---

## 10. Community Conduct

All contributors must follow the Code of Conduct.

Be respectful.
Assume good intent.
Disagreements are resolved through technical reasoning.

---

## 11. Licensing

By contributing, you agree that your contributions will be licensed under the project’s license.

---

## 12. Invariant

> **No contribution may weaken Glyph’s guarantees.**