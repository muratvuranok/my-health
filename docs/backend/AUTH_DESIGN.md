Status: DRAFT
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Authentication specification and security acceptance
Supported: Google OIDC, Sign in with Apple, email/password, email magic link. Gmail is Google OIDC, not a separate provider.

## Endpoint contract (draft; implementation must supply OpenAPI and tests)
POST /v1/auth/email/signup {email,password,locale} -> 202 generic verification queued; POST /v1/auth/email/verify/request {email} -> 202; POST /v1/auth/email/verify/confirm {token} -> verified status (PROPOSED: 204 No Content; final response contract OPEN pending owner approval); POST /v1/auth/email/login {email,password} -> tokens; POST /v1/auth/password/reset/request {email} -> 202; POST /v1/auth/password/reset/confirm {token,new_password} -> 204.
POST /v1/auth/magic/request {email,redirect_target} -> 202 generic; POST /v1/auth/magic/exchange {token,confirmation} -> session; POST /v1/auth/oauth/google {authorization_code,code_verifier,redirect_uri,nonce} -> session; POST /v1/auth/oauth/apple {authorization_code,code_verifier?,redirect_uri,nonce} -> session. OAuth flow implementation must align with provider-specific native SDK rules; validate state at client and callback and avoid accepting arbitrary redirect_uri.
POST /v1/auth/refresh {refresh_token} -> rotated session; POST /v1/auth/logout -> 204; POST /v1/auth/logout-all -> 204; GET /v1/me/sessions -> list; DELETE /v1/me/sessions/{id} -> revoke; POST /v1/auth/identities/link and /unlink -> authenticated + reauth.
GET /v1/me -> profile; PATCH /v1/me -> safe profile fields; POST /v1/me/export -> export request; DELETE /v1/me -> deletion request with confirmation.

## Test matrix
Google valid signature/issuer/aud/nonce, invalid JWKS, expired/replayed state, email not verified, account collisions; Apple private relay, first-login name, invalid nonce, rotated JWKS; password wrong/locked/rate-limited/compromised; magic link expired/reused/concurrent redemption/link scanners, token leak prevention, request enumeration; refresh rotation/reuse/revocation; linking requires reauth; ownership isolation; delete/recovery; local dev provider cannot run in production mode.

## External configuration gates
Google OAuth client IDs (iOS and web as applicable), redirect allowlist and consent screen; Apple Developer App ID, Service ID if web, Team ID and signing keys/entitlements; verified email sender domain/provider for production; HTTPS domains/universal links. None may be invented or committed.

## Proposed simplification (C1–C2; not approved)
- Consider deferring email/password and using Apple + Google + email six-digit OTP instead of magic links. Do not remove the existing four-method plan without owner decision. OTP proposal: short TTL, attempts/rate limits, server-side hashed code and uniform 202 requests.

## Open decisions (C3–C5)
- C3: Verify current App Store account deletion requirement, Apple token revocation procedure, and Sign in with Apple equivalence rules before publishing.
- C4: Prevent automatic identity merging based on matching emails; propose re-authenticated explicit linking, including Apple private relay and collision recovery; final policy OPEN.
- C5: Compare opaque vs signed short-lived access tokens and logout-all consistency across replicas. No token type selected.
- A7: Email verification proposed `204` response is not frozen; document whether verification also establishes a session before OpenAPI approval.
