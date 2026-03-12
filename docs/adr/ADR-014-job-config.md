# ADR-014 — Job Configuration Strategy
**Status:** ✅ Accepted
**Decision:** Separate job_config and matching_config, both snapshotted into payload at submission
**Payload structure:**
```json
{
  "job_config_id": "uuid",
  "matching_config_id": "uuid",
  "job_config": { "...snapshot" },
  "matching_config": { "...snapshot" }
}
```
**job_config:** priority, timeout, retry policy, triggered_by
**matching_config:** sources, primary keys, value columns, matching mode
**Rule:** Payload immutable once queued. Config table changes never affect queued/running jobs.
**Watch:** UI must snapshot latest config at submission time.
