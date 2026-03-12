# ADR-009 — Documentation Strategy
**Status:** ✅ Accepted
**Decision:** Markdown + Mermaid in /docs. ADRs split into individual files with INDEX.md.
**Reason:** Docs = portable memory across chats and tools. GitHub renders Mermaid natively. Individual ADR files reduce token usage per session — load only what's relevant.
**Files:** CLAUDE.md → entry point. adr/INDEX.md → ADR index. HLD.md → architecture. LLD-{module}.md → per module.
