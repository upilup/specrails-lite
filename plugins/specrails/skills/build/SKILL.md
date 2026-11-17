---
name: build
description: SpecRails phase 4. Builds exactly one slice of an approved plan, test first, touching only what that slice needs. Use after /specrails:plan is approved, once per slice.
argument-hint: "[slice-number] [feature-slug]"
disable-model-invocation: true
---

# SpecRails - BUILD

## Find the slice

Arguments: `$ARGUMENTS` (optional slice number, optional feature slug).

- Feature: use the given slug, or the only folder in `.specrails/` that has a
  `plan.md` with unchecked tasks. If several do, ask which one.
- Slice: use the given number, or the first slice in `tasks.md` that still has
  unchecked items.

Read `spec.md`, `plan.md` and `tasks.md` for that feature. Build **only this slice**.
If the slice turns out to need something the plan did not foresee, stop and say so
instead of quietly expanding it.

## Red, green, refactor

1. **RED.** Write a test that encodes this slice's Verify line. Run it and confirm it
   fails *for the right reason* (the missing behavior, not a typo or import error).
   Show the user the test and the failure.
   - If the plan says this project verifies by hand, skip the test and instead run
     the hand check now to confirm the behavior is missing.
2. **GREEN.** Write the minimum code that makes it pass. No features nobody asked
   for, no abstractions for one use, no config nobody requested, no error handling
   for impossible cases.
3. **REFACTOR.** Clean up only the code you just wrote, keeping tests green.

## Hard rules while building

- Touch only what this slice requires. Do not improve, reformat or refactor
  neighbouring code. Match the existing style.
- If you notice unrelated dead code or a separate bug, mention it. Do not fix it.
- Remove only the imports, variables and functions that *your* change made unused.
- Every changed line must trace back to this slice.
- A deliberate shortcut is allowed only if it is visible: mark it in the code with
  `SPECRAILS-DEFER: <what was skipped> -> <how to do it properly>`.

## Finish the slice

1. Run the full test suite (or every hand check done so far) and confirm it passes.
2. Tick this slice's items in `tasks.md` **only for checks that actually passed**.
3. Show a short diff summary that proves the change is scoped to this slice.
4. Name the next step: `/specrails:verify`. Then stop. Do not start the next slice.
