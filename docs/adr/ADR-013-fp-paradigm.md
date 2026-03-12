# ADR-013 — Functional Programming as Core Paradigm
**Status:** ✅ Accepted
**Decision:** Strictly functional Scala in Spark jobs and dispatcher
**Enforced:**
- No `var` — immutable by default
- `Option`, `Either`, `Try` — no null, no raw exceptions
- Pure functions — no side effects in business logic
- Side effects pushed to edges (DB writes, Spark actions)
- Traits + case classes over inheritance
**Watch:** Spark is not purely functional — contain Spark side effects at job boundaries.
