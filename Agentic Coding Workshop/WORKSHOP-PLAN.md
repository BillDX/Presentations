# Agentic Coding Workshop — Planning Document

**Format:** 60-minute interactive workshop
**Audience:** Developers who have coded before and used Claude Code at least a little
**Goal:** Move participants from L2 (pair programming) toward L3/L4 (delegation + orchestration) through hands-on exercises

---

## Workshop Structure

### Part 1: Leveling Up (10 min) — Presentation
Quick framing from the Agent Orchestration talk. No deep theory — just enough to set up the exercises.

| Slide | Content | Time |
|-------|---------|------|
| Title | Workshop intro, wifi/setup check | 1 min |
| The Five Levels | Where are you today? Quick audience poll | 2 min |
| The Single-Agent Ceiling | Why one agent breaks down | 2 min |
| The Time Bottleneck | Tokens are cheap, your time isn't | 2 min |
| What We'll Build Today | Overview of the three exercises | 3 min |

### Part 2: Exercise 1 — Custom Sub-Agents (15 min) — Hands-On
**Goal:** Create and use a custom sub-agent definition

**Setup:** Participants clone a starter repo (or use their own project)

**Exercise steps:**
1. Create `.claude/agents/reviewer.md` — a read-only code reviewer agent
2. Create `.claude/agents/documenter.md` — generates docstrings/comments
3. Prompt Claude to use both agents on a file in the project
4. Observe: agents run in parallel, each with isolated context

**Key concepts covered:**
- Agent definition files (frontmatter: name, description, tools, model)
- Tool restrictions (read-only vs full access)
- Model routing (opus for planning, sonnet/haiku for grunt work)
- How sub-agents report results back to the parent

**Debrief:** 2 min group discussion — What did the reviewer catch? How did isolation help?

### Part 3: Exercise 2 — The Controller Pattern (15 min) — Hands-On
**Goal:** Build a multi-agent pipeline using the controller pattern

**Exercise steps:**
1. Launch Claude with `--worktree controller`
2. Write a prompt that spawns a pipeline:
   - Agent 1 (planner): Read a spec file and produce an implementation plan
   - Agent 2 (coder): Implement the plan in a worktree
   - Agent 3 (tester): Write and run tests against the implementation
3. Watch the pipeline execute — agents work in their own worktrees
4. Review the results: plan, code diff, test output

**Key concepts covered:**
- Worktree isolation (no file collisions)
- Sequential vs parallel agent execution
- The parent as orchestrator — collecting and routing results
- Cost implications: when to use opus vs sonnet vs haiku

**Debrief:** 3 min — What surprised you? Where did agents struggle?

### Part 4: Exercise 3 — Agent Teams in tmux (10 min) — Guided Demo + Try It
**Goal:** See peer-to-peer agent collaboration in action

**Format:** Instructor does a live demo first (5 min), then participants try a simplified version (5 min)

**Demo steps:**
1. Open tmux with 3 panes
2. Launch a lead agent + 2 teammates
3. Give the lead a task that requires coordination
4. Watch agents message each other directly

**Participant try-it:**
- Launch a 2-agent team on a simple task (e.g., one agent writes a function, the other writes tests for it)

**Key concepts covered:**
- Agent teams vs sub-agents (peer messaging vs hub-and-spoke)
- When to use teams vs sub-agents (exploration vs execution)
- The experimental nature of teams — failure modes and workarounds

### Part 5: Choosing Your Weapon + Q&A (10 min) — Discussion
**Content:**
- Decision tree: when to use single agent / sub-agents / agent teams
- Git worktrees as the isolation primitive
- Token cost reality check
- Cheat sheet slide
- Open Q&A

---

## Pre-Workshop Setup Requirements

Participants need:
- [ ] Claude Code CLI installed and authenticated
- [ ] A GitHub account with a repo they can experiment on (or use the starter repo)
- [ ] tmux installed (`brew install tmux` on macOS)
- [ ] Git configured
- [ ] Terminal with at least 3 panes/tabs capability

**Starter repo** should contain:
- A small project with 3-4 files (e.g., a simple API with routes, models, utils)
- A `SPEC.md` with a feature to implement (for Exercise 2)
- Pre-written `.claude/agents/` examples they can reference
- A `EXERCISES.md` with step-by-step instructions for each exercise

---

## Timing Budget

| Section | Duration | Cumulative |
|---------|----------|------------|
| Part 1: Leveling Up (slides) | 10 min | 10 min |
| Part 2: Exercise 1 — Custom Sub-Agents | 15 min | 25 min |
| Part 3: Exercise 2 — Controller Pattern | 15 min | 40 min |
| Part 4: Exercise 3 — Agent Teams (demo + try) | 10 min | 50 min |
| Part 5: Decision Tree + Q&A | 10 min | 60 min |

**Buffer:** Exercises 2 and 3 can be shortened if Exercise 1 runs long. Exercise 3 can be demo-only if time is tight.

---

## Facilitation Notes

- **Pace check after Exercise 1** — if most people are still setting up, extend and shorten Exercise 3 to demo-only
- **Have a "fast track"** — participants who finish early can try bonus challenges:
  - Add a `security-reviewer` agent that checks for OWASP issues
  - Create a TDD team with red/green agents
  - Build a "bug tribunal" pattern from the original talk
- **Common failure modes to watch for:**
  - Claude Code not authenticated — have them run `claude auth` first
  - Worktree conflicts if they have uncommitted changes — remind them to commit or stash
  - tmux unfamiliarity — have a tmux cheat sheet ready
  - Token limits on free tier — have a plan for pair-coding if needed
