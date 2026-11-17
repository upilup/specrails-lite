# Spec: Login Rate Limiting

## Problem / why
Attackers can brute-force passwords against the login endpoint. We need to throttle
repeated failed attempts to make credential-stuffing impractical.

## User story
As the system, I want to block login attempts after too many recent failures, so
that brute-force and credential-stuffing attacks are impractical without harming
legitimate users.

## Acceptance criteria
- [ ] After 5 failed attempts from a single IP within a rolling 15-minute window,
      the next attempt from that IP returns HTTP 429.
- [ ] After 5 failed attempts for a single account within a rolling 15-minute
      window, the next attempt for that account returns HTTP 429, even from a new IP.
- [ ] When both dimensions apply, the stricter one (whichever trips first) governs.
- [ ] A 429 response includes a `Retry-After` header with the seconds remaining.
- [ ] A successful login resets the failure counter for that identifier to zero.
- [ ] Attempts older than 15 minutes no longer count toward the limit.

## Out of scope (explicit)
- CAPTCHA or any challenge flow.
- Account-lockout notification emails.
- Admin/manual unlock tooling.
- Multi-server shared storage (Redis) - deferred; in-memory is acceptable for now.

## Data / inputs / outputs
- Inputs: client IP, submitted account identifier (email/username), login result.
- State: per-identifier list of recent failed-attempt timestamps.
- Output: normal login response, or 429 + `Retry-After` when limited.

## Edge cases to handle
- Window boundary: an attempt exactly at the 15-minute edge should no longer count.
- A new account or IP with no history is never blocked.

## Open questions
- (none - resolved in ALIGN)
