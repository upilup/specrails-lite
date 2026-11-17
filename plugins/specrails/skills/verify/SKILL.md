---
name: verify
description: SpecRails phase 5. Reviews the work just built against the spec in three gates (code review, delete-list, definition of done) and says whether it is really done. Use after each /specrails:build.
argument-hint: "[feature-slug]"
disable-model-invocation: true
---

# SpecRails - VERIFY

Your job here is to find problems, not to reassure. Review the work as if someone
else wrote it.

## Find the work

Use the feature slug in `$ARGUMENTS`, or the only folder in `.specrails/` with work
in progress (ask if there are several). Read `spec.md`, `plan.md` and `tasks.md`,
then look at the actual changes (`git diff` against the last commit before this
feature, or the files the slice touched).

## Gate 1 - Code review

Go through the code-review checklist at the end of this file item by item. Mark each PASS or FAIL
with a one-line reason. Pay special attention to:
- **Scope:** does every changed line trace to the spec? Flag any that don't.
- **Simplicity:** is anything overcomplicated or speculative?
- **Tests:** do they test the criterion, or just pad coverage?
- **Regressions:** run the full suite (or the hand checks) now. Do not rely on an
  earlier run.

## Gate 2 - Delete-list

Hunt for what can be removed: logic the standard library already provides,
abstractions with a single use, config nobody asked for, code that could be
shorter. Output one line per finding:

`what to cut -> what replaces it`

The best result of this gate is a shorter diff. If there is genuinely nothing to
cut, write "Lean already."

## Gate 3 - Definition of done

Go through the definition-of-done checklist at the end of this file. Check each acceptance
criterion in `spec.md` against the real implementation, not against what was
intended. List any criterion that is not fully and verifiably met.

Then collect every `SPECRAILS-DEFER:` tag in the changed code into a short ledger,
and confirm none of them is a required acceptance criterion in disguise.

## Verdict

- **Everything passes:** say `VERIFIED`, tick the slice's remaining items in
  `tasks.md`, and say whether the next step is `/specrails:build` (more slices) or
  the feature is complete.
- **Anything fails:** list the exact fixes needed, leave the boxes unticked, and
  stop. A partial pass is not a pass.

## Code-review checklist

```markdown
# Code Review Checklist

> Run at VERIFY. Mark PASS/FAIL with a one-line reason for each. A single FAIL means
> the work is not done.

## Correctness
- [ ] The code does what the spec's acceptance criteria require.
- [ ] Edge cases listed in the spec are handled.
- [ ] No obvious logic errors, off-by-ones, or unhandled failure paths that matter.

## Scope discipline
- [ ] Every changed line traces directly to the spec/slice.
- [ ] No adjacent code was "improved," reformatted, or refactored without reason.
- [ ] No unrelated files were touched.
- [ ] Only imports/variables made unused by this change were removed; pre-existing
      dead code was left alone (mentioned, not deleted).

## Simplicity
- [ ] No speculative features or configurability that wasn't requested.
- [ ] No abstractions introduced for single-use code.
- [ ] No error handling for impossible scenarios.
- [ ] A senior engineer would not call this overcomplicated.

## Over-engineering pass (produce a delete-list)
> Don't just confirm the code is fine - actively hunt for what can be removed.
> Output an explicit delete-list; the best outcome of a review is a shorter diff.
- [ ] Hand-rolled logic the standard library or platform already provides (replace
      it; name the built-in).
- [ ] Abstractions with a single implementation, factories with one product,
      wrappers that only delegate (inline them).
- [ ] Config, flags, or "flexibility" nobody asked for (delete).
- [ ] Same behavior expressible in fewer lines (show the shorter form).
- [ ] **Delete-list:** one line per finding - `what to cut -> what replaces it`.
      If there is genuinely nothing to cut, write "Lean already."

## Tests
- [ ] Tests actually encode the spec criteria (not just coverage padding).
- [ ] There is a test that would fail if the core behavior broke.
- [ ] The full suite passes (no regressions).

## Style & clarity
- [ ] Matches the existing code style of the repo.
- [ ] Names are clear; code is self-explanatory.
- [ ] Comments exist only where intent isn't obvious from the code.

## Security & data (if applicable)
- [ ] No secrets, keys, or sensitive data committed or logged.
- [ ] Inputs from outside the system are validated.
```

## Definition-of-done checklist

```markdown
# Definition of Done

> A feature is NOT done until every box here is checked. "Feels finished" is not
> "is finished."

## Spec satisfied
- [ ] Every acceptance criterion in `spec.md` is met and verifiably checked.
- [ ] Nothing in the spec's "Out of scope" crept in.
- [ ] Realistic edge cases from the spec are handled.

## Quality
- [ ] Gate 1 (code review) passes with no FAILs.
- [ ] Full test suite is green.
- [ ] Lint / typecheck pass (if the project uses them).

## Completeness
- [ ] No TODOs, stubs, or commented-out code left from this work.
- [ ] No half-finished slices; partial work is not merged.
- [ ] Any user-facing behavior change is reflected in docs/README if the project
      keeps them.

## Hygiene
- [ ] The diff is scoped and reviewable.
- [ ] Commit message (or PR) explains the *why*, not just the *what*.
- [ ] No debug logging, scratch files, or secrets left behind.

## Deferred-work ledger
> Deliberate shortcuts are fine - silent ones are not. Anything you chose to defer
> must be visible, or "later" quietly becomes "never."
- [ ] Every intentional shortcut is marked in-code with a consistent, greppable tag
      (e.g. `SPECRAILS-DEFER:`) naming what was skipped and the upgrade path.
- [ ] Those tags are collected into a short ledger (in the PR description or a
      tracking note) so each deferral is tracked, not buried.
- [ ] Nothing on the ledger is actually a spec acceptance criterion in disguise -
      deferring a required behavior means the feature is NOT done.

## Final check
- [ ] If someone else opened this PR cold, would they understand what changed and
      trust it? If not, fix that before calling it done.
```
