# ADR-017 — Retry Strategy for Schema Validation
**Status:** ✅ Accepted
**Decision:** Source access: max 3 retries with backoff. Schema refresh: max 1 retry.
**Reason:** Source access is external API — transient failures common. Schema refresh is internal — retry once, fail fast if still broken.
**If exhausted:** Terminal FAILED status + notify user.
