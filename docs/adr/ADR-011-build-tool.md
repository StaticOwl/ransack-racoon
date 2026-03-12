# ADR-011 — Build Tool
**Status:** ✅ Accepted
**Supersedes:** ADR-008 (SBT entry)
**Decision:** Maven over SBT for multi-module build
**Reason:** Maven shade plugin more battle-tested for fat jar packaging. Orgs expect Maven-compatible artifacts.
**Trade-off:** Maven more verbose than SBT for Scala. Scala-Maven plugin adds config overhead.
**Watch:** Ensure scala-maven-plugin version compatible with Scala 2.13.16.
