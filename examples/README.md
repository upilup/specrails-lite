# Worked Example: Rate-Limit the Login Endpoint

This walks one small, realistic feature through the entire SpecRails loop so you can
see exactly how each phase feels in practice. It is intentionally language-agnostic;
the artifacts matter more than the syntax.

Files in this folder:

- `transcript.md` - the full ALIGN -> SPEC -> PLAN -> BUILD -> VERIFY walkthrough,
  including the questions the agent asks and how vague requests get sharpened into
  verifiable ones.
- `spec.md` - the spec that came out of the SPEC phase.
- `plan.md` - the sliced plan from the PLAN phase.

Read `transcript.md` first. Notice three things:

1. How ALIGN catches assumptions ("limit per IP or per account?") that would
   otherwise produce the wrong build.
2. How every acceptance criterion is phrased as something testable.
3. How the work is sliced so no single step is a giant commit.
