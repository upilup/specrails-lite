---
name: align
description: SpecRails phase 1. Questions you about a feature until nothing ambiguous is left, before any design or code is written. Use at the start of any non-trivial feature.
argument-hint: <the feature, in 1-3 sentences>
disable-model-invocation: true
---

# SpecRails - ALIGN

**Feature:** $ARGUMENTS

If no feature was given above, ask the user to describe it in 1-3 sentences and wait.

Do NOT write any code, spec, or design in this phase. Your only job is to remove
every assumption that could send the build in the wrong direction.

## Steps

1. **Look before you ask.** Skim the parts of the codebase this feature will touch
   (entry points, related modules, existing patterns, how tests are run). Your
   questions should be about *this* project, not generic ones. Never ask something
   you could answer by reading the code.
2. **Ask sharp questions in small batches** (3-5 at a time), then wait for answers.
   Cover only what would change the design:
   - scope and who the user is
   - data shape, inputs and outputs
   - realistic edge cases and failure behavior
   - what is explicitly OUT of scope
3. **Never pick an interpretation silently.** If the request can reasonably mean
   two things, list both and ask which one.
4. **Say so if there is a simpler approach** than what was described, and explain
   the tradeoff.
5. Keep going until you have nothing material left to ask. On a non-trivial
   feature, fewer than 3 questions usually means you are guessing.

## When aligned

1. Pick a short kebab-case slug for the feature (for example `login-rate-limit`).
2. Write `.specrails/<slug>/align.md` in the project root:

   ```markdown
   # Align: <feature name>

   ## Feature
   <the request, in one or two sentences>

   ## Agreed
   - <decision>

   ## Out of scope
   - <explicitly excluded>
   ```

3. Reply with `ALIGNED`, the 3-5 bullet summary, and the next step:
   `/specrails:spec`. Then stop.
