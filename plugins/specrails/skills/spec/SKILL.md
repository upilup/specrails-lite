---
name: spec
description: SpecRails phase 2. Turns an aligned feature into a short spec with verifiable acceptance criteria and an explicit out-of-scope list. Use after /specrails:align.
argument-hint: "[feature-slug]"
disable-model-invocation: true
---

# SpecRails - SPEC

## Find the feature

Look in `.specrails/` in the project root.
- If a slug was given (`$ARGUMENTS`), use `.specrails/<slug>/`.
- Otherwise, if exactly one feature folder has an `align.md` but no `spec.md`, use it.
- Otherwise, list the feature folders and ask which one. If there are none, tell
  the user to run `/specrails:align <feature>` first and stop.

Read `align.md`. If it is missing, run the ALIGN questions first instead of
guessing.

## Write the spec

Write `.specrails/<slug>/spec.md` using the spec template at the end of this file.
Rules:

- **Short.** About half a page. A long spec nobody reads is worthless.
- **Every acceptance criterion is verifiable:** a test that can run, or a behavior
  someone can observe. No "works well", "is fast", "handles errors properly".
- **Write each criterion so it cannot be technically satisfied while missing the
  intent.** If a lazy implementation could pass it, tighten it.
- **Out of scope is filled in explicitly.** Copy it from `align.md` and add anything
  the spec makes obvious.
- **Only realistic edge cases.** Do not invent impossible scenarios to handle.
- **Open questions must be empty.** If one remains, ask it now instead of writing it
  down, and update `align.md` with the answer.

Delete the template's guidance comments from the final file.

## Then stop

Show the acceptance criteria and out-of-scope list, ask the user to approve or
change them, and name the next step: `/specrails:plan`. Do not plan or write code
until the spec is approved.

## Spec template

```markdown
# Spec: <feature name>

> Keep this short - half a page beats five pages. Every requirement must be
> verifiable. Fill in the blanks, delete the guidance comments.

## Problem / why
<!-- One or two sentences. What problem does this solve, for whom? -->

## User story
<!-- As a <user>, I want to <action>, so that <outcome>. -->

## Acceptance criteria
<!-- Each must be a concrete, checkable behavior or test. The agent must not be
able to "technically satisfy" these while missing the point. -->
- [ ]
- [ ]
- [ ]

## Out of scope (explicit)
<!-- Listing this prevents scope creep and silent assumptions. Be specific. -->
-
-

## Data / inputs / outputs
<!-- Shapes, formats, sources. Only what matters. -->

## Edge cases to handle
<!-- The realistic ones. Do NOT invent impossible scenarios. -->
-
-

## Open questions
<!-- Should be EMPTY before you leave ALIGN. If anything is here, you're not ready
to spec. -->
- (none - resolved in ALIGN)
```
