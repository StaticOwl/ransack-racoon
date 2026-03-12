# ADR-007 — Spark ↔ API Coupling
**Status:** ✅ Accepted
**Decision:** Zero coupling between Spark jobs and API layer
**Reason:** Spark jobs only consume a payload (JSONB) and write results to Postgres. Completely blind to API.
