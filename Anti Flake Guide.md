# 📘 Anti-Flake Guide

This guide provides **practical recipes** for enforcing the principles in the **Anti-Flake Manifesto**.

---

## 1. Randomness

* **Always seed RNG** in unit tests.

  * **Java**: `new Random(42)` or inject `Clock`/`Random`.
  * **Python**: `random.seed(42)`.
  * **JS/TS**: use `seedrandom` library.
* **Record seeds** on failure:

  ```java
  long seed = System.nanoTime();
  Random rng = new Random(seed);
  try {
      // test code
  } catch (AssertionError e) {
      System.err.println("Failed with seed=" + seed);
      throw e;
  }
  ```
* **CI Tip**: expose last failing seed in logs so failures are reproducible.

---

## 2. Time

* **Inject clock** into prod code, pass `FakeClock` in tests.

  * **Java**: `java.time.Clock` subclasses.
  * **Python**: `freezegun` or manual injection.
  * **Go**: interfaces wrapping `time.Now()`.
* **Ban real sleeps** in unit tests.

  * Use virtual timers, deterministic schedulers.

---

## 3. I/O and Fixtures

* Prefer **inline constants** for small JSON/SQL.
* For medium fixtures, use **codegen** to embed as constants (no runtime file I/O).
* For large/realistic data, move to **integration tests**.
* **In-memory FS**: e.g., Jimfs for Java.

---

## 4. Processes and Network

* **Unit tests** must not spawn processes or call the network.
* Use fakes/mocks:

  * **Java**: Mockito, WireMock (but only in integration scope).
  * **Python**: unittest.mock, responses.
  * **Go**: httptest.Server (integration scope).

---

## 5. Concurrency

* Use deterministic executors in unit tests.

  * **Java**: `MoreExecutors.newDirectExecutorService()` from Guava.
  * **Go**: controlled goroutine schedulers (libraries like `goleak`).
* For race detection, use specialized tools in integration, not unit.

---

## 6. CI Enforcement Patterns

* **Sandbox unit jobs**: disable network, repo read-only, only temp dirs writable.
* **Lint rules / forbidden APIs**:

  * Block `java.net.*`, `java.sql.*`, `ProcessBuilder`, etc.
  * ESLint rules for `fs`, `net`, `child_process` in JS unit tests.
* **Shuffle test order**: `junit.jupiter.testinstance.lifecycle.default=per_class` + random order.
* **Fail on flakes**: no retry-until-green; quarantine + track.

---

## 7. Classification

* **Unit**: pure, fast, deterministic, obeys Manifesto.
* **Integration**: can use real DB, network, FS, Testcontainers.
* **E2E**: full-system, realistic environment, slower.

---

## 8. Quick Recipes by Stack

**Java**

* Use `@Tag("integration")` to separate slow tests.
* Use Jimfs, FakeClock, Random(seed).
* Enforce via Gradle ForbiddenAPIs.

**Python**

* Use `pytest --maxfail=1 --disable-warnings`.
* Inject `random.seed(42)`, freeze time.
* Use fixtures instead of live I/O.

**Go**

* `t.Parallel()` for unit tests with isolated state.
* Fake clock, deterministic seeds.
* Integration tests under `+build integration` tag.

**JS/TS**

* Use `jest --runInBand --detectOpenHandles`.
* Block `fs`, `net` imports in unit scope.
* Use `seedrandom` for deterministic randomness.

---

### Closing

The **Manifesto** defines the principles; this **Guide** makes them actionable.
Together, they ensure that our test suite is **fast, reliable, and trusted**.
