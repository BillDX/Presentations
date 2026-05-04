# Demo 1 — Sub-Agents in Parallel

Two parts. Part A gets Claude to build a small target file. Part B spins up two sub-agents to work on that file in parallel — a reviewer and a documenter — entirely on the fly, no repo setup required.

**What this demonstrates:**
- Sub-agents run in isolated context windows with no crosstalk
- Tool restrictions (read-only vs. read-write) are enforced per agent
- Model routing — expensive model for nuanced review, cheap model for mechanical work
- Wall-clock time is roughly half the sequential equivalent

---

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- Python 3.x (for the generated file to be syntactically valid)
- Any working directory — no repo needed for this demo

---

## Part A — Make a target

Open Claude Code in any directory:

```bash
claude
```

Paste this prompt:

```
Create a quick CLI utility in Python that converts a text document to
lowercase. Call it lowerCaseMe.py.
```

Wait ~30 seconds. Skim the generated `lowerCaseMe.py` — we're about to send agents at it.

---

## Part B — Two agents, in parallel

In the **same Claude session**, paste:

```
Use two sub-agents on lowerCaseMe.py, in parallel:

1. A reviewer agent (sonnet, read-only: Read, Grep, Glob) -- find bugs, edge
   cases, and style problems.

2. A documenter agent (haiku, can edit) -- add docstrings to public functions.
```

**Watch for:** two agent boxes spinning at the same time in the Claude Code UI.

When they finish:
- Read the reviewer's findings. It should have flagged issues without touching the file.
- Check the diff on `lowerCaseMe.py` — the documenter should have added docstrings.

---

## What to notice

| | Reviewer | Documenter |
|---|---|---|
| Model | sonnet | haiku |
| Can write files? | No | Yes |
| Why | Nuanced judgment needed | Mechanical task, cheaper model fine |

The reviewer's read-only constraint isn't trust — it's enforcement. The `tools:` field in the agent definition makes it structurally impossible to write, regardless of what the prompt says.

---

## Persisting agents for reuse

The agents above were defined inline in the prompt — disposable, zero setup. To make them permanent and available to your whole team, save them as markdown files in `.claude/agents/`:

**`.claude/agents/reviewer.md`**
```markdown
---
name: reviewer
description: Reviews code for bugs, correctness issues, and style problems. Read-only — never edits.
tools: Read, Grep, Glob
model: sonnet
---

You are a senior code reviewer. When asked to review code:

1. Read the file(s) under review.
2. Identify issues grouped by severity: Critical, Style/Maintainability, Suggestion.
3. Quote the offending line(s).
4. Be specific — "this could be cleaner" is not a finding.

Do not propose full diffs. Describe changes in prose.
If the file looks fine, say so.
```

**`.claude/agents/documenter.md`**
```markdown
---
name: documenter
description: Adds Google-style docstrings to public functions and classes. Edits files in place.
tools: Read, Write, Edit, Glob
model: haiku
---

You add docstrings to Python code. That is your only job.

1. Read the target file.
2. For every public function or class without a docstring, write one (Google-style: Args, Returns, Raises).
3. Keep docstrings short — one or two sentences plus structured fields.
4. Edit the file in place.

Do not change signatures, behavior, or any non-docstring code.
Do not add docstrings to private helpers (names starting with `_`).
```

Once persisted, invoke them by name:

```
Use the reviewer and documenter agents on lowerCaseMe.py in parallel.
```

**Rule of thumb:** throwaway → inline. Used twice → persist it.

---

## Starter repo

The starter repo ships with pre-built versions of both agents. Clone it and run the exercise against `src/talkhub/routes/talks.py`:

```bash
git clone https://github.com/BillDX/spicy-to-dark-factory-starter
cd spicy-to-dark-factory-starter
uv sync
claude
```

Then:
```
Use the reviewer and documenter agents on src/talkhub/routes/talks.py in parallel.
```
