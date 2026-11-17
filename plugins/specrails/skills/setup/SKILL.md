---
name: setup
description: Adds the SpecRails engineering guidelines (think before coding, simplicity first, surgical changes, goal-driven execution) to this project's CLAUDE.md or AGENTS.md so they apply to every task. Run once per project.
disable-model-invocation: true
---

# SpecRails - setup

Add the guidelines at the end of this file (the text inside the fence under
"Guidelines to add") to this project so the agent
follows them on every task, not only inside the SpecRails commands.

1. Find the project's instructions file at the project root: `CLAUDE.md` first,
   then `AGENTS.md`.
2. If either file already contains a heading "Engineering guidelines (SpecRails)",
   say the guidelines are already installed and stop.
3. If the file exists, append those guidelines at the end, after a
   blank line. Do not change anything already in the file.
4. If neither file exists, create `CLAUDE.md` with a `# Project guidelines` heading
   followed by those guidelines.
5. Suggest adding `.specrails/` to version control, so specs and plans survive
   across sessions and teammates can read them.
6. Tell the user which file you changed, and that `/specrails:align <feature>`
   starts the loop on their next feature.

## Guidelines to add

````markdown
## Engineering guidelines (SpecRails)

_Sections 1-4 are adapted from [andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) by forrestchang (MIT)._

### 1. Think before coding
- Do not assume. If a request has multiple reasonable interpretations, list them
  and ask. Never pick one silently.
- State assumptions explicitly before implementing.
- If a simpler approach exists than what was asked, say so and explain the
  tradeoff.
- If something is unclear or contradictory, stop and ask. Name the confusion.

### 2. Simplicity first
- Write the minimum code that solves the stated problem. Nothing speculative.
- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or configurability that was not requested.
- No error handling for impossible scenarios.
- If you wrote 200 lines and it could be 50, rewrite it.
- Test: would a senior engineer call this overcomplicated? If yes, simplify.

### 3. Surgical changes
- Touch only what the task requires.
- Do not "improve" adjacent code, comments, or formatting.
- Do not refactor things that are not broken.
- Match the existing style, even if you would do it differently.
- If you notice unrelated dead code, mention it - do not delete it.
- Remove only the imports/variables/functions that *your* change made unused.
- Every changed line must trace directly to the request.

### 4. Goal-driven execution
- Turn tasks into verifiable goals before starting:
  - "Add validation" -> "Write tests for invalid inputs, then make them pass."
  - "Fix the bug" -> "Write a test that reproduces it, then make it pass."
- For multi-step work, state a short plan with a verification check per step:
  ```
  1. [step] -> verify: [check]
  2. [step] -> verify: [check]
  ```
- Do not declare work finished until its success criteria verifiably pass and the
  full test suite is still green.

### Workflow
For any non-trivial feature, follow the SpecRails loop (`/specrails:align` to start):
ALIGN -> SPEC -> PLAN -> BUILD (red-green-refactor) -> VERIFY.
Do not jump straight to BUILD on work that has open questions.

### Comments
Default to no comments. Add one only when the reason behind the code is not obvious
from the code itself (a hidden constraint, a workaround, a subtle invariant). Never
write comments that just restate what the code does.
````
