# ADR-003 — Job Queue Strategy
**Status:** ✅ Accepted
**Decision:** PostgreSQL-backed priority queue over Kafka
**Reason:** Target scale ~1000 users/org. Kafka overkill. Postgres `SELECT FOR UPDATE SKIP LOCKED` handles concurrent-safe job picking natively.
**Watch:** Long-running Spark jobs need heartbeat/timeout column to prevent stuck job locks.
