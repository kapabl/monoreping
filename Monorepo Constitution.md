# Monorepo Constitution

## 1. Purpose

This document establishes the guiding principles, rules, and responsibilities for maintaining a healthy monorepo. Its purpose is to ensure consistency, stability, and long-term maintainability across all projects.

---

## 2. Foundational Principles

1. **Single Source of Truth**: All code lives in the monorepo; no duplicated forks or hidden copies.
2. **Atomic Changes**: Cross-project changes must be made in one atomic commit to preserve consistency.
3. **Hermetic Builds**: Builds and tests must be reproducible and isolated from local developer environments.
4. **Transparency**: All changes are visible and traceable through version control and CI pipelines.

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
4. **Cross-Team Library Updates**: Shared dependency upgrades must follow a disciplined process:

   * **Central Ownership**: All shared versions live in a single manifest owned by Infra.
   * **Automation First**: Use bots or scripts to propose updates and fan out changes.
   * **Compatibility Rules**: Minor and patch bumps can roll forward quickly; majors require an RFC and migration plan.
   * **Ring Rollouts**: Start in core services, then expand to the rest once stable.
   * **Testing & Signals**: CI must run unit, integration, and compatibility checks; canary deploys must be monitored.
   * **Communication**: Updates are announced in weekly digests; affected teams get migration timelines and support.
   * **Deprecation & Removal**: Old versions are blocked once the migration window passes.
   * **Rollback Safety**: Always keep a last-known-good lockfile for fast revert.

---

## 8. Governance

1. **Stewards**: A rotating group of senior engineers act as monorepo stewards.
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
