# ADR-024 — Authorization + RBAC Strategy
**Status:** ✅ Accepted
**Decision:** Fully dynamic RBAC managed by org admin
**Model:** Super Admin → Org (license) → Org Admin → Custom Roles → Users
**Rule:** Effective access = org_license ∩ role_permissions
**Org Provisioning:** Manual by Super Admin now. Self-serve + payment gateway future stream.
**Watch:** Role changes immediate — no permission caching without invalidation strategy.
