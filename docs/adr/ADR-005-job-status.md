# ADR-005 — Job Status Strategy
**Status:** ✅ Accepted
**Decision:** API polls Postgres for job status. No push/callback mechanism.
**Reason:** Simple. No extra infra. Sufficient at this scale.
