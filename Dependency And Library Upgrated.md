# Dependency & Library Upgrade Guide

## 0. Why this exists

Upgrading shared libraries should be boring, fast, and safe. This guide is the source of truth for how we do it across the monorepo.

---

## 1. Goals

* Keep security and bug fixes moving without churn
* Avoid breaking changes reaching users
* Keep changes auditable and reversible
* Minimize cross‑team coordination cost

---

## 2. Single source of truth

* All shared versions live in one place owned by Infra

  * Gradle: `gradle/libs.versions.toml`
  * Bazel JVM: `maven_install.json` or `rules_jvm_external` lock
  * Node: `pnpm-lock.yaml` and workspace `package.json` constraints
  * Python: `constraints.txt` compiled with `pip-tools`
  * Go: `go.work` plus `go.mod` in modules and a simple version map doc

Infra owns edits to these files. Everyone else consumes them.

---

## 3. What we upgrade and when

* Patch and minor: auto eligible weekly
* Major: requires an RFC and a plan
* Security fixes: priority and may bypass the usual batch window with steward approval

---

## 4. Workflow overview

1. Bot proposes upgrades in a branch
2. CI runs scoped builds and tests for affected targets
3. If clean, bot fans out PRs per owner group
4. Ring rollout through environments
5. Monitor canary signals
6. Promote or rollback

---

## 5. Automation

* Use Renovate or an internal bot. The bot must:

  * Open a PR against the central version file
  * Generate release note summaries and a risk note
  * Compute the impacted targets and owners
  * Split into small PRs per owner group if code‑mods are required
  * Add labels: `lib-upgrade`, `ring-0`, `ring-1`, `major`, `security`

---

## 6. Rings and environments

* Ring 0: samples, sandboxes, small internal services
* Ring 1: core platform services
* Ring 2: the rest

Promotion rule: do not move to the next ring until error rate, latency, and critical SLOs are stable for a full bake period.

---

## 7. Tests and checks we require

* Unit and integration for impacted targets only
* API compatibility where possible

  * JVM: `revapi` or `japicmp`
  * TS: `api-extractor` or type check reports
* Lint and static analysis
* Image build and vulnerability scan

---

## 8. Canary and metrics

* Deploy Ring 0 and Ring 1 with the new version at low traffic
* Watch error budget, p50 and p99 latency, key business metrics, and log anomalies
* Use automatic rollback on regression

---

## 9. Rollback plan

* Keep last known good lockfiles
* Revert is a single commit in the version file
* Bot opens rollback PRs for affected services
* Announce rollback in the same channels as the rollout

---

## 10. SLAs and windows

* Minor and patch: migrate within 2 weeks
* Major: migrate within 4 to 6 weeks with an RFC
* After a window closes, the old version is blocked in CI

---

## 11. Communication templates

### Announcement

* Title: Upgrade <library> to <version>
* Why: security, bug fix, or performance
* Impact: list of affected targets and owners
* Action: what teams need to do, if anything
* Timeline: start date, bake period, deadline
* Links: PRs, release notes, RFC if major

### Rollback

* Title: Rollback <library> from <version>
* Why: brief symptom and signal
* Action: none, the bot is reverting
* Next steps: issue link and follow up time

---

## 12. RFC template for major upgrades

* Summary and motivation
* Breaking changes and code‑mods
* Test strategy and compatibility checks
* Rollout plan and ring schedule
* Backout steps
* Owners and approvers

---

## 13. Exact commands and playbooks

### 13.1 Gradle (versions catalog)

* Bump in `gradle/libs.versions.toml`
* Run

```
./gradlew help
./gradlew test
./gradlew :some:project:integrationTest
```

* Check dependency updates report

```
./gradlew dependencyUpdates
```

* Optional API checks (example with japicmp)

```
./gradlew japicmp
```

### 13.2 Bazel (rules\_jvm\_external)

* Update `maven_install` in `WORKSPACE` or `MODULE.bazel`
* Refresh lock

```
bazel run @maven//:pin
```

* Build and test impacted targets only

```
bazel query 'allrdeps(kind("java_library|kt_jvm_library", //...)) intersect deps(@maven//:artifact)'
# Use the result set:
bazel test <targets>
```

### 13.3 Node (pnpm workspaces)

* Propose in central `package.json` constraints

```
pnpm up -r <pkg>@<version>
pnpm -r test
```

* Type checks

```
pnpm -r tsc -b
```

### 13.4 Python (pip-tools)

* Update `requirements.in`

```
pip-compile --upgrade -o constraints.txt requirements.in
pip install -r constraints.txt
pytest -q
```

### 13.5 Go

* For a module

```
cd path/to/module
go get example.com/lib@v1.2.3
go mod tidy
go build ./...
go test ./...
```

* If multiple modules, update `go.work` then repeat per module

---

## 14. Code‑mods

If a new version requires mechanical changes, ship a small code‑mod with the upgrade. Keep it idempotent and safe to rerun. Provide before and after examples in the PR.

---

## 15. Ownership and approval

* The central version file change requires Infra approval
* Service PRs require one owner approval from the target directory
* Major upgrade RFC requires steward approval

---

## 16. Deny list and enforcement

* Once the migration window closes, CI blocks the old version coordinates
* The bot replaces any new additions of the old version with the approved one and comments on the PR

---

## 17. Troubleshooting quick list

* Tests fail only in CI: check hermeticity and forbidden APIs
* Runtime error only in canary: compare transitive dependency graph
* Type or ABI breaks: review compatibility reports and apply the code‑mod
* Build cache misses spike: check version alignment across modules

---

## 18. Example timeline

* Day 0: Bot opens central PR and ring 0 PRs
* Day 1: CI green, deploy ring 0
* Day 2: Bake ring 0, promote to ring 1
* Day 3 to 4: Bake ring 1, promote to ring 2
* Day 7: Close rollout or rollback

---

## 19. Definition of done

* Central version updated
* All rings promoted
* Dashboards stable for the bake period
* Old version blocked in CI
* Post summary in the weekly digest
