# Demo 3 — The Architecture Debate

Three Claude agents. Three different engineering philosophies. One small problem. They argue it out and produce a single recommendation — the disagreement is the point.

A single agent asked to design a function will pick one approach and move on. It won't surface the tradeoffs it waved past. This demo forces three agents with opposing stances to make those tradeoffs explicit — on the record, in writing.

**What this demonstrates:**
- Multiple Claude sessions running simultaneously in tmux, each with a distinct role
- Agents reading each other's work and responding with specific objections
- Structured disagreement producing a richer artifact than any one agent would write alone
- `--dangerously-skip-permissions` used responsibly (throwaway branches + you watching live)

> **Honest note:** This is a *team-shaped exercise*, not a true Claude Code agent team. The three agents coordinate by reading and writing files across shared git worktrees — no direct messaging between sessions. Real Claude Code agent teams (an experimental feature) add native `@-mention` messaging on top. The pattern — sharp opposing roles, structured disagreement, a converged artifact — is the same in both. The plumbing differs. See the slides for a full comparison.

---

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- `tmux` installed (`brew install tmux` on macOS)
- The starter repo cloned locally (the script lives there):
  ```bash
  git clone https://github.com/BillDX/spicy-to-dark-factory-starter
  cd spicy-to-dark-factory-starter
  ```
- Python 3.x (the agents write Python)

> **New to tmux?** tmux is a terminal multiplexer — it lets you split one terminal window into multiple independent panes. You don't need to know tmux commands for this demo. The script sets everything up and turns on mouse mode, so you can just click between panes.

> **New to git worktrees?** A worktree is a second (or third, or fourth) checkout of the same git repo in a different directory. Each agent in this demo gets its own worktree so they can write files independently without overwriting each other. The script creates and destroys these automatically.

---

## The fast path — one command

From the `spicy-to-dark-factory-starter` directory:

```bash
./scripts/demo3-debate.sh
```

That's it. The script:
1. Creates three git branches (`debate-minimalist`, `debate-defensive`, `debate-perf`)
2. Creates a git worktree for each branch in a sibling directory
3. Opens a tmux session called `debate` with three panes
4. Launches Claude in each pane with its persona already submitted as the first message
5. Waits ~20 seconds for each Claude to acknowledge its role
6. Auto-fires the kickoff prompt to all three panes simultaneously
7. Attaches you to the tmux session so you can watch

You'll land in tmux watching three Claude sessions work.

### Navigating tmux

Mouse mode is on — **click a pane to focus it**. Keyboard shortcuts if you prefer:

| Key | Action |
|---|---|
| Click a pane | Focus it |
| `Ctrl-b` then `←/→/↑/↓` | Switch panes by keyboard |
| `Ctrl-b` then `z` | Zoom in on one pane / unzoom |
| `Ctrl-b` then `d` | Detach (leaves the session running) |
| `tmux attach -t debate` | Re-attach after detaching |

### When you're done

```bash
./scripts/demo3-debate.sh --cleanup
```

Kills the tmux session, removes the three worktrees, and deletes the debate branches.

---

## What the agents are doing

### The problem

Each agent is asked to implement:

```python
def dedupe(items: list[str]) -> list[str]:
    """Remove case-insensitive duplicates, preserving first-seen order."""
    ...
```

Simple enough that all three can hold it in mind. Complex enough that three engineers with different priorities will genuinely disagree on how to write it.

### The three personas

**Minimalist** — "The simplest correct implementation always wins. Lines of code are a cost."

Expects a short list comprehension or generator. Will push back on anything that adds complexity without clear necessity.

**Defensive Coder** — "Handle every edge case up front. Silent bugs are the worst bugs."

Expects explicit checks for `None` items, empty input, Unicode edge cases, and clear error messages. Will push back on anything that could fail silently.

**Performance Hawk** — "Runtime and memory matter most. Don't ship code you haven't reasoned about."

Expects O(n) time, minimal allocations, and justification for every data structure choice. Will push back on anything with a hidden performance cost.

### The four-step sequence

Each agent runs through the same steps:

1. **Write `IMPL.py`** in their own worktree — their proposed implementation, with comments explaining their design choices
2. **Read the others' `IMPL.py` files** — each agent reads the other two worktrees
3. **Write `CRITIQUE.md`** — their sharpest, most specific objections to each of the other implementations
4. **Converge on `PROPOSAL.md`** — the Minimalist writes the synthesis; the Defensive Coder and Perf Hawk each append a paragraph of agreement or dissent

### What the artifacts look like

After the debate, each worktree has:
- `IMPL.py` — the agent's proposed implementation
- `CRITIQUE.md` — their objections to the other two

The Minimalist's worktree also has:
- `PROPOSAL.md` — the team's final recommendation, with dissent noted

To read them during or after the debate:

```bash
# From the repo parent directory
cat ../talkhub-minimalist/IMPL.py
cat ../talkhub-defensive/CRITIQUE.md
cat ../talkhub-minimalist/PROPOSAL.md
```

---

## What to watch for

**Real pushback, not politeness.** The personas are instructed not to capitulate just to be agreeable. Look for `CRITIQUE.md` files that say "I disagree because..." with a specific reason, not "Great point, you're right."

**The Defensive Coder will raise edge cases the others missed.** Watch for them flagging things like: what if `items` contains `None`? What if two entries are the same string in different Unicode normalisation forms?

**The Perf Hawk will ask about complexity.** They may push back on the Minimalist's `dict.fromkeys()` approach or the Defensive Coder's `try/except` blocks.

**The PROPOSAL.md is more honest than what any one agent would have written.** It names the tradeoffs because it had to survive three perspectives. That's the value of the exercise.

**If they get stuck in a politeness loop:** sometimes agents will start agreeing too readily. If that happens, click into one pane and prompt them directly:

```
Be specific about your strongest objection. Don't capitulate yet.
```

---

## A note on `--dangerously-skip-permissions`

The script runs each Claude session with `--dangerously-skip-permissions`. This flag bypasses Claude's normal permission prompts — without it, every time an agent tries to read a sibling worktree (outside its own root), Claude would stop and ask. That kills the demo flow.

This is one of the textbook-safe uses of the flag:
- **Throwaway branches** — `debate-*` branches get deleted on cleanup, nothing ships from here
- **Deny rules in place** — `.claude/settings.json` in the repo blocks destructive operations (force push, `rm -rf`, `.env` writes)
- **You're watching live** — you're in the tmux session and can kill it instantly

If you adapt this pattern for non-demo work, drop the flag and use proper permission configuration instead.

---

## The manual path

If you'd rather see every step:

```bash
# Create a tmux session
tmux new -s debate

# Split into three panes: Ctrl-b %, then Ctrl-b "
# (or use the mouse after enabling mouse mode: tmux set-option mouse on)

# In each pane, create a worktree and open Claude with the persona
# Pane 1 — minimalist
git worktree add ../talkhub-minimalist debate-minimalist && cd ../talkhub-minimalist
claude "You are the Minimalist on a 3-agent debate team. Your role: defend the simplest correct implementation. Lines of code are a cost. Acknowledge the role in one sentence and wait for the problem."

# Pane 2 — defensive
git worktree add ../talkhub-defensive debate-defensive && cd ../talkhub-defensive
claude "You are the Defensive Coder on a 3-agent debate team. Your role: defend handling edge cases, input validation, and explicit error paths. Silent bugs are the worst bugs. Acknowledge the role in one sentence and wait for the problem."

# Pane 3 — perf hawk
git worktree add ../talkhub-perf debate-perf && cd ../talkhub-perf
claude "You are the Performance Hawk on a 3-agent debate team. Your role: defend runtime cost, memory, and big-O analysis. Don't ship code you haven't reasoned about. Acknowledge the role in one sentence and wait for the problem."
```

Wait for all three to acknowledge their roles, then paste the kickoff prompt into any one pane:

```
DEBATE BEGINS. Implement dedupe(items: list[str]) -> list[str] that removes
case-insensitive duplicates and preserves first-seen order.

(1) Write your version as IMPL.py in your current worktree, with comments
    explaining your role's design choices.
(2) Read the others' versions at ../talkhub-minimalist/IMPL.py,
    ../talkhub-defensive/IMPL.py, ../talkhub-perf/IMPL.py (skip your own).
(3) Write CRITIQUE.md in your worktree -- your sharpest, most specific
    objections to each of the others' choices.
(4) If you are the Minimalist, write PROPOSAL.md synthesizing the team's
    final recommendation including dissent where present. Otherwise, watch
    for ../talkhub-minimalist/PROPOSAL.md and append one paragraph of
    agreement or dissent to it.
Begin step 1 now.
```

**Manual cleanup:**

```bash
git worktree remove ../talkhub-minimalist
git worktree remove ../talkhub-defensive
git worktree remove ../talkhub-perf
tmux kill-session -t debate
git branch -D debate-minimalist debate-defensive debate-perf
```

---

## The takeaway

The disagreement is the value. A solo agent would pick one of the three approaches, write it confidently, and not surface the tradeoffs. The team is forced to make the tradeoffs explicit — and that's the artifact. `PROPOSAL.md` is more honest than what any one of them would have written alone.

The same pattern works beyond architecture debates: bug diagnosis (each agent starts from a different theory of where the bug lives), security review (offensive vs defensive mindset), or API design (consumer vs implementer perspective). Any time you want divergent priors on the same problem, this is the shape.
