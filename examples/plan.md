# Plan: Login Rate Limiting

## Reference
- Spec: `./spec.md`

## Slices (in build order)

### Slice 1 - Rolling-window counter store
- **Delivers:** a reusable in-memory counter that records attempts, counts recent
  ones in a window, and resets.
- **Build:** `record(id)`, `count_recent(id, window)`, `reset(id)`; prune
  timestamps older than the window.
- **Verify:** unit tests for threshold counting, window expiry, and reset.

### Slice 2 - Enforce limit by IP
- **Delivers:** the login handler blocks an IP after the threshold and returns 429.
- **Build:** record failures by IP; before processing, check the counter; on block
  return 429 with `Retry-After`.
- **Verify:** integration test - 6th failed attempt from one IP returns 429 with a
  `Retry-After` header.

### Slice 3 - Enforce limit by account
- **Delivers:** the same protection keyed on account identifier; stricter dimension
  wins.
- **Build:** also record/check by account; if either dimension is over the limit,
  block.
- **Verify:** integration test - 6th failed attempt for one account returns 429 even
  from a fresh IP.

### Slice 4 - Reset on success
- **Delivers:** a successful login clears that identifier's counter.
- **Build:** call `reset()` for IP and account on successful authentication.
- **Verify:** test - after a success, the counter is zero and the next attempt is
  not blocked.

## Sequencing notes
- Slice 1 has no dependencies and de-risks the core logic first.
- Slices 2-4 build on Slice 1 and should each ship green before the next starts.

## Rules
- No slice should be a 300-line commit.
- Each slice ends green with a scoped diff before the next begins.
