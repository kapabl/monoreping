# Monorepo Handbook — Table of Contents

## Part I: Core Principles

**[Monorepo Constitution](Anti-Flake Guide.md)**

1. Purpose
2. Foundational Principles
3. Allowed Languages & Frameworks
4. Testing & Quality

   * Anti-Flake Manifesto
   * Test Fixtures
5. Build & CI Rules
6. Code Ownership & Review
7. Versioning & Dependencies

   * Internal vs External Dependencies
   * Vulnerability Scans
   * Cross-Team Library Upgrades
8. Governance
9. Enforcement
10. Closing Principles

---

## Part II: Operational Guides

**[Dependency & Library Upgrade Guide](Dependency And Library Upgrated.md)**

0. Why this exists

1. Goals
2. Single Source of Truth
3. What we Upgrade and When
4. Workflow Overview
5. Automation
6. Rings and Environments
7. Tests and Checks Required
8. Canary and Metrics
9. Rollback Plan
10. SLAs and Windows
11. Communication Templates
12. RFC Template for Major Upgrades
13. Exact Commands and Playbooks

* Gradle
* Bazel
* Node (pnpm)
* Python (pip-tools)
* Go

14. Code-mods
15. Ownership and Approval
16. Deny List and Enforcement
17. Troubleshooting Quick List
18. Example Timeline
19. Definition of Done

---

## Part III: Testing Standards

**[Anti-Flake Manifesto](Anti-Flake Manifesto.md)**

* Principles to reduce flakiness
* Restrictions on I/O, randomness, time bombs, and external services

**[Anti-Flake Testing Guide](Anti-Flake Guide.md)**

* Practical steps to apply the manifesto
* Examples for Bazel and Gradle testFixtures
* Mocking, fakes, and deterministic test data
