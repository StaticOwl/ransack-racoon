# ADR-006 — Repo Structure
**Status:** ✅ Accepted
**Decision:** Monorepo with Maven multi-module
**Structure:**
```
ransack-racoon/
├── api/            # Spring Boot Java
├── dispatcher/     # Standalone Scala JVM daemon
├── spark-jobs/     # Pure Spark computation modules
├── common/         # Shared models, DB access, config
├── docs/           # All design docs
└── docker-compose.yml
```
**Reason:** Shared common module avoids duplication. Single repo = single deploy pipeline.
