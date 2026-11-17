---
name: plan
description: SpecRails phase 3. Breaks an approved spec into small vertical slices, each with its own verify step, plus a task checklist. Use after /specrails:spec is approved.
argument-hint: "[feature-slug]"
disable-model-invocation: true
---

# SpecRails - PLAN

## Find the feature

Look in `.specrails/` in the project root.
- If a slug was given (`$ARGUMENTS`), use `.specrails/<slug>/`.
- Otherwise, if exactly one feature folder has a `spec.md` but no `plan.md`, use it.
- Otherwise, list the feature folders and ask which one.

Read `spec.md`. If it has open questions or no acceptance criteria, stop and send
the user back to `/specrails:spec`.

## Find out how this project verifies work

Check how tests run here (test runner, test folder, the command in package.json,
Makefile, pyproject and so on).
- **Tests exist:** every slice's Verify line names a test.
- **No tests:** ask the user once which they prefer, and record the answer in the
  plan's Sequencing notes:
  - add the smallest test setup that fits this stack as Slice 1, or
  - verify by hand: each Verify line becomes exact steps to run and what to see
    (a command and its output, a page and what appears on it).

## Write the plan

Write `.specrails/<slug>/plan.md` using the plan template at the end of this file:

- **Vertical slices.** Each slice delivers one small piece of real, observable value
  through the whole stack. Not "all the backend, then all the frontend".
- **Order** so each slice builds on the last, and the riskiest unknown is tackled
  early.
- **Every slice has a Verify line:** the exact test or observable behavior that
  proves it is done.
- **No slice should produce a 300-line change.** If one would, split it.
- **No implementation code** in the plan.

Then write `.specrails/<slug>/tasks.md` using the tasks template at the end of this file,
one group per slice. If the project has no tests and the user chose hand checks,
replace the test lines with the hand check for that slice.

Delete the templates' guidance comments from both files.

## Then stop

Show the slice list (title + Verify line for each), ask for approval, and name the
next step: `/specrails:build`. Do not write code until the plan is approved.

## Plan template

```markdown
# Plan: <feature name>

> Turns the spec into ordered **vertical slices**. Each slice delivers one small
> piece of real value through the whole stack, is independently testable, and is
> small enough to review in a few minutes.

## Reference
- Spec: `./spec.md`

## Slices (in build order)

### Slice 1 - <short title>
- **Delivers:** <the one thing of value this slice adds>
- **Build:** <what to implement, briefly>
- **Verify:** <the test or observable behavior that proves it's done>

### Slice 2 - <short title>
- **Delivers:**
- **Build:**
- **Verify:**

### Slice 3 - <short title>
- **Delivers:**
- **Build:**
- **Verify:**

## Sequencing notes
<!-- Dependencies between slices, anything that must come first, anything that can
be parallelized. -->

## Rules
- No slice should be a 300-line commit. If one is, split it.
- Each slice ends with a green test and a clean, scoped diff before starting the
  next.
```

## Tasks template

```markdown
# Tasks: <feature name>

> Live checklist for the current feature. One line per task, grouped by slice.
> Keep exactly one task in progress at a time. Check items off only when their
> verification actually passes - not when you intend to do them.

## Slice 1 - <title>
- [ ] Write failing test for <criterion>
- [ ] Implement minimum code to pass
- [ ] Refactor (tests stay green)
- [ ] Full suite green + diff scoped to this slice

## Slice 2 - <title>
- [ ] Write failing test for <criterion>
- [ ] Implement minimum code to pass
- [ ] Refactor (tests stay green)
- [ ] Full suite green + diff scoped to this slice

## Slice 3 - <title>
- [ ] Write failing test for <criterion>
- [ ] Implement minimum code to pass
- [ ] Refactor (tests stay green)
- [ ] Full suite green + diff scoped to this slice

## Done when
- [ ] Every spec acceptance criterion checked off
- [ ] `/specrails:verify` code review passes
- [ ] `/specrails:verify` definition of done passes
```
