# ADR-008 — Technology Stack
**Status:** ✅ Accepted

| Layer | Technology |
|---|---|
| UI | React |
| API | Spring Boot (Java) |
| Dispatcher | Scala standalone JVM daemon |
| Spark Jobs | Scala 2.13.16 + Spark 3.3.4 |
| Database | PostgreSQL |
| Queue | PostgreSQL (`SELECT FOR UPDATE SKIP LOCKED`) |
| Build Tool | Maven multi-module |
