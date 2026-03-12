# ADR-023 — Licensing Strategy
**Status:** ✅ Accepted
**Decision:** DB-backed licensing. License files ruled out.
**Schema:** org_id, features (JSONB), max_users, valid_from, valid_until, billing_status, external_billing_id
**Payment Gateway Seam:** external_billing_id + billing_status are placeholders. LicenseService is single provisioning point — manual now, webhook-driven later.
**Rule:** License checked at every request — valid + not expired + feature licensed.
**Watch:** All timestamps UTC internally.
