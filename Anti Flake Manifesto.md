# 🧪 The Anti-Flake Manifesto

### Preamble

Flaky tests erode trust, waste engineering time, and slow down innovation.
A unit test is **fast**, **deterministic**, and **isolated**. Anything else belongs in integration or end-to-end.
This manifesto defines how we keep unit tests stable in the monorepo.

---

## 1. Hard “No” for Unit Tests

Unit tests **must not**:

* Touch the **network**: HTTP, gRPC, DNS, message queues, caches.
* Touch the **database**: relational, NoSQL, migrations, ORMs against live engines.
* Do **I/O**: disk, sockets, pipes, cloud SDKs.
* Spawn **processes**: shells, forks, `exec`, subprocess/child\_process.
* Depend on **time**: wall-clock, sleeps, timers.
* Depend on **randomness**: unseeded RNG, UUID v4, crypto nonces. Failures should report the seed for reproducibility.
* Rely on **global state**: environment variables, singletons, static caches.
* Depend on **platform quirks**: locale, timezone, filesystem case sensitivity.
* Be **resource-heavy**: huge fixtures, memory hogs, long loops.

---

## 2. Allowed, With Strict Controls

* **Filesystem**: only in-memory FS (e.g., Jimfs) or ephemeral temp dirs auto-cleaned.
* **Randomness**: fixed seed injected.
* **Time**: fake clock advanced programmatically.
* **Concurrency**: deterministic schedulers or test harnesses.
* **Floating point**: use epsilon, never exact equality.
* **Logs/STDOUT**: normalize; avoid fragile timestamp/PID asserts.

---

## 3. Test Hygiene

* **Isolation**: no hidden coupling; reset globals/state after each test.
* **Determinism**: fixed `TZ=UTC` and locale (`en_US` or `C`).
* **Clear intent**: one behavior per test; descriptive names.
* **Speed**: target <100 ms per test; >500 ms → integration.
* **Minimal fixtures**: inline constants or generated code; avoid runtime file I/O.

---

## 4. CI Enforcement

* **Sandbox**: unit test jobs run with network disabled, repo read-only, temp dirs only.
* **Lint rules**: deny imports of `net/fs/subprocess/db` in unit test sources.
* **Env discipline**: scrub env vars; set fixed TZ and locale.
* **Randomization**: shuffle test order; record seeds.
* **Quarantine flakes**: first confirmed flake → quarantine and track to closure; no “retry until green.”
* **Budgets**: report slowest tests; fail if median or p95 exceeds budget.

---

## 5. Test Classes

* **Unit**: obeys this Manifesto.
* **Integration**: may use I/O, DB, network; runs in separate jobs.
* **E2E/Smoke**: full system; slower but stable; separate pipeline.

---

## 6. PR Checklist

* [ ] No I/O, DB, network, or process calls in unit tests.
* [ ] Time faked, randomness seeded, globals reset.
* [ ] Deterministic under `TZ=UTC`, fixed locale, shuffled order.
* [ ] Slow/external tests tagged as **integration**, not unit.

---

### Closing

A test suite is a safety net. Flaky tests are holes in that net.
By following the **Anti-Flake Manifesto**, we guarantee speed, trust, and flow — enabling engineers to innovate boldly without fear.

---

👉 For concrete recipes and enforcement patterns, see the **Anti-Flake Guide**.
