# The SpecRails Methodology

> The difference between an AI agent that ships and one that wastes your day is
> not the model. It is how much ambiguity you remove before it writes a line of
> code, and how tightly you define "done."

This guide is the entire system. The templates and prompts are just this guide,
operationalized. Read it once end to end; it takes about ten minutes and it will
change how you work with AI agents permanently.

---

## Why vibe coding hits a wall

Large models are extraordinary at one thing: **looping toward a target until they
hit it.** Give them a clear target and they are unstoppable. Give them a vague one
and they fill the gap with assumptions - and their assumptions are confidently
wrong.

Vibe coding ("just describe it and let the AI go") fails not because the model is
weak, but because *you never gave it a target it could verify against.* So it:

1. **Assumes silently** instead of asking, and builds the wrong thing.
2. **Overengineers** - adds abstractions, config, and error handling nobody asked
   for, because "more" feels safer than "right."
3. **Edits beyond scope** - refactors adjacent code it doesn't fully understand,
   introducing bugs orthogonal to your request.
4. **Has no definition of done** - so it stops when it *feels* finished, not when
   it *is* finished.

Spec-driven development fixes all four by front-loading clarity. SpecRails is a
concrete, repeatable way to do that.

---

## The four principles underneath everything

Every phase below is an application of these four rules. If you remember nothing
else, remember these. They are adapted from
[andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) by
forrestchang (MIT).

### 1. Think before coding
Don't assume. Don't hide confusion. Surface tradeoffs. If there are multiple
interpretations, the agent must list them and ask - never pick silently.

### 2. Simplicity first
The minimum code that solves the *stated* problem. No speculative features, no
abstractions for single-use code, no error handling for impossible scenarios. If
200 lines could be 50, it must be 50.

### 3. Surgical changes
Touch only what the task requires. Don't "improve" adjacent code. Match the
existing style even if you'd do it differently. Every changed line must trace
directly to the spec.

### 4. Goal-driven execution
Define verifiable success criteria, then loop until they pass. "Make it work" is
not a criterion. "This test passes" is.

---

## The 5-phase loop

```
ALIGN  ->  SPEC  ->  PLAN  ->  BUILD  ->  VERIFY
  ^                                          |
  +------------------------------------------+
           (next slice / next feature)
```

You run the full loop per **feature**, and you run BUILD->VERIFY per **slice**
inside a feature. Small features can compress ALIGN+SPEC into five minutes; large
ones deserve a real spec. Use judgment - the loop scales down, it is not
bureaucracy.

---

### Phase 1 - ALIGN (kill ambiguity)

**Goal:** surface every hidden assumption *before* design, not after a wrong build.

This is the highest-leverage phase and the one everyone skips. You let the agent
interrogate *you*. It asks pointed questions until the decision tree is resolved:
scope, edge cases, data shape, who the user is, what is explicitly out of scope.

**You are done with ALIGN when:** there are no open questions that would change the
design, and "out of scope" is written down explicitly.

> Command: `/specrails:align <feature>`

**Anti-pattern:** accepting the first interpretation. If the agent isn't asking you
at least 3-5 sharp questions on a non-trivial feature, it's guessing.

---

### Phase 2 - SPEC (define verifiable success)

**Goal:** write down *what* success looks like, in terms you could test, before any
*how*.

A spec is not a design doc. It answers: What does the user do? What should happen?
What are the acceptance criteria, as concrete checks? What is explicitly NOT being
built? Keep it short - half a page beats five pages nobody reads.

**You are done with SPEC when:** every requirement is phrased as something you can
verify (a test, an observable behavior), and the agent could not "technically
satisfy" it while missing the point.

> Command: `/specrails:spec`

---

### Phase 3 - PLAN (slice into shippable steps)

**Goal:** turn the spec into an ordered list of **vertical slices**, each one
independently buildable, testable, and (ideally) shippable.

A vertical slice cuts through the whole stack to deliver one small piece of real
value, rather than building all the "back" then all the "front." Each slice gets a
verification step attached: "slice done when X test passes / Y is observable."

**You are done with PLAN when:** you can hand any single slice to the agent in
isolation and it knows exactly what to build and how you'll check it.

> Command: `/specrails:plan`

**Anti-pattern:** one giant 300-line commit. If a slice can't be reviewed in a few
minutes, it's too big - split it.

---

### Phase 4 - BUILD (red-green-refactor, surgically)

**Goal:** implement one slice with test-first discipline and zero scope creep.

The loop, per slice:

1. **RED** - write a failing test that encodes the slice's success criterion.
2. **GREEN** - write the *minimum* code to make it pass. Nothing more.
3. **REFACTOR** - clean up only what you just wrote, keeping tests green.

Principles 2 and 3 (simplicity, surgical changes) live here. The agent does not
add features you didn't ask for, does not touch unrelated code, and does not
"improve" things mid-task. If it spots unrelated dead code, it *mentions* it - it
does not delete it.

**You are done with a slice when:** its test passes, the full suite is still green,
and the diff contains only lines that trace to this slice.

> Command: `/specrails:build [slice]`

---

### Phase 5 - VERIFY (review and confirm done)

**Goal:** prove the work meets the spec, not just that it runs.

Two gates:

- **Code review** against the code-review checklist - correctness, scope
  discipline, simplicity, tests, no regressions.
- **Definition of done** against the definition-of-done checklist - every spec
  acceptance criterion is checked off, docs/edge cases handled, nothing left
  half-finished.

**You are done when:** every spec criterion is verifiably met and the review
checklist passes. Then loop back for the next slice or next feature.

> Command: `/specrails:verify`

---

## How to actually use this day to day

- **Trivial change** (typo, one-liner): skip the loop. Use judgment. Don't turn
  this into bureaucracy.
- **Normal feature**: 5 min ALIGN, half-page SPEC, quick PLAN, then BUILD->VERIFY
  per slice. This is the default.
- **Big or risky feature**: full loop, real spec, careful slicing. The rigor pays
  for itself the moment it prevents one wrong rebuild.

The whole system biases toward **caution over raw speed**. That trade is correct
for anything you intend to maintain. It is the opposite of vibe coding, and that
is the point.

---

## What good looks like (you'll know it's working when)

- Your diffs shrink. Only requested changes appear.
- The agent asks clarifying questions *before* building, not apologies after.
- You stop throwing away whole sessions of work.
- The codebase stays legible at month three, not just week one.

That's the entire methodology. The rest of this kit is just these ideas turned into
commands you can run. Start with `/specrails:align` on your next feature.
