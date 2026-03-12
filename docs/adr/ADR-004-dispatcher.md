# ADR-004 — Dispatcher Architecture
**Status:** ✅ Accepted
**Decision:** Dispatcher as standalone Scala JVM daemon process, NOT embedded in API app
**Reason:** Independent crash boundary. API crash doesn't kill dispatcher. Debuggable in isolation.
**Deployment:** Same repo, separate process. Managed via docker-compose or systemd.
