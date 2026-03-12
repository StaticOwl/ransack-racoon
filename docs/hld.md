# High Level Design (HLD)
> ransack-racoon | v0.1 | Status: Draft

---

## 1. System Overview

A multi-tenant, org-scoped data processing platform with async Spark job execution, priority-based scheduling, and clean separation of concerns across all layers.

---

## 2. System Architecture

```mermaid
graph TD
    UI[React UI / curl]

    subgraph API Layer - Play Framework
        AUTH[Auth + Validation]
        JOB_SUB[Job Submission]
        STATUS[Status Polling]
    end

    subgraph Job Queue - PostgreSQL
        QUEUE[(job_queue table\njob_id, type, priority,\nstatus, payload JSONB,\nheartbeat_at)]
    end

    subgraph Dispatcher - Daemon JVM Process
        POLLER[Queue Poller\nSELECT FOR UPDATE SKIP LOCKED]
        ROUTER[Job Router]
    end

    subgraph Spark Job Layer - Scala 2.13 + Spark 3.3.4
        DM[Data Matching]
        FUTURE[Future Jobs...]
    end

    subgraph PostgreSQL
        JQ[(job_queue)]
        JR[(job_results)]
        DOM[(domain tables)]
    end

    UI -->|REST| AUTH
    AUTH --> JOB_SUB
    JOB_SUB -->|writes job| JQ
    STATUS -->|polls| JQ

    POLLER -->|polls on interval| JQ
    POLLER --> ROUTER
    ROUTER --> DM
    ROUTER --> FUTURE

    DM -->|writes results| JR
    DQ -->|writes results| JR
    DM -->|reads/writes| DOM
```

---

## 3. Separation of Concerns

| Module | Responsibility | Knows About |
|---|---|---|
| **API Layer** | HTTP in/out, auth, validation, job submission | Queue only |
| **Dispatcher** | Queue polling, priority-based routing | Queue + Job runners |
| **Spark Jobs** | Pure computation, self-contained | Data payload only |
| **PostgreSQL** | State, results, domain data | Nothing |
| **Common** | Shared models, DB access, utilities | Used by all |

> **Key Principle:** Spark jobs are completely blind to the API. They only consume a payload and write results. Zero coupling.

---

## 4. Async Job Flow

```mermaid
sequenceDiagram
    participant Client
    participant API as API Layer (Play)
    participant DB as PostgreSQL
    participant Dispatcher as Dispatcher (Daemon)
    participant Spark as Spark Job

    Client->>API: POST /jobs {type, payload, priority}
    API->>DB: INSERT INTO job_queue (status=PENDING)
    API-->>Client: 202 Accepted {job_id}

    loop Poll every N seconds
        Dispatcher->>DB: SELECT FOR UPDATE SKIP LOCKED\nORDER BY priority
        DB-->>Dispatcher: job row
        Dispatcher->>DB: UPDATE status=RUNNING, heartbeat_at=now()
        Dispatcher->>Spark: submit job(payload)
        Spark->>DB: write results
        Dispatcher->>DB: UPDATE status=DONE
    end

    Client->>API: GET /jobs/{job_id}/status
    API->>DB: SELECT status FROM job_queue
    DB-->>API: status + result ref
    API-->>Client: {status, result}
```

---

## 5. Job Queue Schema (Draft)

```mermaid
erDiagram
    JOB_QUEUE {
        uuid job_id PK
        varchar job_type
        int priority
        varchar status
        jsonb payload
        timestamp created_at
        timestamp updated_at
        timestamp heartbeat_at
        varchar error_message
    }

    JOB_RESULTS {
        uuid result_id PK
        uuid job_id FK
        jsonb result_data
        timestamp completed_at
    }

    JOB_QUEUE ||--o| JOB_RESULTS : produces
```

---

## 6. Repo Structure (Monorepo)

```
ransack-racoon/
├── api/                  # Play Framework app
│   ├── controllers/
│   ├── services/
│   └── routes
├── dispatcher/           # Standalone daemon JVM process
│   ├── poller/
│   └── router/
├── spark-jobs/           # Pure Spark computation modules
│   ├── data-matching/
│   ├── data-quality/
│   └── common-spark/
├── common/               # Shared: models, DB access, config
│   ├── models/
│   ├── db/
│   └── config/
├── docs/                 # All design docs live here
│   └── HLD.md
└── docker-compose.yml
```

---

## 7. Technology Stack

| Layer | Technology |
|---|---|
| UI | React |
| API | Play Framework (Scala) |
| Dispatcher | Scala (standalone JVM daemon) |
| Spark Jobs | Scala 2.13.16 + Spark 3.3.4 |
| Database | PostgreSQL |
| Queue | PostgreSQL (DB-backed, `SELECT FOR UPDATE SKIP LOCKED`) |
| Build Tool | SBT (multi-module) |

---

## 8. Key Design Decisions

| Decision | Choice | Reason |
|---|---|---|
| Job Queue | Postgres-backed | Low volume, no Kafka overhead needed, ~1000 users/org |
| Dispatcher | Separate JVM daemon | Independent crash boundary, debuggable in isolation |
| Job status | API polls Postgres | Simple, no callback infra needed at this scale |
| Monorepo | Single repo, multiple modules | Shared `common`, single deploy, clean boundaries |
| Spark ↔ API coupling | Zero | Spark jobs only consume payload, write results |

---

*Next: LLD per module — start with Spark Data Matching job*