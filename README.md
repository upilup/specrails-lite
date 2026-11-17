# SpecRails Lite - Stop Vibe Coding (free starter)

**The free taste of SpecRails - the spec-driven system that
turns any AI coding agent into a disciplined senior engineer.**

Vibe coding feels great for a weekend, then collapses: silent wrong assumptions,
800 lines where 80 would do, "improvements" to code you never touched, and an
unmaintainable mess by month three.

SpecRails fixes that with one idea: **remove ambiguity and define "done" before the
agent writes a line.** This free starter gives you the foundation. The full kit
gives you the whole system.

---

## What's free here

- [`CLAUDE.md`](./CLAUDE.md) - the four-principle guidelines file. Drop it into any
  repo and your agent immediately stops overengineering and starts making surgical,
  scoped changes. Works as `AGENTS.md` for Cursor/Codex too.
- The methodology in a nutshell (below).

## The 5-phase loop (overview)

```
ALIGN  ->  kill ambiguity before any code is written
SPEC   ->  write down what success looks like, verifiably
PLAN   ->  break it into small, shippable vertical slices
BUILD  ->  red-green-refactor, surgical, nothing speculative
VERIFY ->  review against a checklist, confirm definition of done
```

## The four principles (the heart of it)

1. **Think before coding** - don't assume; list interpretations and ask.
2. **Simplicity first** - minimum code for the stated problem; nothing speculative.
3. **Surgical changes** - touch only what the task requires.
4. **Goal-driven execution** - define verifiable success, loop until it passes.

These four principles are adapted from
[andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) by
forrestchang (MIT), which distills
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on LLM coding pitfalls. SpecRails builds the 5-phase loop on top of them.

---

## The full kit (not released yet)

SpecRails Lite is the foundation. The complete kit, still in progress, adds the parts
that make the loop effortless to run every day:

- Copy-paste **workflow prompts** for all 5 phases (ALIGN/SPEC/PLAN/BUILD/VERIFY)
- **Spec, plan, and tasks templates**
- **Code-review** and **definition-of-done** checklists
- A full **worked example** (one feature, idea to shipped, every phase shown)
- **Adapters** for Claude Code, Cursor, and Codex

---

## License
This free starter is MIT licensed - use it anywhere. See [`LICENSE`](./LICENSE),
which also carries the notice for the adapted Karpathy guidelines.
