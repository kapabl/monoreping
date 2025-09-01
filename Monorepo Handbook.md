# Monorepo Handbook — Table of Contents

This repository collects all parts of the Monorepo Handbook. Each section is its own markdown file so they can be browsed on GitHub.

---

## Part I: Core Principles

* [Monorepo Constitution](./Monorepo%20Constitution.md)

  * [Purpose](./Monorepo%20Constitution.md#1-purpose)
  * [Foundational Principles](./Monorepo%20Constitution.md#2-foundational-principles)
  * [Allowed Languages & Frameworks](./Monorepo%20Constitution.md#3-allowed-languages--frameworks)
  * [Testing & Quality](./Monorepo%20Constitution.md#4-testing--quality)

    * [Anti-Flake Manifesto](./Monorepo%20Constitution.md#41-anti-flake-manifesto)
    * [Test Fixtures](./Monorepo%20Constitution.md#42-test-fixtures)
  * [Build & CI Rules](./Monorepo%20Constitution.md#5-build--ci-rules)
  * [Code Ownership & Review](./Monorepo%20Constitution.md#6-code-ownership--review)
  * [Versioning & Dependencies](./Monorepo%20Constitution.md#7-versioning--dependencies)
  * [Governance](./Monorepo%20Constitution.md#8-governance)
  * [Enforcement](./Monorepo%20Constitution.md#9-enforcement)
  * [Closing Principles](./Monorepo%20Constitution.md#10-closing-principles)

## Part II: Operational Guides

* [Dependency & Library Upgrade Guide](./Dependency%20and%20Library%20Upgrade%20Guide.md)

  * [Why this exists](./Dependency%20and%20Library%20Upgrade%20Guide.md#0-why-this-exists)
  * [Goals](./Dependency%20and%20Library%20Upgrade%20Guide.md#1-goals)
  * [Single Source of Truth](./Dependency%20and%20Library%20Upgrade%20Guide.md#2-single-source-of-truth)
  * [What we Upgrade and When](./Dependency%20and%20Library%20Upgrade%20Guide.md#3-what-we-upgrade-and-when)
  * [Workflow Overview](./Dependency%20and%20Library%20Upgrade%20Guide.md#4-workflow-overview)
  * [Automation](./Dependency%20and%20Library%20Upgrade%20Guide.md#5-automation)
  * [Rings and Environments](./Dependency%20and%20Library%20Upgrade%20Guide.md#6-rings-and-environments)
  * [Tests and Checks Required](./Dependency%20and%20Library%20Upgrade%20Guide.md#7-tests-and-checks-we-require)
  * [Canary and Metrics](./Dependency%20and%20Library%20Upgrade%20Guide.md#8-canary-and-metrics)
  * [Rollback Plan](./Dependency%20and%20Library%20Upgrade%20Guide.md#9-rollback-plan)
  * [SLAs and Windows](./Dependency%20and%20Library%20Upgrade%20Guide.md#10-slas-and-windows)
  * [Communication Templates](./Dependency%20and%20Library%20Upgrade%20Guide.md#11-communication-templates)
  * [RFC Template for Major Upgrades](./Dependency%20and%20Library%20Upgrade%20Guide.md#12-rfc-template-for-major-upgrades)
  * [Exact Commands and Playbooks](./Dependency%20and%20Library%20Upgrade%20Guide.md#13-exact-commands-and-playbooks)

    * [Gradle](./Dependency%20and%20Library%20Upgrade%20Guide.md#131-gradle-versions-catalog)
    * [Bazel](./Dependency%20and%20Library%20Upgrade%20Guide.md#132-bazel-rules_jvm_external)
    * [Node (pnpm)](./Dependency%20and%20Library%20Upgrade%20Guide.md#133-node-pnpm-workspaces)
    * [Python (pip-tools)](./Dependency%20and%20Library%20Upgrade%20Guide.md#134-python-pip-tools)
    * [Go](./Dependency%20and%20Library%20Upgrade%20Guide.md#135-go)
  * [Code-mods](./Dependency%20and%20Library%20Upgrade%20Guide.md#14-code-mods)
  * [Ownership and Approval](./Dependency%20and%20Library%20Upgrade%20Guide.md#15-ownership-and-approval)
  * [Deny List and Enforcement](./Dependency%20and%20Library%20Upgrade%20Guide.md#16-deny-list-and-enforcement)
  * [Troubleshooting Quick List](./Dependency%20and%20Library%20Upgrade%20Guide.md#17-troubleshooting-quick-list)
  * [Example Timeline](./Dependency%20and%20Library%20Upgrade%20Guide.md#18-example-timeline)
  * [Definition of Done](./Dependency%20and%20Library%20Upgrade%20Guide.md#19-definition-of-done)

## Part III: Testing Standards

* [Anti-flake Manifesto](./Anti-flake%20Manifesto.md)

  * [Rules](./Anti-flake%20Manifesto.md#core-rules)
  * [Enforcement](./Anti-flake%20Manifesto.md#enforcement)
  * [Language Notes](./Anti-flake%20Manifesto.md#language-notes)
  * [Test Data Policy](./Anti-flake%20Manifesto.md#test-data-policy)
  * [Exceptions](./Anti-flake%20Manifesto.md#exceptions)

* [Anti-flake Testing Guide](./Anti-flake%20Testing%20Guide.md)

  * [Clock control](./Anti-flake%20Testing%20Guide.md#1-clock-control)
  * [Replace sleeps](./Anti-flake%20Testing%20Guide.md#2-replace-sleeps)
  * [Data without I/O](./Anti-flake%20Testing%20Guide.md#3-data-without-io)
  * [External services mocks/fakes](./Anti-flake%20Testing%20Guide.md#4-external-services)
  * [Randomness policy](./Anti-flake%20Testing%20Guide.md#5-randomness-policy)
  * [Concurrency hygiene](./Anti-flake%20Testing%20Guide.md#6-concurrency-hygiene)
  * [Source set layout](./Anti-flake%20Testing%20Guide.md#7-source-set-layout)
  * [CI enforcement](./Anti-flake%20Testing%20Guide.md#8-ci-enforcement)
  * [Examples](./Anti-flake%20Testing%20Guide.md#examples)
  * [Checklist](./Anti-flake%20Testing%20Guide.md#checklist-before-merging)
