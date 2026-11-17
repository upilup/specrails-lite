# Walkthrough: Rate-Limit the Login Endpoint

A realistic run of the full loop. **You** lines are what the developer types;
**Agent** lines are the agent following the SpecRails guidelines.

---

## Phase 1 - ALIGN

**You:** `/specrails:align` Feature: add rate limiting to our login
endpoint so people can't brute-force passwords.

**Agent:** Before I design anything, some questions:

1. **Limit dimension:** per IP address, per account (username/email), or both? They
   defend against different attacks.
2. **Threshold + window:** what counts as "too many"? e.g. 5 attempts per 15 minutes?
3. **Response on limit:** HTTP 429 with a retry-after header, or a generic error? Do
   you want to reveal how long they're locked out?
4. **Storage:** single server (in-memory is fine) or multiple servers (needs shared
   store like Redis)?
5. **Successful logins:** do they reset the counter?
6. **Out of scope:** are CAPTCHA, account lockout emails, or admin unlock part of
   this, or explicitly not?

**You:** Per IP and per account, whichever trips first. 5 attempts / 15 min. Return
429 with Retry-After. Single server for now, in-memory is fine. Yes, a successful
login resets the counter. CAPTCHA and emails are out of scope.

**Agent:** ALIGNED. Summary:
- Rate-limit login by IP and by account; the stricter of the two applies.
- 5 failed attempts per rolling 15 minutes triggers a block.
- Blocked requests get HTTP 429 + `Retry-After`.
- In-memory store, single server (Redis explicitly deferred).
- A successful login clears the counter for that identifier.
- Out of scope: CAPTCHA, lockout emails, admin unlock.

---

## Phase 2 - SPEC

**You:** `/specrails:spec`

**Agent:** *(produces `spec.md` - see that file)*. Key acceptance criteria, each
verifiable:
- After 5 failed attempts from one IP within 15 min, the 6th returns 429.
- After 5 failed attempts for one account within 15 min, the 6th returns 429.
- A 429 response includes a `Retry-After` header with seconds remaining.
- A successful login resets that identifier's counter to zero.
- Attempts older than 15 minutes no longer count (rolling window).

Open questions: none. Approve to plan?

**You:** Approved.

---

## Phase 3 - PLAN

**You:** `/specrails:plan`

**Agent:** *(produces `plan.md`)*. Vertical slices:
- **Slice 1:** Counter store with rolling-window logic (record attempt, count
  recent, reset). Verify: unit tests for count/expiry/reset.
- **Slice 2:** Wire it into the login handler for the IP dimension; return 429 +
  Retry-After. Verify: integration test - 6th attempt from an IP gets 429.
- **Slice 3:** Add the account dimension; stricter of the two applies. Verify:
  integration test - 6th attempt for an account gets 429 even from a new IP.
- **Slice 4:** Reset on successful login. Verify: test - success clears the counter.

No slice is large enough to be a 300-line commit. Approve to build?

**You:** Approved.

---

## Phase 4 - BUILD (Slice 1 shown)

**You:** `/specrails:build 1`

**Agent:**
- **RED:** wrote `test_counter_blocks_after_threshold`, `test_window_expiry`,
  `test_reset_clears_count`. Confirmed they fail (no implementation yet).
- **GREEN:** implemented a minimal in-memory store: a map of identifier -> list of
  timestamps; `record()`, `count_recent(window)`, `reset()`. ~40 lines. No Redis, no
  config system, no extra features.
- **REFACTOR:** extracted the window-pruning into one small helper. Tests stay green.

Full suite green. Diff touches only the new counter module and its test. Stopping
for VERIFY.

---

## Phase 5 - VERIFY (Slice 1)

**You:** `/specrails:verify`

**Agent:**
- **Code review** (the code-review checklist):
  - Correctness: PASS - count/expiry/reset covered by tests.
  - Scope: PASS - only the counter module and its test changed.
  - Simplicity: PASS - no speculative Redis layer; deferred per spec.
  - Tests: PASS - tests encode the criteria, suite green.
  - Security: N/A for this slice.
- **Definition of done:** Slice 1's Verify criteria met. Feature not complete -
  Slices 2-4 remain.

VERIFIED. Proceed to Slice 2?

**You:** Yes.

*(Repeat BUILD -> VERIFY for slices 2-4. When the final VERIFY checks every spec
criterion, the feature is done - and you never once got a 400-line surprise diff.)*

---

## What to notice

- **ALIGN** turned "stop brute force" into precise, testable decisions and caught
  the IP-vs-account fork that would otherwise have been guessed.
- **SPEC** made every requirement verifiable.
- **PLAN** kept commits small and reviewable.
- **BUILD** stayed surgical and simple - no premature Redis, no scope creep.
- **VERIFY** proved the spec was met, not just that code ran.
