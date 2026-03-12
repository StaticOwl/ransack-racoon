# ADR-018 — Whole Record Matching Strategy
**Status:** ✅ Accepted
**Decision:** Row hash comparison over column-by-column equality
**Reason:** Single O(n) pass per dataset. Column equality requires multiple passes — slower at scale.
**Trade-off:** Hash doesn't identify which column mismatched — that's CellByCell's job.
**Watch:** Sort columns before hashing — deterministic order prevents false mismatches.
