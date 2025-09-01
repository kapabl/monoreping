# Monorepo Constitution

## 1. Purpose

This document establishes the guiding principles, rules, and responsibilities for maintaining a healthy monorepo. Its purpose is to ensure consistency, stability, and long-term maintainability across all projects.

---

## 2. Foundational Principles

1. **Single Source of Truth**: All code lives in the monorepo; no duplicated forks or hidden copies.
2. **Atomic Changes**: Cross-project changes must be made in one atomic commit to preserve consistency.
3. **Hermetic Builds**: Builds and tests must be reproducible and isolated from local developer environments.

---

## 3. Allowed Languages & Frameworks

* Only approved languages are allowed for new projects.
* Current approved set: `Java`, `Kotlin`, `Scala`, `Go`, `Python`, `TypeScript`.
* Frameworks and libraries must be reviewed before adoption to avoid fragmentation.

---

## 4. Testing & Quality

### 4.1 Anti-Flake Manifesto

* **No I/O in Unit Tests**: Unit tests must not perform disk I/O, network requests, database access, or process spawning.
* **Randomness**: Must inject fixed seeds; failures must log the seed.
* **Time Bombs**: Tests must not depend on real-world clock time. Use mocked clocks or frozen time.
* **External Services**: Must be mocked or faked, never real.

### 4.2 Test Fixtures

* Test data must be embedded (generated code or resources packaged hermetically), not loaded from arbitrary files.
* In Java/Gradle/Bazel: use `testFixtures` with code generation to embed data directly.

---

## 5. Build & CI Rules

1. **Bazel / Gradle Enforcement**: All builds must run under the supported build system.
2. **Forbidden APIs**: Dangerous APIs (e.g., `Thread.sleep`, direct `System.currentTimeMillis`) must be banned using tools like Gradle ForbiddenAPIs.
3. **CI Reliability**: Every commit must pass tests and lint checks before merging.
4. **Caching & Remote Execution**: Must be enabled to ensure speed and reproducibility.

---

## 6. Code Ownership & Review

1. Each directory must have defined `OWNERS`.
2. Changes require approval from at least one owner.
3. Cross-cutting or systemic changes require approval from infra/core maintainers.

---

## 7. Versioning & Dependencies

1. Internal dependencies should reference source, not vendored binaries.
2. External dependencies must be declared centrally, pinned to specific versions.
3. Vulnerability scans must run periodically; unsafe libraries must be blocked at introduction.
4. **Cross-Team Library Upgrades**: When a dependency bump touches multiple teams, we handle it in a simple, disciplined way:

   * All shared versions are kept in one central file, owned by Infra.
   * Upgrades are proposed by an automation job or bot, not ad‑hoc.
   * Minor and patch updates should roll quickly; majors require an RFC with justification and migration notes.
   * Rollouts happen in stages: first core infra, then gradually across the rest of the repo once signals are clean.
   * CI must prove builds and tests are green; canary deploys give extra safety.
   * Communication is key: updates are announced, deadlines are clear, and teams get support for migrations.
   * Old versions are blocked once the migration window closes.
   * We always keep a known‑good lockfile so we can roll back fast if needed.

---

## 8. Governance

1. **Stewards**: A designated group responsible for monorepo governance, RFC reviews, and enforcing build and testing rules.
2. **Decision Process**: Major changes are proposed as RFCs; consensus or steward approval required.
3. **Amendments**: Changes to this constitution require steward approval and public review.

---

## 9. Enforcement

1. CI will block violations (flake-prone tests, forbidden APIs, etc.).
2. Persistent violators will have commits reverted.
3. Teams are responsible for maintaining compliance.

---

## 10. Closing Principles

* Optimize for **long-term maintainability** over short-term speed.
* Favor **consistency** across projects to reduce cognitive load.
* Keep the bar high: the monorepo is a shared asset, not private space.
