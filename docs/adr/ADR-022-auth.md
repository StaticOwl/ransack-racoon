# ADR-022 — Authentication Strategy
**Status:** ✅ Accepted
**Decision:** OAuth2.0 SSO. No JWT-first, no API keys.
**Reason:** Enterprise orgs have existing IDPs (Okta, Azure AD, Google). SSO is the standard.
**Implementation:** Spring Security OAuth2.0 Resource Server.
**Watch:** Multiple IDP configurations needed per org.
