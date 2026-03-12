# ADR-021 — API Framework Switch
**Status:** ✅ Accepted
**Supersedes:** ADR-002, ADR-019, ADR-020
**Decision:** Spring Boot (Java) over Play Framework (Scala) for API layer
**Reason:** API is thin CRUD + job submission — FP benefits minimal. Spring Boot is enterprise standard. Spring Security mature. Spring Data JPA + Postgres cleaner. Tomcat compatible. Easier hiring.
**Trade-off:** Mixed language stack — Java API, Scala Spark/Dispatcher.
**Watch:** common module must be Java-compatible — no Scala-specific types in shared interfaces.
