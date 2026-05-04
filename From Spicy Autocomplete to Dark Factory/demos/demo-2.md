# Demo 2 — Plan → Execute → Review

The controller pattern. Instead of telling Claude "build this feature" and hoping for the best, you force it to commit to a written plan *before* writing any code — then review its own work against that plan when it's done.

The magic is the pause in the middle: you read the plan, spot wrong assumptions, redirect if needed, *then* say go. That moment is much cheaper than throwing away half-finished code.

**What this demonstrates:**
- Forcing a planning step that produces a readable artifact (PLAN.md)
- Human-in-the-loop approval before execution starts
- Self-review against a stated plan (REVIEW.md) — deviations are logged, not buried
- Artifacts that survive the session and can be code-reviewed by teammates

---

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- `lowerCaseMe.py` from Demo 1 in your working directory (or any small Python file you want to extend)
- Python 3.x

> **New to agentic coding?** Demo 1 is a good warmup — it introduces sub-agents and takes about 5 minutes.

---

## The prompt

Open Claude Code in the directory with `lowerCaseMe.py`:

```bash
claude
```

Paste this entire block as one message:

```
Extend lowerCaseMe.py into a multi-mode text transformer with --upper,
--snake, --kebab, and --leet flags. Add tests.

But work in three steps:

1. PLAN: Write PLAN.md -- the modes, ordering, file/stdin handling, edge
   cases, and the test cases. Stop and wait for me to approve.

2. EXECUTE: Once I approve, implement the plan. Run the tests as you go.

3. REVIEW: Write REVIEW.md -- diff the result against the plan, list any
   deviations, and rate your own work.
```

---

## Step by step

### Step 1 — Claude writes the plan and stops

Claude will write `PLAN.md` and then stop. It will not write any code yet.

Open `PLAN.md` and read it. Look for:
- Does it understand what `--snake` and `--kebab` mean? (underscores vs hyphens)
- Does it plan to handle stdin as well as file input?
- Are the edge cases sensible? (empty string, already-lowercase input, etc.)
- Does the test plan match what you'd actually want tested?

If something looks wrong, tell Claude now. For example:
```
The plan looks good, but also handle the case where no flag is passed —
it should default to lowercase. Update PLAN.md, then wait again.
```

Or if it looks good, approve it:
```
Plan approved. Proceed to step 2.
```

### Step 2 — Claude implements and runs tests

Claude implements everything in the plan and runs the tests as it goes. Watch the output — if a test fails it should fix it before moving on.

When it finishes, you'll have an updated `lowerCaseMe.py` and a test file.

### Step 3 — Claude writes the review

Claude writes `REVIEW.md`. This should list:
- Everything in the plan that was implemented as specified
- Anything that was skipped, changed, or added beyond the plan
- An honest self-rating

Read `REVIEW.md`. If it notes deviations, decide whether they were good judgment calls or mistakes.

---

## What to notice

**The plan is the cheap moment to redirect.** If Claude misunderstood `--snake_case` vs `--kebab-case` in the plan, you catch it in 30 seconds. If you catch it after implementation, you're throwing away code.

**PLAN.md and REVIEW.md are real artifacts.** They're in your working directory. You can `git add` them, share them with a teammate, or diff them against each other to see what drifted.

**You were the controller.** Claude ran the three steps, but you decided when to proceed from Plan to Execute. That gate is the pattern. Without it, Claude would have just built whatever it thought you meant.

---

## When to use this pattern

| Use it when... | Skip it when... |
|---|---|
| The change touches multiple files | It's a one-line fix |
| You'd hate to throw away the work | You're exploring / learning the codebase |
| You want a paper trail beyond the chat | The task is well-understood and low-risk |
| The task is big enough to lose the thread | Speed matters more than auditability |

---

## Going further — worktrees

For larger features, you can run the Execute step in a **git worktree** — a separate checkout of the same repo in a different directory. This means:
- The main branch stays clean while work is in progress
- You can keep using other parts of the repo uninterrupted
- Multiple agents can execute in parallel without stepping on each other

The starter repo's `/ship` slash command does exactly this:

```bash
git clone https://github.com/BillDX/spicy-to-dark-factory-starter
cd spicy-to-dark-factory-starter
uv sync
claude
```

Then:
```
/ship SPEC.md
```

The planner reads the spec and produces a plan. After you approve, a coder and a tester run in parallel worktrees. When both finish, you review the combined result.
