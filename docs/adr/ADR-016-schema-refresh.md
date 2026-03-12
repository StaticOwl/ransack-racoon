# ADR-016 — Schema Refresh Strategy
**Status:** ✅ Accepted
**Decision:** Two-layer refresh — EOD daemon + submission-time check. No DB event triggers.
**Reason:** DBAs typically disallow direct DB schema connections for event triggers.
**Layer 1 — EOD Daemon:** Daily refresh, configurable timezone, updates template_metadata.
**Layer 2 — Submission check:** Lightweight dtype comparison on every job submission.
**On mismatch:** Requeue with WARNED status → refresh metadata → notify user → retry.
**Watch:** Max retry count needed. Notification system TBD.
