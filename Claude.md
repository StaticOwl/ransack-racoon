# CLAUDE.md
> Project memory index for ransack-racoon.
> Claude must read this file + all files listed under "Document Index" before any discussion or code generation.
> When new docs are created, update the Document Index section below.

---

## 🔴 Session Start Protocol

1. Read this file fully
2. Read every file listed in "Document Index" below
3. Treat latest file versions as source of truth
4. Only then respond or generate code

---

## Project Identity

- **Name:** ransack-racoon
- **Purpose:** Multi-tenant data processing platform with async Spark job execution
- **Target Scale:** ~1000 users per org. Not a global service.
- **Current Focus:** Backend Spark jobs + API layer. UI is deprioritized.
- **Primary Language:** Scala
- **Build Tool:** Maven multi-module

---

## Document Index
> This is the live index of all project docs.
> When Claude creates or is asked to record a new doc, it must add an entry here.

| File | Type | Description | Status |
|---|---|---|---|
| `docs/HLD.md` | Architecture | Full system architecture, stack, async flow, repo structure | ✅ Active |
| `docs/adr/INDEX.md` | Decisions | ADR index — load this first, then load relevant ADRs only | ✅ Active |
| `docs/LLD-data-matching.md` | LLD | Low level design — Data Matching spark job | ✅ Active |
| `docs/LLD-dispatcher.md` | LLD | Low level design — Dispatcher daemon | ✅ Active |

---

## How Claude Updates This Index

When a new doc is created or a decision changes:
1. Add new file to Document Index table above with type, description, status
2. Mark deprecated docs as `🚫 Deprecated` in status column
3. Never remove rows — deprecate instead, so history is preserved
4. Commit `CLAUDE.md` alongside the new doc

---

## Ground Rules for Claude

1. **Never contradict an ADR** without explicitly flagging it as a proposed change first
2. **Separation of concerns is non-negotiable** — enforced at every layer
3. **Docs are source of truth** — not Claude's memory
4. **If a decision changes** — update `ADR.md` first, then proceed
5. **Always be critical and concise** — no filler, no sugar coating

---

## Stack (locked unless ADR updated)

| Layer | Technology |
|---|---|
| UI | React |
| API | Spring Boot (Java) |
| Dispatcher | Scala standalone JVM daemon |
| Spark Jobs | Scala 2.13.16 + Spark 3.3.4 |
| Database | PostgreSQL |
| Queue | PostgreSQL (`SELECT FOR UPDATE SKIP LOCKED`) |
| Build | Maven multi-module |

---

## Current Progress

- [x] HLD drafted
- [x] ADR documented
- [x] LLD — Data Matching
- [x] LLD — Dispatcher
- [ ] LLD — API Layer
- [ ] Maven multi-module setup
- [ ] Postgres schema + migrations
- [ ] Docker compose setup

> Update checkboxes as things get done.

---

## How to Use in New Chats

**Claude Code:** reads this automatically on every session. no action needed.

**claude.ai web chat:** paste this message at the start:
```
Here is my project context. Read CLAUDE.md first, then read every file listed in its Document Index.
[paste CLAUDE.md]
[paste any relevant LLD or ADR if working on specific module]
```