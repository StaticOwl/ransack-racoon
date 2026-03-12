# ADR-015 — Template Metadata Strategy
**Status:** ✅ Accepted
**Decision:** Store source schema metadata at template creation, validate at submission + spark pre-flight
**Flow:**
```
Template creation → fetch schema → store in template_metadata
Job config (UI)   → reference template_metadata for column mapping
Job submission    → lightweight dtype check → mismatch: requeue + warn user
Spark pre-flight  → final validation → Either[DtypeError, MatchingConfig]
```
**Watch:** Submission check must be lightweight — dtype comparison only, not full refresh.
