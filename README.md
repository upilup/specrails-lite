# SpecRails

**A spec-driven loop for AI coding agents.** Five commands that make the agent ask
before it guesses, write down what "done" means, build in small tested steps, and
review its own work honestly. Free and MIT licensed.

```
/specrails:align   ->  question the request until nothing is ambiguous
/specrails:spec    ->  write verifiable acceptance criteria and what's out of scope
/specrails:plan    ->  slice the work into small steps, each with its own check
/specrails:build   ->  build one slice, test first, touching only what it needs
/specrails:verify  ->  code review, a delete-list, and a definition-of-done check
```

## Why

Left alone, a coding agent fills gaps with assumptions, writes far more code than
the task needs, "improves" code you never asked it to touch, and stops when the
work *feels* finished. SpecRails fixes the cause: it removes ambiguity and defines
done before any code is written.

## Install (Claude Code)

```
/plugin install specrails --marketplace upilup/specrails-lite
```

On Claude Code older than 2.1.275, use two steps:

```
/plugin marketplace add upilup/specrails-lite
/plugin install specrails@specrails
```

Then, once per project, add the engineering guidelines to your `CLAUDE.md` so they
apply to every task:

```
/specrails:setup
```

## How a feature goes

```
/specrails:align Add discount codes to the cart total
```

The agent reads the relevant code first, then asks questions you'd otherwise find
out the hard way: where do codes come from, percent or fixed, what happens with an
unknown code, how to round. When nothing is left to ask it writes a summary and you
move on:

```
/specrails:spec      # review and approve the acceptance criteria
/specrails:plan      # review and approve the slices
/specrails:build     # builds the next slice, test first
/specrails:verify    # reviews it; repeat build + verify per slice
```

Each step saves its output in your project, so you can stop and pick up in a new
session:

```
.specrails/discount-codes/
  align.md   what was agreed and what's out of scope
  spec.md    acceptance criteria you can test
  plan.md    the slices and how each one is checked
  tasks.md   the checklist, ticked only when checks pass
```

Commit `.specrails/` with your code so specs and plans stay with the project.

From a real test run: after slice 1 of that discount feature, `/specrails:verify`
passed the slice but flagged that the second code in the table returned `-8000`
until slice 2 added fixed discounts, and said not to ship before then.

## No tests in your project?

SpecRails doesn't skip verification. During `/specrails:plan` it asks once: add the
smallest test setup that fits your stack as the first slice, or check each slice by
hand with exact steps and what you should see.

## Other agents

- **Agents that read `SKILL.md` skills:** the six folders in
  [`plugins/specrails/skills/`](plugins/specrails/skills) are plain skills. Copy them
  to wherever your agent loads skills from (see its docs).
- **Anything else (Cursor, Codex and others):** copy [`CLAUDE.md`](CLAUDE.md) into
  your repo, as `AGENTS.md` if your agent reads that, and paste a skill's text as a
  prompt when you reach that phase.

## More

- [`GUIDE.md`](GUIDE.md) - the method behind each phase, in about ten minutes.
- [`examples/`](examples) - one feature (login rate limiting) taken through every
  phase.

## Credits

The four principles behind SpecRails (think before coding, simplicity first,
surgical changes, goal-driven execution) are adapted from
[andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) by
forrestchang (MIT), which distills
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on LLM coding pitfalls. SpecRails adds the five-phase loop, the commands, the
templates and the review gates.

## License

MIT. See [`LICENSE`](LICENSE), which also carries the notice for the adapted
Karpathy guidelines.
